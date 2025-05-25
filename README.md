# TP2 – Tests Unitaires & Mockups

##  Objectifs

Ce TP avait pour but de :
- Comprendre l’intérêt des tests unitaires et leur rôle dans le cycle de vie logiciel.
- Apprendre à isoler les composants à tester avec des **mockups**.
- Utiliser **JUnit5** et **Mockito** pour réaliser des tests d’état et d’interactions.
- Structurer les scénarios de test en classes d'équivalence réalistes.

---

---

## Exercice 1 – Tests de base avec Mockito

### Objectif :
Tester la méthode `additionner()` et vérifier :
- le résultat retourné
- l’appel à la méthode
- l’état interne via `getResult()`

### Type de test :
- Test d'état (sur le résultat)
- Test d'interaction (via `verify()`)

---

## Exercice 2 – Mock d’un service externe

### Objectif :
Tester l'appel d’un service API externe (`UtilisateurApi`) via `UserService`.

### Méthodes testées :
- `creerUtilisateur(Utilisateur utilisateur)`

### Test réalisé :
- Vérifie que `creerUtilisateur()` est bien appelé avec les bons arguments.

---

## Exercice 3 – Scénarios API

###  Scénarios testés :

1. **Exception API**
   - Lève une `ServiceException` si l’API échoue.

2.  **Erreur de validation**
   - Le service ne doit **pas appeler** l’API si les données sont invalides.

3.  **Retour ID utilisateur**
   - L'API renvoie un ID fictif `123`, et le test vérifie l'état de l’objet.

4.  **Vérification des arguments**
   - Utilisation de `ArgumentCaptor` pour capturer et vérifier les champs.

---

##  Exercice 4 – Jeu de dés (logique métier complexe)

###  1. Objets à mocker :
- `Joueur`, `De`, `Banque`
- Ils sont **mockés** car :
  - Ce sont des dépendances de `Jeu`
  - Ils représentent des composants externes (I/O, hasard)

---

### 2. Scénarios testés (classes d’équivalence) :

| # | Description                                   | Type de test      |
|---|-----------------------------------------------|--------------------|
| 1 | Jeu fermé → exception                         | Test d'état        |
| 2 | Joueur insolvable → aucun lancement de dés    | Test d'interaction |
| 3 | Somme ≠ 7 → perte                             | État + interaction |
| 4 | Somme = 7 → gain                              | État + interaction |
| 5 | Gain + banque non solvable → jeu fermé        | État               |

---

###  3. Implémentation `Jeu`

La méthode `jouer()` applique les règles :
- Vérifie l'ouverture du jeu
- Demande la mise et débite le joueur
- Lance les dés
- Si somme == 7, paie 2x la mise
- Si banque devient insolvable, ferme le jeu

---

### 4. Cas : jeu fermé

Test d’état : on s'attend à ce qu’une exception `JeuFermeException` soit levée si le jeu est fermé.

---

###  5. Cas : joueur insolvable

Test d’interaction : on vérifie que **les dés ne sont jamais appelés** si le joueur ne peut pas être débité (`verifyNoInteractions(de1, de2)`).

---

### 6. Tests complémentaires

- Victoire : vérifie les débits/crédits.
- Fermeture du jeu automatique si la banque n’est plus solvable après un gain.

---

### 7. Banque intégrée

Une implémentation réelle de `BanqueImpl` a été utilisée pour simuler une perte de solvabilité :



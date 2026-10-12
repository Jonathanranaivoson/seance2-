# TP2 — Des animations qui informent

**Animation Web · L3 Informatique · ITUniversity**
**Séance 2 — @keyframes et courbes de timing · Durée : 1 h 20**

---

## Objectifs

À la fin de ce TP, vous saurez :

1. écrire une animation `@keyframes` avec `from`/`to` et des pourcentages ; [x]
2. régler une animation avec les propriétés `animation-*` et le raccourci `animation` ;
3. décaler plusieurs animations avec `animation-delay` et `calc()` ;
4. comprendre et régler `animation-fill-mode` ;
5. éviter le piège du `transform` écrasé ;
6. *(bonus)* utiliser `steps()`.

## Démarrage

1. Copiez le dossier `starter/` dans votre dépôt Git, sous le nom `seance2/`.
2. Ouvrez `seance2/index.html` dans le navigateur, et `style.css` dans votre éditeur.
3. **Vous ne modifiez que `style.css`**, aux endroits marqués `TODO`.
4. Le fichier HTML contient un peu de JavaScript **fourni** (rejouer une animation, vérifier un mot de passe). Ne le modifiez pas, sauf à l'étape 3.3. Nous apprendrons à l'écrire en séance 3.

Astuce DevTools : l'onglet **Animations** (menu ⋮ > More tools > Animations dans Chrome) permet de ralentir et de rejouer les animations de la page.

| Niveau | Contenu | Durée indicative |
|---|---|---|
| 1 | Loaders | 20 min |
| 2 | Apparition | 25 min |
| 3 | Erreur de saisie | 20 min |
| Bonus | Machine à écrire | 15 min |

---

## Niveau 1 — Loaders

Un loader signale une attente. C'est le seul cas où une animation `infinite` est justifiée.

### Étape 1.1 — Écrire le mouvement

Écrivez `@keyframes tourner`, qui fait passer l'élément de `rotate(0deg)` à `rotate(360deg)` avec `transform`.

### Étape 1.2 — L'appliquer

Dans `.spinner`, appliquez l'animation `tourner` en une ligne avec le raccourci `animation` : durée 1 s, rythme `linear`, répétition infinie.

> **Question 1.** Remplacez `linear` par `ease-in-out`. Que voyez-vous ? Pourquoi `linear` est-il le bon choix pour une rotation continue ?

### Étape 1.3 — Le piège du transform

Regardez bien : le spinner tourne-t-il **à sa place**, au centre de sa boîte ?

> **Question 2.** Expliquez ce qui se passe. Indice : regardez la propriété `transform` dans la règle `.spinner`, puis celle de vos keyframes.

Corrigez le problème **sans toucher à vos keyframes**. Regardez comment les trois points (`.dots`) sont centrés, juste en dessous.

### Étape 1.4 — Les trois points : le mouvement

Écrivez `@keyframes rebondir` :
- à `0%`, `80%` et `100%` : position normale (`translateY(0)`), opacité `0.4` ;
- à `40%` : `translateY(-10px)`, opacité `1`.

> 💡 Plusieurs pourcentages qui partagent le même état s'écrivent ensemble, séparés par des virgules.

### Étape 1.5 — L'appliquer

Appliquez `rebondir` aux `span` : 0.9 s, `ease-in-out`, infini.

Les trois points bougent ensemble, comme un seul bloc. Ce n'est pas l'effet voulu.

### Étape 1.6 — Décaler les départs

Avec `:nth-child()`, donnez un `animation-delay` de `0.15s` au 2e point et de `0.3s` au 3e.

> **Question 3.** Pourquoi le décalage donne-t-il l'impression d'une « vague » ?

✅ **Checkpoint niveau 1**
- [x] Le spinner tourne régulièrement, **au centre** de sa boîte
- [x] Les trois points rebondissent l'un après l'autre
- [x] Questions 1 à 3 répondues (en commentaire dans `style.css`)

---

## Niveau 2 — Apparition

Au chargement, les quatre cartes doivent apparaître l'une après l'autre, en fondu et en remontant légèrement.

### Étape 2.1 — Le mouvement

Écrivez `@keyframes apparaitre` avec **seulement un `from`** : `opacity: 0` et `transform: translateY(20px)`.

> 💡 Sans `to`, l'animation se termine sur l'état normal de l'élément. C'est pratique : on ne décrit que le point de départ.

### Étape 2.2 — L'appliquer

Écrivez une règle `.is-playing .reveal` qui applique `apparaitre` : 0.5 s, `ease-out`.

> **Question 4.** Pourquoi `ease-out` et pas `ease-in` pour un élément qui **arrive** ?
 

Cliquez sur « Rejouer l'apparition » : les cartes apparaissent toutes en même temps.

### Étape 2.3 — Décaler avec calc()

Chaque carte a une variable `--i` dans le HTML (0, 1, 2, 3). Donnez à chaque carte un délai de **100 ms multiplié par son rang** :

```css
animation-delay: calc(var(--i) * 100ms);
```

### Étape 2.4 — L'expérience du fill-mode

Rejouez l'apparition et regardez **la dernière carte** pendant son délai.

1. Sans `animation-fill-mode` : que voyez-vous juste avant qu'elle apparaisse ?
2. Ajoutez `animation-fill-mode: forwards;` : le problème est-il réglé ?
3. Essayez `backwards`, puis `both`.

> **Question 5.** Expliquez le rôle de `backwards`. Quelle valeur choisissez-vous, et pourquoi ?

✅ **Checkpoint niveau 2**
- [x] Les cartes apparaissent l'une après l'autre, en fondu
- [x] Aucune carte ne « clignote » avant son apparition
- [x] Questions 4 et 5 répondues

---

## Niveau 3 — Erreur de saisie

Quand le mot de passe est faux, le champ doit devenir rouge et secouer « non ». Le JavaScript fourni ajoute la classe `.is-error` sur `.field`.

### Étape 3.1 — Le mouvement

Écrivez `@keyframes secouer` :
- `0%, 100%` : `translateX(0)` ;
- `20%, 60%` : `translateX(-8px)` ;
- `40%, 80%` : `translateX(8px)`.

### Étape 3.2 — L'appliquer

Écrivez une règle `.field.is-error input` qui :
- passe la bordure en rouge (`var(--red)`) ;
- joue `secouer` une seule fois : 0.4 s, `ease-in-out`.

Testez avec un mauvais mot de passe, **deux fois de suite**. Le champ secoue-t-il à chaque fois ?

### Étape 3.3 — Comprendre la relance

Dans le JavaScript, remplacez **temporairement** la ligne :

```js
restart(field, 'is-error');
```

par :

```js
field.classList.add('is-error');
```

Testez deux mauvais mots de passe de suite.

> **Question 6.** Pourquoi le champ ne secoue-t-il plus à la deuxième erreur ? Que fait la fonction `restart()` ?

Remettez la ligne d'origine.

✅ **Checkpoint niveau 3**
- [x] Le champ devient rouge et secoue à **chaque** erreur
- [x] Rien ne secoue quand le mot de passe est correct (`itu2026`)
- [ ] Question 6 répondue

---

## Bonus — Machine à écrire

Le texte « Bienvenue à ITUniversity » doit s'afficher lettre par lettre, avec un curseur qui clignote.

1. **B.1** — Dans `.typewriter`, supprimez `width: max-content`, mettez `width: 0` et ajoutez un curseur : `border-right: 3px solid var(--navy);`
2. **B.3** — Écrivez `@keyframes taper`, qui amène la largeur à `24ch`.
3. **B.4** — Écrivez `@keyframes clignoter`, où la bordure devient `transparent` à `50%`.
4. **B.2** — Appliquez les **deux** animations, séparées par une virgule :
   - `taper` : 2.4 s, `steps(24)`, avec `forwards` pour garder le texte affiché ;
   - `clignoter` : 0.8 s, `steps(1)`, infini.

> **Question 7.** Pourquoi `24ch` et `steps(24)` ? Que se passe-t-il avec `linear` au lieu de `steps(24)` ?

---

## Livrable

- Dossier `seance2/` dans votre dépôt Git, avec `index.html` et `style.css` complétés.
- Réponses aux questions **en commentaire** en haut de `style.css`.

```bash
git add seance2/
git commit -m "Séance 2 : animations keyframes"
git push
```

## Critères d'évaluation (10 % de la note du module)

| Critère | Points |
|---|---|
| Niveau 1 : spinner centré et trois points décalés | 3 |
| Niveau 2 : apparition décalée, sans clignotement | 3 |
| Niveau 3 : secousse à chaque erreur | 2 |
| Réponses aux questions, justes et argumentées | 2 |
| *Bonus : machine à écrire fonctionnelle* | *+1* |
| **Total** | **10** |

## Pour aller plus loin

- MDN — Utiliser les animations CSS : <https://developer.mozilla.org/fr/docs/Web/CSS/CSS_animations/Using_CSS_animations>
- MDN — `animation-fill-mode` : <https://developer.mozilla.org/fr/docs/Web/CSS/animation-fill-mode>
- Tester des courbes : <https://cubic-bezier.com>

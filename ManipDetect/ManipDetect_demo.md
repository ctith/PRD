# ManipDetect — Kit de démo (6 cas utilisateurs)

Objectif : couvrir tout le spectre pour la présentation finale. Lance chaque cas, capture le résultat, et présente la galerie à la fin.

**Ce que la batterie de cas prouve :**

| # | Cas | Entrée | Ce que ça démontre | Résultat attendu |
|---|-----|--------|--------------------|------------------|
| 1 | Pub (texte) | texte collé | Détection riche, cœur du produit | Intensité **élevée / systémique** (5-7 techniques) |
| 2 | Article (texte) | texte collé | Ne crie pas au loup sur du factuel | **Faible** (0-1 technique) |
| 3 | Phrase banale (texte) | texte collé | État vide propre | **Aucune technique** |
| 4 | Webinaire (URL) | URL scrapable | Scraping + analyse de bout en bout | **Modérée / élevée** |
| 5 | Fnac (URL bloquée) | URL protégée bot | Garde-fou « page bloquée » | Message **« collez le texte »** |
| 6 | Info neutre (URL) | URL scrapable sans pub | Scraping OK + pas de fausse alerte | **Faible / aucune** |

> Astuce : pour la démo *live*, fais le **cas 1** (le plus spectaculaire). Présente les **cas 2 à 6 en screenshots** à la fin pour montrer la palette.

---

## CAS 1 — Texte publicitaire (golden path)

**Onglet « Texte libre » → coller :**

```
🔥 DERNIÈRE CHANCE — La cohorte ferme dans 48 h.

Rejoins l'Académie Momentum et deviens libre financièrement grâce à l'IA.

Il ne reste que 9 places sur 12 pour la promotion de juin. Une fois la cohorte
complète, les inscriptions ferment définitivement.

Valeur réelle du programme : 4 500 € — aujourd'hui, seulement 990 € si tu
t'inscris avant dimanche minuit. Après, le tarif repasse à 2 500 €.

Déjà plus de 3 000 élèves accompagnés et 12 M€ de chiffre d'affaires généré.
Notre méthode est validée par les plus grands experts du secteur.

« Ça a littéralement changé ma vie, je ne pensais pas que c'était possible ! »
— Julie, entrepreneure

« Le meilleur investissement de toute ma vie. Foncez, il n'y a aucun risque. »
— Marc, ancien salarié

Tu fais déjà partie des 1 % qui sont allés au bout de cette page : tu n'es pas
comme les autres. Ceux qui hésitent encore resteront exactement où ils sont.
Alors, tu choisis quoi ?

👉 JE RÉSERVE MA PLACE MAINTENANT
```

**Résultat attendu :** intensité **élevée à systémique**. Techniques visées :
Rareté artificielle (places limitées), Fausse urgence (compte à rebours / hausse de tarif), Ancrage de prix (4 500 € barré), Preuve sociale artificielle (chiffres flous), Appel à l'autorité implicite (« experts du secteur »), Faux témoignages (génériques, sans résultat mesurable), Flatterie instrumentale (« les 1 % »), Culpabilisation / double bind (« ceux qui hésitent resteront où ils sont »).

**Légende screenshot :** *« Page de vente : 6+ techniques combinées → intensité systémique. »*

---

## CAS 2 — Texte d'article (factuel, neutre)

**Onglet « Texte libre » → coller :**

```
La médiathèque municipale rouvrira ses portes le 15 septembre après six mois
de travaux. La rénovation a porté sur l'isolation thermique du bâtiment, le
remplacement de l'éclairage et la mise en accessibilité de l'étage.

Les horaires restent inchangés : du mardi au samedi, de 10 h à 18 h. La carte
d'adhérent demeure gratuite pour les habitants de la commune. Le fonds compte
environ 35 000 ouvrages, complétés par un espace numérique de douze postes.

La municipalité précise que le budget des travaux, voté en 2024, s'élève à
1,2 million d'euros, financé pour moitié par une subvention régionale.
```

**Résultat attendu :** intensité **faible**, 0 à 1 technique (éventuellement un faux positif léger). C'est le cas qui montre que l'outil **ne sur-détecte pas** sur du contenu informatif.

**Légende screenshot :** *« Article factuel : aucune (ou quasi) technique → l'outil ne crie pas au loup. »*

---

## CAS 3 — Phrase banale (état vide)

**Onglet « Texte libre » → coller :**

```
On se retrouve à 13 h devant la gare pour déjeuner, je réserve une table pour quatre.
```

**Résultat attendu :** **aucune technique détectée** → carte verte « Aucune technique de manipulation détectée ». Montre la gestion propre de l'état vide.

**Légende screenshot :** *« Message anodin : état vide géré proprement. »*

---

## CAS 4 — URL de webinaire (scraping + analyse)

**Onglet « URL » → soumettre l'une de ces pages** (ce sont des landings de webinaire/masterclass de ta liste de tests, riches en techniques) :

- `https://www.indexmasterclass.com/webinar-registration`
- Secours : `https://go.ambroisedebret.com/inscription-masterclass`
- Secours : `https://formation.revolia.pro/inscription-masterclass-ia-consultant`

**Résultat attendu :** intensité **modérée à élevée** — typiquement rareté, urgence, preuve sociale, autorité. Démontre le **chemin URL complet** : scraping Jina → analyse → rapport.

**Légende screenshot :** *« Page web réelle scrapée et analysée automatiquement. »*

> ⚠️ Teste la veille : ces pages changent. Garde celle qui scrape bien et donne un beau résultat.

---

## CAS 5 — URL bloquée par protection anti-robot (garde-fou)

**Onglet « URL » → soumettre une page de gros e-commerce protégée** (Fnac utilise une protection anti-bot type DataDome) :

- `https://www.fnac.com/` (ou n'importe quelle fiche produit Fnac)
- Secours si la Fnac passe : `https://www.vinted.fr/`, `https://www.leboncoin.fr/`, `https://www.ticketmaster.fr/`

**Résultat attendu :** le scraping renvoie une page de vérification → le **garde-fou se déclenche** et l'utilisateur voit le message *« Je n'ai pas pu lire automatiquement cette page… copiez-collez le texte visible »* — **pas** une fausse analyse.

**Légende screenshot :** *« Site protégé anti-robot : l'outil refuse d'inventer et demande le texte. »*

> C'est un screenshot clé : il prouve que le produit **échoue proprement** plutôt que de produire une analyse bidon.

---

## CAS 6 — URL d'info neutre, sans contenu publicitaire (pas de fausse alerte)

**Onglet « URL » → soumettre une page informative qui scrape bien et ne vend rien** :

- `https://fr.wikipedia.org/wiki/Orphée` (Wikipédia scrape de façon fiable et n'a aucun marketing)
- Alternative info : un article de presse factuel, ex. une brève de `https://www.francetvinfo.fr/` ou une page `https://www.service-public.fr/`

**Résultat attendu :** la page est lue sans problème, mais l'analyse renvoie **faible / aucune technique**. Montre que l'outil **distingue le contenu informatif du contenu manipulatoire** — il ne se déclenche pas juste parce que c'est une URL.

**Légende screenshot :** *« Page d'information neutre : scrapée, analysée, et jugée sans manipulation. »*

---

## Pour la présentation finale

Dispose les 6 captures en **grille 2×3**, dans cet ordre de lecture :

1. Pub (systémique) → 2. Webinaire (élevée) → 3. Article (faible)
4. Phrase banale (vide) → 5. Fnac (bloquée) → 6. Info neutre (faible)

Le message à dire en montrant la grille :

> « Le même outil, six entrées différentes. À gauche, il détecte finement quand la manipulation est là. À droite, il reste sobre sur du contenu neutre, et surtout — ici — il refuse d'analyser une page qu'il n'a pas vraiment pu lire, au lieu d'inventer un résultat. C'est cette robustesse qui le rend fiable. »

---

## Note importante

L'analyse est générée par IA : les scores et le détail des techniques peuvent **varier légèrement d'un essai à l'autre**. Capture chaque résultat une fois que tu en as un qui illustre bien le cas — c'est exactement ce que ferait une vraie démo. Les cas 1, 3, 5 sont les plus stables ; les cas 2, 4, 6 peuvent osciller d'une technique ou deux.

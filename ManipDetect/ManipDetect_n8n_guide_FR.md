# ManipDetect — Guide technique n8n
### Construire le workflow pas à pas

![Stack](https://img.shields.io/badge/stack-n8n%20%7C%20Airtable%20%7C%20Jina%20%7C%20LLM-blue)
![Durée](https://img.shields.io/badge/durée%20estimée-2--3h-orange)
![Niveau](https://img.shields.io/badge/niveau-débutant%20no--code-green)

> Ce guide suppose que Lovable a déjà généré l'interface utilisateur.
> L'objectif ici est de construire le cerveau du produit : le workflow n8n
> qui reçoit le texte ou l'URL, l'analyse, et retourne le rapport de manipulation.

---

## Ce qu'on va construire

```
[Webhook] → [IF: text ou url?]
                ↓ url          ↓ text
           [Jina scrape]    [passe direct]
                ↓                ↓
           [Merge les deux branches]
                ↓
           [Code: normalise le texte]
                ↓
           [Airtable: fetch taxonomie]
                ↓
           [LLM: analyse + JSON]
                ↓
           [Respond to Webhook]
```

**7 nœuds. Aucune ligne de code serveur. Pas de Supabase requis.**

---

## Comptes à créer avant de commencer

| Outil | Rôle | Coût | Lien |
|---|---|---|---|
| **n8n cloud** | Héberger le workflow | Gratuit (starter) | n8n.io |
| **Airtable** | Stocker les 18 techniques | Gratuit | airtable.com |
| **OpenAI** ou **Anthropic** | Appel LLM | ~$5 de crédit suffisent | platform.openai.com ou console.anthropic.com |
| **Jina.ai** | Scraper les URLs | Gratuit, sans compte | r.jina.ai |
| **Lovable** | Interface utilisateur | Gratuit (plan de base) | lovable.dev |

> Supabase : **non requis pour le MVP.** Utile seulement si tu veux stocker
> l'historique des analyses — feature V1.

---

## ÉTAPE 0 — Préparer Airtable

Avant de toucher à n8n, il faut que la taxonomie soit prête dans Airtable.
Le workflow va la chercher à chaque analyse.

### 0.1 Créer la base

1. Va sur **airtable.com** → connecte-toi
2. Clique **"Add a base"** → **"Start from scratch"**
3. Nomme la base : `ManipDetect`

### 0.2 Créer la table

1. Dans la base, renomme la table par défaut en `Manipulation_Taxonomy`
2. Crée ces 6 colonnes (toutes en type **"Long text"**) :

| Nom de colonne | Type |
|---|---|
| `Nom` | Single line text |
| `Definition` | Long text |
| `Mecanisme` | Long text |
| `Signaux` | Long text |
| `Exemple` | Long text |
| `Suggestion` | Long text |

### 0.3 Remplir les 18 lignes

Copie-colle chaque ligne ci-dessous dans Airtable.

---

#### Techniques interpersonnelles (12)

**#01 — Fausse urgence**
- Définition : Créer une pression temporelle artificielle pour empêcher la réflexion et forcer une décision rapide.
- Mécanisme : Court-circuite l'analyse rationnelle en activant la peur de manquer quelque chose ou de subir une conséquence.
- Signaux : "il faut décider maintenant", "c'est ma dernière offre", "tu as jusqu'à ce soir", "on ne peut pas attendre", "le délai expire"
- Exemple : "J'ai besoin d'une réponse aujourd'hui, sinon je dois proposer le poste à quelqu'un d'autre."
- Suggestion : Nommer la pression et ralentir — "Je prends le temps qu'il me faut pour décider correctement."

**#02 — Culpabilisation**
- Définition : Attribuer à l'interlocuteur la responsabilité d'une situation négative pour le placer en position de dette morale.
- Mécanisme : Exploite le besoin d'approbation et la peur du rejet — la personne culpabilisée cherche à réparer même si elle n'est pas en tort.
- Signaux : "à cause de toi", "tu m'as déçu", "je n'aurais pas pensé que tu ferais ça", "après tout ce que j'ai fait", "c'est ta faute si"
- Exemple : "C'est à cause de ton refus qu'on en est là. J'espère que tu es fier de toi."
- Suggestion : Séparer les faits de l'interprétation — questionner la causalité présentée comme évidente.

**#03 — DARVO**
- Définition : Deny (nier), Attack (attaquer), Reverse Victim and Offender — l'agresseur se présente comme la victime quand il est mis en cause.
- Mécanisme : Déstabilise l'interlocuteur qui se retrouve à devoir se justifier d'avoir osé soulever un problème.
- Signaux : "je n'ai jamais dit ça", "tu m'attaques", "c'est toi qui me fais du mal", "je suis la victime ici", "tu es injuste avec moi"
- Exemple : "Je n'ai rien fait de mal. C'est toi qui m'accuses sans preuve — tu me fais énormément de mal en disant ça."
- Suggestion : Revenir aux faits concrets et documentés, ne pas répondre à la contre-accusation.

**#04 — Double bind**
- Définition : Placer l'interlocuteur devant deux options dont aucune n'est favorable — l'illusion du choix masque l'absence de bonne issue.
- Mécanisme : Génère une paralysie ou une résignation — toute réponse est perdante, ce qui renforce le sentiment d'impuissance.
- Signaux : "dans tous les cas", "quoi que tu fasses", deux alternatives présentées comme exhaustives sans tierce option
- Exemple : "Si tu acceptes, tu montres que tu ne tiens pas à tes principes. Si tu refuses, tu n'es pas un joueur d'équipe."
- Suggestion : Refuser le cadre — "Je ne reconnais pas ces deux options comme les seules possibles."

**#05 — Inversion de responsabilité**
- Définition : Retourner la charge de la preuve sur celui qui a subi, comme s'il était responsable de ce qui lui est arrivé.
- Mécanisme : Décharge l'émetteur de toute responsabilité tout en maintenant l'autre en position d'acteur obligé.
- Signaux : "tu n'avais qu'à", "si tu avais fait attention", "c'est à toi de gérer", "tu l'as bien cherché", "tu aurais dû prévoir"
- Exemple : "Si tu avais mieux communiqué dès le départ, on n'en serait pas là. C'est à toi de trouver une solution."
- Suggestion : Nommer l'inversion — "Je note que la responsabilité m'est attribuée sans examen des faits."

**#06 — Ambiguïté délibérée**
- Définition : Formuler volontairement un message flou pour garder une porte de sortie tout en créant une fausse impression.
- Mécanisme : Permet à l'émetteur de se dédire tout en ayant créé une attente ou une pression.
- Signaux : "dans un sens oui", "c'est possible", "on verra", "ça dépend", promesses sans engagement clair ni date ni critère
- Exemple : "On verra pour ta promotion, les choses évoluent dans le bon sens."
- Suggestion : Demander une reformulation explicite et écrite — "Peux-tu préciser ce que ça signifie concrètement ?"

**#07 — Appel à l'autorité implicite**
- Définition : Invoquer une autorité non nommée pour donner du poids à une position discutable et inhiber la contestation.
- Mécanisme : Remettre en question le message revient à s'opposer à une autorité floue mais intimidante.
- Signaux : "tout le monde sait que", "la direction a décidé", "c'est la norme", "les experts s'accordent", "on m'a dit que"
- Exemple : "La direction est très préoccupée par ton attitude — plusieurs personnes ont remonté des choses."
- Suggestion : Demander à nommer l'autorité et les faits précis — "Qui exactement, et quels faits ?"

**#08 — Minimisation**
- Définition : Réduire la légitimité d'un ressenti ou d'un problème pour éviter d'avoir à le traiter.
- Mécanisme : Invalide l'expérience de l'autre — la personne finit par douter de la légitimité de sa propre perception.
- Signaux : "tu exagères", "c'est rien", "tu es trop sensible", "tout le monde vit ça", "tu dramatises", "ce n'est pas si grave"
- Exemple : "Tu te plains pour rien — tout le monde a des objectifs difficiles."
- Suggestion : Maintenir sa perception sans justification excessive — "Mon ressenti est réel, qu'il soit partagé ou non."

**#09 — Gaslighting textuel**
- Définition : Nier ou réécrire des faits passés pour faire douter l'interlocuteur de sa propre mémoire ou perception.
- Mécanisme : Crée une instabilité cognitive — la personne dépense son énergie à valider la réalité plutôt qu'à répondre au fond.
- Signaux : "je n'ai jamais dit ça", "tu rêves", "tu inventes", "tu te souviens mal", "ça ne s'est pas passé comme ça"
- Exemple : "Je ne t'ai jamais promis une augmentation. Tu as dû mal comprendre."
- Suggestion : S'appuyer sur des preuves écrites — emails, comptes-rendus. Ne pas argumenter sur la mémoire.

**#10 — Flatterie instrumentale**
- Définition : Complimenter de manière ciblée avant une demande pour créer une obligation de réciprocité.
- Mécanisme : Exploite la règle de réciprocité — après avoir reçu un compliment, on se sent redevable.
- Signaux : "toi qui es si compétent", "je savais que je pouvais compter sur toi", "tu es la seule personne capable de", compliment excessif avant demande
- Exemple : "Tu es vraiment la meilleure dans ce domaine — j'ai besoin que tu gères ce dossier urgent ce week-end."
- Suggestion : Séparer le compliment de la demande — évaluer la demande sur ses mérites propres.

**#11 — Projection**
- Définition : Attribuer à l'autre ses propres intentions, sentiments ou comportements problématiques.
- Mécanisme : Déplace l'attention du comportement réel de l'émetteur vers un comportement supposé du récepteur.
- Signaux : "tu cherches à me manipuler", "tu es agressif", "tu mens", "tu veux me faire du mal", accusations miroir
- Exemple : "Je vois très bien que tu essaies de me manipuler avec tes arguments."
- Suggestion : Ne pas répondre à l'accusation miroir — revenir aux faits observables et documentés.

**#12 — Faux consensus**
- Définition : Présenter sa position comme partagée par une majorité silencieuse pour isoler l'interlocuteur.
- Mécanisme : Active la peur de l'exclusion sociale — s'opposer revient à être seul contre tous.
- Signaux : "tout le monde pense que", "personne n'a compris pourquoi tu", "l'équipe est d'accord pour dire", sans nommer les personnes
- Exemple : "Franchement, tout le monde dans l'équipe trouve que tu es difficile. Je suis le seul à encore te défendre."
- Suggestion : Demander à identifier "tout le monde" — les consensus anonymes n'existent pas.

---

#### Techniques marketing / copywriting (3)

**#13 — Preuve sociale artificielle**
- Définition : Présenter des chiffres ou témoignages invérifiables pour simuler une validation par le nombre.
- Mécanisme : Inhibe l'esprit critique — si des milliers de personnes ont fait ce choix, remettre en question semble irrationnel.
- Signaux : chiffres ronds non sourcés, "plus de X clients satisfaits", témoignages sans identité vérifiable, logos presse décontextualisés
- Exemple : "Plus de 10 000 entrepreneurs ont transformé leur vie grâce à cette méthode."
- Suggestion : Demander la source, la date, la méthodologie du chiffre avancé.

**#14 — Rareté artificielle**
- Définition : Créer une fausse impression de limitation (stock, places, temps) pour déclencher une décision rapide.
- Mécanisme : Combine fausse urgence et fausse exclusivité — ce qui est rare est perçu comme précieux.
- Signaux : "plus que X places", compteurs de temps, "édition limitée", "offre valable jusqu'à minuit", stocks affichés décroissants
- Exemple : "Il ne reste que 3 places pour cette formation. L'offre expire dans 47 minutes."
- Suggestion : Vérifier si la rareté est réelle en actualisant la page le lendemain.

**#15 — Ancrage de prix**
- Définition : Afficher un prix de référence élevé barré pour faire paraître le prix réel comme une bonne affaire.
- Mécanisme : Le cerveau évalue le prix réel par rapport à l'ancre affichée, pas par rapport à la valeur intrinsèque.
- Signaux : prix barrés, "valeur réelle : X€", "économisez X%", comparaison avant/après prix, "au lieu de"
- Exemple : "Formation en ligne : ~~2 497€~~ Aujourd'hui seulement : 197€. Économisez 92%."
- Suggestion : Évaluer le prix affiché par rapport à la valeur réelle et aux alternatives du marché.

---

#### Techniques dropshipping / e-commerce (3)

**#16 — Faux témoignages**
- Définition : Présenter des avis clients fabriqués ou achetés pour simuler une validation sociale inexistante.
- Mécanisme : Exploite la preuve sociale — des dizaines d'avis positifs court-circuitent le jugement personnel.
- Signaux : photos de profil génériques, prénoms anglophones sur site français, notes exclusivement 5 étoiles, dates groupées, texte trop parfait
- Exemple : "⭐⭐⭐⭐⭐ Incroyable produit, livraison rapide, je recommande ! — Sarah M., il y a 2 jours."
- Suggestion : Chercher des avis sur Trustpilot ou Google. Vérifier les photos de profil par recherche inversée.

**#17 — Badges de confiance factices**
- Définition : Afficher des logos de certification ou de paiement sécurisé sans que ceux-ci soient vérifiables.
- Mécanisme : Transfère la légitimité d'une institution reconnue vers le site par simple association visuelle.
- Signaux : logos Visa/Mastercard décoratifs non cliquables, "certifié par" sans lien, faux labels qualité, icônes cadenas génériques
- Exemple : Site affichant 6 logos de sécurité dont aucun ne renvoie vers une page de vérification réelle.
- Suggestion : Cliquer sur chaque badge — un badge légitime renvoie vers une page de vérification externe.

**#18 — Présentation produit générique**
- Définition : Utiliser des visuels et descriptions directement copiés des catalogues fournisseurs sans valeur ajoutée.
- Mécanisme : Crée une illusion de valeur ajoutée alors que le produit est identique à ce qui est vendu bien moins cher à la source.
- Signaux : photos identiques aux fiches AliExpress, fonds blancs uniformes, descriptions traduites automatiquement, marque inconnue
- Exemple : "Montre de luxe tendance 2024 — Livraison express depuis notre entrepôt européen." (photos identiques à 47 vendeurs AliExpress)
- Suggestion : Faire une recherche image inversée sur la photo principale.

---

### 0.4 Récupérer les identifiants Airtable

Tu en auras besoin dans n8n.

1. Va sur **airtable.com/create/tokens** → clique **"Create token"**
2. Donne-lui un nom : `n8n-manipdetect`
3. Scopes → coche **"data.records:read"**
4. Access → sélectionne ta base `ManipDetect`
5. Clique **"Create token"** → **copie la clé** (tu ne la reverras plus)

Pour l'ID de ta base :
1. Va sur ta base Airtable
2. Regarde l'URL : `https://airtable.com/appXXXXXXXX/...`
3. Copie la partie `appXXXXXXXX` — c'est ton **Base ID**

---

## ÉTAPE 1 — Créer le compte n8n et le workflow

1. Va sur **n8n.io** → **"Get started for free"**
2. Crée un compte (email + mot de passe)
3. Une fois connectée, clique **"New Workflow"**
4. Clique sur le titre en haut → renomme en `ManipDetect`

---

## ÉTAPE 2 — Nœud 1 : Webhook

Le webhook est le point d'entrée. Lovable enverra les données ici.

1. Clique **"Add first step"** → cherche `Webhook` → sélectionne
2. Dans le panneau de droite :
   - **HTTP Method** → `POST`
   - **Path** → tape `manipdetect`
3. En haut du panneau, bascule sur **"Test"** → clique **"Listen for test event"**

> ✅ **Copie l'URL du webhook** affichée — format :
> `https://ton-compte.app.n8n.cloud/webhook-test/manipdetect`
> C'est ce que tu colleras dans Lovable. La version sans `-test` sera
> l'URL de production une fois le workflow activé.

---

## ÉTAPE 3 — Nœud 2 : IF (détecter text ou url)

1. Clique **"+"** à droite du Webhook → cherche `IF` → sélectionne
2. Dans le panneau, configure la condition :
   - **Value 1** → clique sur l'icône `{}` (expression) → tape :
     `{{ $json.input_type }}`
   - **Operation** → `equals`
   - **Value 2** → tape : `url`

Cela crée deux sorties :
- **TRUE** → c'est une URL, il faut scraper
- **FALSE** → c'est du texte, on passe directement

---

## ÉTAPE 4 — Nœud 3 : Jina Scrape (branche URL)

Sur la branche **TRUE** du nœud IF :

1. Clique **"+"** → cherche `HTTP Request` → sélectionne
2. Dans le panneau :
   - **Method** → `GET`
   - **URL** → clique sur `{}` → tape :
     `https://r.jina.ai/{{ $('Webhook').first().json.content }}`
   - Descends jusqu'à **Headers** → clique **"Add Header"** :
     - **Name** : `Accept`
     - **Value** : `text/plain`
3. Renomme ce nœud : clique sur son titre → tape `Jina Scrape`

> 💡 Jina Reader est un service gratuit qui convertit n'importe quelle
> URL en texte Markdown propre. Aucun compte requis.

---

## ÉTAPE 5 — Nœud 4 : Merge

Il faut réunir les deux branches (texte direct + URL scrapée)
avant la suite du workflow.

1. Depuis la branche **FALSE** du IF → clique **"+"** → cherche `Merge` → sélectionne
2. Connecte aussi la **sortie de Jina Scrape** à ce même nœud Merge
   (glisse le fil depuis la sortie de Jina Scrape vers le nœud Merge)
3. Dans le panneau :
   - **Mode** → `Append`

---

## ÉTAPE 6 — Nœud 5 : Code (normaliser le texte)

Les deux branches n'ont pas le même format. Ce nœud crée
une variable unique `texte_a_analyser` quelle que soit la source.

1. Clique **"+"** après Merge → cherche `Code` → sélectionne
2. **Language** → `JavaScript`
3. Colle ce code :

```javascript
// Récupère le type d'input envoyé par Lovable
const inputType = $('Webhook').first().json.input_type;

let texte;

if (inputType === 'url') {
  // Jina retourne le texte dans .body ou directement dans la réponse
  const jinaData = $('Jina Scrape').first().json;
  texte = jinaData.body || jinaData.data || JSON.stringify(jinaData);
} else {
  // Texte libre envoyé directement par Lovable
  texte = $('Webhook').first().json.content;
}

// Limite à 4000 caractères pour contrôler le coût LLM
const texteTronque = String(texte).slice(0, 4000);

return [{ json: { texte_a_analyser: texteTronque } }];
```

4. Renomme ce nœud : `Prépare le texte`

---

## ÉTAPE 7 — Nœud 6 : Airtable (fetch la taxonomie)

1. Clique **"+"** après `Prépare le texte` → cherche `Airtable` → sélectionne
2. **Action** → `Search Records`
3. Clique **"Create new credential"** :
   - **API Key** → colle ta clé Airtable (copiée à l'étape 0.4)
4. Dans le panneau :
   - **Base** → tape ton Base ID (`appXXXXXXXX`)
   - **Table** → `Manipulation_Taxonomy`
   - Laisse tous les filtres vides (tu veux les 18 lignes)
5. Renomme : `Fetch Taxonomie`

---

## ÉTAPE 8 — Nœud 7 : LLM (le cœur de l'analyse)

### Option A — OpenAI (GPT-4o)

1. Clique **"+"** → cherche `OpenAI` → sélectionne
2. **Action** → `Message a Model`
3. Clique **"Create new credential"** → colle ta clé API OpenAI
4. Dans le panneau :
   - **Model** → `gpt-4o`
   - **System Message** → colle ce prompt :

```
Tu es un analyste expert en rhétorique et psychologie de la persuasion.

Ton rôle est d'analyser un texte ou le contenu d'une page web
et d'identifier les techniques de manipulation présentes.

INSTRUCTIONS :
1. Lis attentivement le texte soumis.
2. Identifie chaque passage qui correspond à une technique de la taxonomie.
3. Une même technique peut apparaître plusieurs fois.
4. Un passage peut combiner plusieurs techniques — signale-le.
5. Ne diagnostique PAS l'auteur du texte — analyse le texte uniquement.
6. Évalue l'intensité globale :
   faible (1-2 techniques), modérée (3-5),
   élevée (6+) ou systémique (techniques combinées et répétées).
7. Si aucune technique n'est détectée, dis-le clairement.
8. Sois pédagogique, pas alarmiste.

RETOURNE UNIQUEMENT un JSON valide avec cette structure exacte :
{
  "score_global": <entier>,
  "intensite": "<faible|moderee|elevee|systemique>",
  "techniques": [
    {
      "nom": "<nom exact de la technique>",
      "extrait": "<passage exact du texte>",
      "explication": "<pourquoi c'est cette technique>",
      "suggestion": "<comment répondre ou se positionner>"
    }
  ],
  "synthese": "<2-3 phrases résumant le profil rhétorique global>"
}

NE PAS ajouter de texte avant ou après le JSON.
NE PAS utiliser de balises markdown.
Retourner uniquement le JSON brut.
```

   - **User Message** → clique sur `{}` → tape :

```
Voici la taxonomie des techniques de manipulation :

{{ $('Fetch Taxonomie').all().map(r => '- ' + r.json.fields.Nom + ' : ' + r.json.fields.Definition + ' | Signaux : ' + r.json.fields.Signaux).join('\n') }}

Voici le texte à analyser :

{{ $('Prépare le texte').first().json.texte_a_analyser }}
```

5. Renomme : `Analyse LLM`

---

### Option B — Claude (Anthropic) via HTTP Request

Si tu préfères utiliser Claude :

1. Clique **"+"** → cherche `HTTP Request` → sélectionne
2. Dans le panneau :
   - **Method** → `POST`
   - **URL** → `https://api.anthropic.com/v1/messages`
   - **Headers** → ajoute ces 3 headers :
     - `x-api-key` : ta clé Anthropic
     - `anthropic-version` : `2023-06-01`
     - `Content-Type` : `application/json`
   - **Body** → `JSON` → colle :

```json
{
  "model": "claude-sonnet-4-5",
  "max_tokens": 2000,
  "system": "COLLE ICI LE MÊME PROMPT SYSTÈME QUE CI-DESSUS",
  "messages": [
    {
      "role": "user",
      "content": "Taxonomie :\n{{ $('Fetch Taxonomie').all().map(r => '- ' + r.json.fields.Nom + ' : ' + r.json.fields.Definition).join('\\n') }}\n\nTexte à analyser :\n{{ $('Prépare le texte').first().json.texte_a_analyser }}"
    }
  ]
}
```

3. Renomme : `Analyse LLM (Claude)`

---

## ÉTAPE 9 — Nœud 8 : Respond to Webhook

Ce nœud renvoie le résultat JSON à Lovable.

1. Clique **"+"** → cherche `Respond to Webhook` → sélectionne
2. Dans le panneau :
   - **Respond With** → `JSON`
   - **Response Body** → clique sur `{}` → tape :

**Si tu utilises OpenAI :**
```
{{ JSON.parse($('Analyse LLM').first().json.choices[0].message.content) }}
```

**Si tu utilises Claude :**
```
{{ JSON.parse($('Analyse LLM (Claude)').first().json.content[0].text) }}
```

3. **Response Code** → `200`
4. **Headers** → ajoute :
   - `Access-Control-Allow-Origin` : `*`
   *(nécessaire pour que Lovable puisse appeler le webhook sans erreur CORS)*

---

## ÉTAPE 10 — Tester le workflow

### Test rapide avec du texte

1. Dans n8n, assure-toi que le nœud **Webhook** est en mode **"Test"**
   et clique **"Listen for test event"**
2. Ouvre un outil comme **Postman** ou **Hoppscotch** (gratuit en ligne)
3. Envoie ce POST sur ton URL webhook :

```
URL : https://ton-compte.app.n8n.cloud/webhook-test/manipdetect
Method : POST
Headers : Content-Type: application/json
Body :
{
  "input_type": "text",
  "content": "Il ne reste que 3 places disponibles. Offre valable jusqu'à ce soir uniquement. Après tout ce que j'ai investi pour vous accompagner, j'espère que vous ne me décevrez pas. Tout le monde dans notre communauté a déjà franchi le pas."
}
```

4. Dans n8n, clique **"Execute step by step"** et observe
   chaque nœud s'exécuter dans l'ordre
5. Vérifie que le dernier nœud retourne un JSON valide avec
   `score_global`, `intensite`, `techniques`, `synthese`

### Test avec une URL

```json
{
  "input_type": "url",
  "content": "https://coachings.richissime.net/lm/roadmap-personnalisee/"
}
```

---

## ÉTAPE 11 — Connecter n8n à Lovable

Une fois le workflow testé et fonctionnel :

1. Dans n8n, clique le bouton **"Activate"** en haut à droite
   (bascule le workflow de mode test à mode production)
2. L'URL du webhook change de :
   `https://...webhook-test/manipdetect`
   à :
   `https://...webhook/manipdetect`
3. Dans Lovable, envoie ce prompt :

```
Replace the value of WEBHOOK_URL (or PLACEHOLDER_N8N_WEBHOOK_URL)
with this URL:
https://TON-COMPTE.app.n8n.cloud/webhook/manipdetect

Also make sure the fetch call uses these headers:
{
  "Content-Type": "application/json"
}

And sends this body structure:
{
  "input_type": "text" or "url",
  "content": the text or URL string
}
```

---

## Dépannage — erreurs fréquentes

| Erreur | Cause probable | Solution |
|---|---|---|
| `CORS error` dans Lovable | Header CORS manquant | Ajoute `Access-Control-Allow-Origin: *` dans Respond to Webhook |
| `Cannot read property of undefined` dans le nœud Code | Jina n'a pas retourné de corps | Ajoute un fallback : `texte = texte \|\| 'Contenu non extractible'` |
| JSON invalide dans Respond to Webhook | Le LLM a ajouté du texte autour du JSON | Ajoute un nœud Code avant Respond qui nettoie : `text.replace(/```json\|```/g, '').trim()` |
| Airtable retourne 0 records | Mauvais Base ID ou nom de table | Vérifie l'orthographe exacte de `Manipulation_Taxonomy` et le Base ID `appXXX` |
| Timeout après 30 secondes | LLM trop lent sur grand texte | Réduis la limite de caractères de 4000 à 2000 dans le nœud Code |
| `401 Unauthorized` Airtable | Token expiré ou mauvais scope | Recrée le token avec le scope `data.records:read` |

---

## Récapitulatif des 8 nœuds

| # | Nœud | Type | Rôle |
|---|---|---|---|
| 1 | Webhook | Trigger | Reçoit la requête de Lovable |
| 2 | IF | Logic | Détecte text vs url |
| 3 | Jina Scrape | HTTP Request | Scrape la page si url |
| 4 | Merge | Merge | Réunit les deux branches |
| 5 | Prépare le texte | Code | Normalise en variable unique |
| 6 | Fetch Taxonomie | Airtable | Charge les 18 techniques |
| 7 | Analyse LLM | OpenAI / HTTP | Analyse et retourne le JSON |
| 8 | Respond to Webhook | Response | Renvoie le rapport à Lovable |

---

## URLs de test recommandées

Commence par celles-ci — elles sont connues pour leur copywriting agressif :

| Site | URL | Techniques attendues |
|---|---|---|
| Richissime | https://coachings.richissime.net/lm/roadmap-personnalisee/ | Rareté artificielle, preuve sociale, ancrage prix |
| DecisionIA | https://decisionia.com/bootcamp-consultant-ia-430449/ | Fausse urgence, flatterie instrumentale |
| Mastery IA | https://www.mastery-ia-pro.fr/sommet?el=wc&utm_source=wc | Rareté, faux consensus, ancrage prix |
| BryanRGT | https://www.bryanrgt-coaching.com/webinaire-fb-3 | Fausse urgence, culpabilisation, preuve sociale |
| Equity Mastermind | https://www.equitymastermind.fr/ | Ancrage prix, rareté, badges factices |

---

**Caroline Tith** — Enterprise Architect, Data & AI Specialist

[LinkedIn](https://linkedin.com/in/caroline-tith) · [Email](mailto:caroline.tith@hotmail.com)

*ManipDetect — Le Wagon, AI Product Builder batch#1 — June 2026*

---

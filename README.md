# VSL /vsl — Le Cloud AI

Landing page VSL mono-objectif (réserver un appel Calendly) pour l'offre Le Cloud AI :
**une équipe d'employés IA installée dans ton entreprise en 7 jours**, branchée à tes
outils, payée une seule fois. Ciblée par Meta Ads. HTML/CSS/JS vanilla + 1 serverless
function Vercel.

> Clone structurel de la landing VSL de Suna Films Media (même ordre, mêmes sections,
> même tunnel), rebrandé à l'identité **Le Cloud AI** (noir + bleu fluo `#22ccff`,
> Manrope + Bodoni Moda italique) avec l'offre reprise de la page `/vsl` du site Le Cloud AI.

## Structure

```
vsl-lecloud/
├── index.html          ← landing complète (hero+vidéo, logos, calendrier, métriques,
│                          impact, mid-CTA, avis Google, portfolio, CTA final, modal 5 étapes)
├── rdv.html            ← page de réservation (widget Calendly + FAQ vidéo)
├── style.css           ← utilisé par rdv.html (index.html a son CSS critique inline)
├── script.js           ← modal, mini-calendrier, compteurs, slider, lecteur vidéo
├── vercel.json         ← cleanUrls + headers sécurité + cache assets
├── api/lead.js         ← serverless : webhook (lead) + email Resend (optionnel)
└── assets/
    ├── fonts/          ← manrope-variable.woff2, bodoni-italic.woff2
    └── images/         ← logos Le Cloud AI, portfolio clients, visages, agences
```

## Tunnel

1. **index.html** — VSL + preuves. Tous les CTA ouvrent la **modal 5 étapes**
   (nom → téléphone → courriel → domaine → taille d'équipe).
2. Submit → `POST /api/lead` (webhook + notif) → redirection vers **/rdv** avec les infos.
3. **rdv.html** — widget **Calendly** pré-rempli (name / email / téléphone) + FAQ vidéo.

## À personnaliser avant la prod

| Élément                    | Où                                                        |
|----------------------------|-----------------------------------------------------------|
| Vidéo VSL (hero)           | `script.js` → `VIDEO_ID` (placeholder repris du VSL Suna)  |
| Vidéos FAQ                 | `rdv.html` → attributs `data-video` (placeholders)         |
| Lien Calendly              | `rdv.html` → `data-url` (`calendly.com/sunafilmsmedia/nouvelle-reunion`) |
| Meta Pixel ID              | `index.html` + `rdv.html` (`889823396710916` — à confirmer)|
| `GHL_WEBHOOK_URL`          | Env var Vercel (jamais dans le code)                       |
| Métriques / cartes impact  | Chiffres illustratifs — à remplacer par tes vrais résultats|

## Variables d'environnement Vercel

| Nom                | Valeur                     | Notes                   |
|--------------------|----------------------------|-------------------------|
| `GHL_WEBHOOK_URL`  | URL du webhook lead        | Requis                  |
| `RESEND_API_KEY`   | `re_xxxxxxxx`              | Optionnel (email notif) |
| `NOTIF_EMAIL`      | `sunafilmsmedia@gmail.com` | Optionnel               |

## Déploiement

```bash
npm i -g vercel      # une fois
cd vsl-lecloud
vercel               # premier déploiement (link projet)
vercel --prod        # production
```

## Events Meta Pixel

- `PageView` — au chargement (auto)
- `ClickBookCTA` (custom) — clic sur un CTA (`data-cta`: hero / mid / final / calendar)
- `VideoPlay` (custom) — lecture de la vidéo VSL
- `Lead` — submit du formulaire réussi
- `Schedule` — RDV confirmé dans Calendly

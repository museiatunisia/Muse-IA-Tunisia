# Muse AI Tunisia — Employé IA unifié : WhatsApp + Messenger

Un seul serveur pour les deux canaux. Mêmes règles que les deux employés séparés : une question à la fois, créateur / vendeur / étudiant, jamais de prix inventé, leads logués dans `leads.csv` avec le canal (WhatsApp ou Messenger).

## Fichiers

- `app.py` — serveur Python sans dépendance (bibliothèque standard uniquement)
- `render.yaml` — déploiement en un clic sur Render

## Variables d'environnement (jamais dans le code, jamais dans un message)

| Variable | Valeur |
|---|---|
| `MODEL_API_KEY` | Clé Meta Model API (la même que pour le chatbot site) |
| `WHATSAPP_TOKEN` | Token permanent du System User « API Integration » — **le dernier copié par Moez** |
| `WHATSAPP_PHONE_NUMBER_ID` | ID du numéro WhatsApp de production (nouveau numéro, à venir demain) |
| `WHATSAPP_VERIFY_TOKEN` | Mot secret choisi pour le webhook WhatsApp |
| `PAGE_ACCESS_TOKEN` | Page Access Token de Muse AI Tunisia — **copié par Moez le 24/11/2026** |
| `MESSENGER_VERIFY_TOKEN` | Mot secret choisi pour le webhook Messenger (peut être le même que WhatsApp) |

## Déploiement sur Render (gratuit pour commencer)

1. Crée un compte sur render.com (avec Google ou GitHub).
2. New → Web Service → dépose ce dossier (ou connecte un dépôt Git avec ces fichiers).
3. Start Command : `python3 app.py` (déjà dans `render.yaml`).
4. Dans Environment, colle les 6 variables ci-dessus.
5. Tu obtiens une URL du type `https://muse-ai-employee.onrender.com`.
6. Teste : `https://muse-ai-employee.onrender.com/health` doit répondre `{"status": "ok"}`.

## Brancher les webhooks

### WhatsApp (quand le nouveau numéro sera enregistré)
- Meta Developers → « Tunisia IA » → WhatsApp → Configuration → Étape 2
- Callback URL : `https://muse-ai-employee.onrender.com/webhook/whatsapp`
- Verify token : la valeur de `WHATSAPP_VERIFY_TOKEN`
- S'abonner au champ **messages**.

### Messenger
- Meta Developers → « Tunisia IA » → Messenger → Webhooks
- Callback URL : `https://muse-ai-employee.onrender.com/webhook/messenger`
- Verify token : la valeur de `MESSENGER_VERIFY_TOKEN`
- S'abonner aux champs **messages** (et **messaging_postbacks**).
- Abonner la Page Muse AI Tunisia dans Messenger → Paramètres.

## Tester

- Messenger : envoie un message à la Page Muse AI Tunisia depuis un autre compte : « Salem, chnowa Muse ? »
- WhatsApp : envoie un message au nouveau numéro depuis un autre téléphone.
- Dans les deux cas, la réponse de Muse doit arriver, et une ligne apparaît dans `leads.csv` (statut Nouveau).

## Limites

- Fenêtre de 24h (WhatsApp et Messenger) : après 24h sans réponse du client, seulement un message template approuvé.
- Historique en mémoire : un redémarrage oublie les conversations en cours (les leads restent dans `leads.csv`).
- Coût Muse : ~800 tokens par message.
- Sur Render gratuit, le service s'endort après inactivité : la première réponse peut prendre ~30-60 secondes à se réveiller.

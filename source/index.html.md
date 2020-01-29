---
title: HappyRenting API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://avis-locataire.com'>Avis-Locataire</a>
  - <a href='https://collecte.avis-locataire.com'>Collecte</a>
  - <a href='https://communication.avis-locataire.com'>Communication</a>

search: true
---

# API Avis-Locataire

Documentation indisponible pour l'instant

# API Collecte

Documentation indisponible pour l'instant

# API Communication

## Envoyer un Email

```shell
curl -X POST \
  "https://communication.avis-locataire.com/v1/send_email" \
  -d "auth_key=mykey" \
  -d "auth_secret=mysecret" \
  -d "recipient=exemple@mail.com" \
  -d "subject=Email exemple" \
  -d "message=Ceci est un exemple%0ATest"
```

> La commande ci-dessus renvoie un JSON structuré comme ceci

```json
{
  "status": 200,
  "message": "Message successfully queued"
}
```

Cette requête permet d'envoyer un email simplement.

### Requête HTTP

`POST https://communication.avis-locataire.com/v1/send_email`

### Paramètres de la requête

Nom | Description
--------- | -----------
auth_key | Clé d'authentification.
auth_secret | Secret d'authentification correspondant à la clé ci-dessus.
recipient | Email du destinataire.
subject | Sujet de l'email.
message | Contenu du message.

<aside class="notice">
Note - Les emails sont envoyés de manière asynchrone
</aside>

## Envoyer un SMS

```shell
curl -X POST \
  "https://communication.avis-locataire.com/v1/send_sms" \
  -d "auth_key=mykey" \
  -d "auth_secret=mysecret" \
  -d "recipient=0602030405" \
  -d "message=Ceci est un example%0ATest"
```

> La commande ci-dessus renvoie un JSON structuré comme ceci

```json
{
  "status": 200,
  "message": "Message successfully queued"
}
```

Cette requête permet d'envoyer un SMS simplement.

### Requête HTTP

`POST https://communication.avis-locataire.com/v1/send_sms`

### Paramètres de la requête

Nom | Description
--------- | -----------
auth_key | Clé d'authentification.
auth_secret | Secret d'authentification correspondant à la clé ci-dessus.
recipient | Numéro de téléphone du destinataire.
message | Contenu du message.

<aside class="notice">
Note - Les SMS sont envoyés de manière asynchrone
</aside>

## Envoyer un message

```shell
curl -X POST \
  "https://communication.avis-locataire.com/v1/messages" \
  -d "auth_key=mykey" \
  -d "auth_secret=mysecret" \
  -d "email=exemple@email.com"
  -d "phone=0602030405" \
  -d "email_subject=Email exemple" \
  -d "email_content=Ceci est un example%0ATest" \
  -d "sms_content=Ceci est un example%0ATest"
  -d "channel=both"
```

> La commande ci-dessus renvoie un JSON structuré comme ceci

```json
{
  "status": 200,
  "message": "Message successfully queued"
}
```

Cette requête permet d'envoyer un message par SMS/Email ou les deux en même temps.

### Requête HTTP

`POST https://communication.avis-locataire.com/v1/send_sms`

### Paramètres de la requête

Nom | Description
--------- | -----------
auth_key | Clé d'authentification.
auth_secret | Secret d'authentification correspondant à la clé ci-dessus.
email | Email du destinataire.
phone | Numéro de téléphone du destinataire.
email_subject | Sujet de l'email.
email_content | Contenu du message Email.
sms_content | Contenu du message SMS.
channel | Canal utilisé, peut être `sms`, `email` ou `both`.

<aside class="notice">
Note - Les messages sont envoyés de manière asynchrone
</aside>

## Liste des messages

```shell
curl -X GET \
  "https://communication.avis-locataire.com/v1/messages" \
  -d "auth_key=mykey" \
  -d "auth_secret=mysecret" \
  -d "with_error=true" \
  -d "limit=1" \
  -d "offset=0" \
  -d "from=2020-01-01T14:00:00Z" \
  -d "from=2020-01-31T14:00:00Z"
```

> La commande ci-dessus renvoie un JSON structuré comme ceci

```json
{
  "status": 200,
  "messages": [
    {
      "id": 1,
      "channel": "both",
      "phone": "0602030405",
      "email": "exemple@email.com",
      "email_subject": "Email exemple",
      "email_content": "Ceci est un example%0ATest",
      "sms_content": "Ceci est un example%0ATest",
      "has_error": false,
      "error_message": null,
      "created_at": "2020-01-18T15:31:12Z"
    }
  ]
}
```

Cette requête permet de récupérer une liste de messages.

### Requête HTTP

`GET https://communication.avis-locataire.com/v1/messages`

### Paramètres de la requête

Nom | Description
--------- | -----------
auth_key | Clé d'authentification.
auth_secret | Secret d'authentification correspondant à la clé ci-dessus.
with_error | Récupérer uniquement les messages avec `true` ou sans `false` erreurs
limit | Nombre maximum de messages retourné.
offset | Ignore les `N` premiers messages.
from | Retourne les messages après la date spécifié au format ISO8601.
until | Retourne les messages avant la date spécifié au format ISO8601.

# Codes d'erreurs

> Example d'erreur avec un numéro de télephone invalide

```json
{
  "status": 400,
  "message": "invalid recipient"
}
```

Les API utilisent les codes d'erreurs suivants:

Code | Signification
---------- | -------
400 | Mauvaise Requête -- La requête est invalide.
401 | Non Autorisé -- Les information d'authentificaton sont invalides.
404 | Page Introuvable -- La page spécifié est introuvable.
429 | Trop de requêtes -- Le client a émis trop de requêtes dans un délai donné.
500 | Erreur Interne du Serveur -- Nous avons eu un problème avec notre serveur. Réessayez plus tard.

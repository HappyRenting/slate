---
title: HappyRenting API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://communication.avis-locataire.com'>Communication</a>

search: true
---

# Communication API

## Envoyer un SMS

```shell
curl -X GET \
  "https://communication.avis-locataire.com/v1/send_sms" \
  -d "auth_key=mykey" \
  -d "auth_secret=mysecret" \
  -d "recipient=0602030405" \
  -d "message=Ceci est un example%0DTest"
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

`GET https://communication.avis-locataire.com/v1/send_sms`

### Paramètres de la requête

Paramètre | Description
--------- | -----------
auth_key | Clé d'authentification.
auth_secret | Secret d'authentification correspondant à la clé ci-dessus.
recipient | Numéro de téléphone du destinataire.
message | Contenu du message.

<aside class="warning">
Attention - Les SMS sont envoyés de manière asynchrone
</aside>

## Codes d'erreurs

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
500 | Erreur Interne du Serveur -- Nous avons eu un problème avec notre serveur. Réessayez plus tard.

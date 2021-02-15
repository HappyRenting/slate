---
title: HappyRenting API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - javascript

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
# Envoi d'un email individuel
curl -X POST \
  "https://communication.avis-locataire.com/v1/send_email" \
  --data "auth_key=mykey" \
  --data "auth_secret=mysecret" \
  --data "recipient=exemple@mail.com" \
  --data "subject=Email exemple" \
  --data "identifier1=001" \
  --data "force_sending=false" \
  --data "unsubscribe_link=true" \
  --data "message=Ceci est un exemple%0ATest"\
  --data "test_mode=false"

# Envoi de plusieurs emails
curl -X POST \
  "https://communication.avis-locataire.com/v1/send_email?auth_key=mykey&auth_secret=mysecret" \
  --header 'Content-Type: application/json' \
  --data-raw '[
    {
      "recipient": "exemple@mail.com",
      "subject": "Email exemple",
      "identifier1": "001",
      "force_sending": true,
      "message": "Ceci est un exemple%0ATest"
    }, {
      "recipient": "exemple2@mail.com",
      "subject": "Email exemple2",
      "identifier1": "002",
      "force_sending": false,
      "unsubscribe_link": true,
      "message": "Ceci est un exemple2%0ATest"
    }
  ]'
```

```ruby
require 'uri'
require 'net/http'

params = {
  auth_key: 'mykey',
  auth_secret: 'mysecret',
  recipient: 'exemple@mail.com',
  subject: 'Email exemple',
  identifier1: '001',
  force_sending: false,
  unsubscribe_link: true,
  message: 'Ceci est un exemple%0ATest'
}

url = URI('https://communication.avis-locataire.com/v1/send_email')
url.query = params.map { |key, value| "#{key}=#{value}" }.join('&')

http = Net::HTTP.new(url.host, url.port)
request = Net::HTTP::Post.new(url)

response = http.request(request)
puts response.read_body
```

```javascript
var endpoint = 'https://communication.avis-locataire.com/v1/send_email'

var requestOptions = {
  method: 'POST',
  redirect: 'follow'
}

params = {
  auth_key: 'mykey',
  auth_secret: 'mysecret',
  recipient: 'exemple@mail.com',
  subject: 'Email exemple',
  identifier1: "001",
  force_sending: false,
  unsubscribe_link: true,
  message: 'Ceci est un exemple%0ATest'
}

queryParams = []
for (const [key, value] of Object.entries(params)) {
  queryParams.push(`${key}=${value}`)
}

fetch(`${endpoint}?${queryParams.join('&')}`, requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error))
```

> Les commandes ci-dessus renvoient un JSON structuré comme ceci

```js
// Exemple de réussite pour un envoi unique
{
  "status": 200,
  "message": "Message successfully queued"
}

// Exemple d'échec pour un envoi unique (destinataire invalide)
{
  "status": 400,
  "message": "invalid recipient"
}

// Exemple pour un envoi multiple
{
  "status": 200,
  "messages": [
    {
      "error": false,
      "recipient": "0602030405",
      "message": "Message successfully queued"
    },
    {
      "error": true,
      "recipient": null,
      "message": "message can't be blank"
    }
  ]
}
```

Cette requête permet d'envoyer un ou plusieurs emails simplement.

### Requête HTTP

`POST https://communication.avis-locataire.com/v1/send_email`

### Paramètres URL de la requête pour un envoi unique

Nom | Description
--------- | -----------
auth_key | Clé d'authentification
auth_secret | Secret d'authentification correspondant à la clé ci-dessus
recipient | Email du destinataire
subject | Sujet de l'email
identifier(1-2) | Paramètres optionel pour identifier en interne le message/envoi
email_originator | Paramètre pour changer le nom de l'expediteur dans l'email
logo_url | Paramètre pour changer le logo afficher dans l'entête de l'email
force_sending | Forcer l'envoie même si le destinataire est désinscrit (optionnel)
unsubscribe_link | Paramètre optionel qui détermine si le liens de désincription doit être visible
message | Contenu du message
test_mode | Mode test, n'envoi pas réellement le message (optionnel)


### Paramètres URL de la requête pour un envoi multiple

Nom | Description
--------- | -----------
auth_key | Clé d'authentification
auth_secret | Secret d'authentification correspondant à la clé ci-dessus

Dans le cas d'un envoi multiple, le corps de la requête HTTP doit contenir un tableau JSON contenant des objets structurés comme ci-dessous.

Valeur | Description
--------- | -----------
recipient | Email du destinataire
subject | Sujet de l'email
identifier(1-2) | Paramètres optionel pour identifier en interne le message/envoi
email_originator | Paramètre pour changer le nom de l'expediteur dans l'email
logo_url | Paramètre pour changer le logo afficher dans l'entête de l'email
force_sending | Forcer l'envoie même si le destinataire est désinscrit (optionnel)
unsubscribe_link | Paramètre optionel qui détermine si le liens de désincription doit être visible
message | Contenu du message
test_mode | Mode test, n'envoi pas réellement le message (optionnel)


<aside class="notice">
Note - Les emails sont envoyés de manière asynchrone
</aside>

## Envoyer un SMS

```shell
# Envoi d'un SMS individuel
curl -X POST \
  "https://communication.avis-locataire.com/v1/send_sms" \
  --data "auth_key=mykey" \
  --data "auth_secret=mysecret" \
  --data "recipient=0602030405" \
  --data "identifier1=001" \
  --data "force_sending=false" \
  --data "message=Ceci est un example%0ATest"

# Envoi de plusieurs SMS
curl -X POST \
  "https://communication.avis-locataire.com/v1/send_sms?auth_key=mykey&auth_secret=mysecret" \
  --header 'Content-Type: application/json' \
  --data-raw '[
    {
      "recipient": "0602030405",
      "identifier1": "001",
      "force_sending": true,
      "message": "Ceci est un exemple%0ATest"
    }, {
      "recipient": "0702030405",
      "identifier1": "002",
      "force_sending": false,
      "message": "Ceci est un exemple2%0ATest"
    }
  ]'
```

```ruby
require 'uri'
require 'net/http'

params = {
  auth_key: 'mykey',
  auth_secret: 'mysecret',
  recipient: '0602030405',
  identifier1: '001',
  force_sending: false,
  message: 'Ceci est un exemple%0ATest'
}

url = URI('https://communication.avis-locataire.com/v1/send_sms')
url.query = params.map { |key, value| "#{key}=#{value}" }.join('&')

http = Net::HTTP.new(url.host, url.port)
request = Net::HTTP::Post.new(url)

response = http.request(request)
puts response.read_body
```

```javascript
var endpoint = 'https://communication.avis-locataire.com/v1/send_sms'

var requestOptions = {
  method: 'POST',
  redirect: 'follow'
}

params = {
  auth_key: 'mykey',
  auth_secret: 'mysecret',
  recipient: '0602030405',
  identifier1: "001",
  force_sending: false,
  message: 'Ceci est un exemple%0ATest'
}

queryParams = []
for (const [key, value] of Object.entries(params)) {
  queryParams.push(`${key}=${value}`)
}

fetch(`${endpoint}?${queryParams.join('&')}`, requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error))
```

> Les commandes ci-dessus renvoient un JSON structuré comme ceci

```js
// Exemple de réussite pour un envoi unique
{
  "status": 200,
  "message": "Message successfully queued"
}

// Exemple d'échec pour un envoi unique (destinataire invalide)
{
  "status": 400,
  "message": "invalid recipient"
}

// Exemple pour un envoi multiple
{
  "status": 200,
  "messages": [
    {
      "error": false,
      "recipient": "0602030405",
      "message": "Message successfully queued"
    },
    {
      "error": true,
      "recipient": null,
      "message": "message can't be blank"
    }
  ]
}
```

Cette requête permet d'envoyer un SMS simplement.

### Requête HTTP

`POST https://communication.avis-locataire.com/v1/send_sms`

### Paramètres de la requête pour un envoi unique

Nom | Description
--------- | -----------
auth_key | Clé d'authentification
auth_secret | Secret d'authentification correspondant à la clé ci-dessus
recipient | Numéro de téléphone du destinataire
identifier(1-2) | Paramètres optionel pour identifier en interne le message/envoi
sms_originator | Paramètre pour changer le nom de l'expediteur dans le SMS
force_sending | Forcer l'envoie même si le destinataire est désinscrit (optionnel)
message | Contenu du message
test_mode | Mode test, n'envoi pas réellement le message (optionnel)

### Paramètres URL de la requête pour un envoi multiple

Nom | Description
--------- | -----------
auth_key | Clé d'authentification.
auth_secret | Secret d'authentification correspondant à la clé ci-dessus.

Dans le cas d'un envoi multiple, le corps de la requête HTTP doit contenir un tableau JSON contenant des objets structurés comme ci-dessous.

Valeur | Description
--------- | -----------
recipient | Numéro de téléphone du destinataire
identifier(1-2) | Paramètres optionel pour identifier en interne le message/envoi
sms_originator | Paramètre pour changer le nom de l'expediteur dans le SMS
force_sending | Forcer l'envoie même si le destinataire est désinscrit (optionnel)
message | Contenu du message
test_mode | Mode test, n'envoi pas réellement le message (optionnel)

<aside class="notice">
Note - Les SMS sont envoyés de manière asynchrone
</aside>

## Envoyer un message

```shell
# Envoi d'un message individuel
curl -X POST \
  "https://communication.avis-locataire.com/v1/messages" \
  --data "auth_key=mykey" \
  --data "auth_secret=mysecret" \
  --data "email=exemple@email.com"
  --data "phone=0602030405" \
  --data "email_subject=Email exemple" \
  --data "email_content=Ceci est un example%0ATest" \
  --data "sms_content=Ceci est un example%0ATest" \
  --data "identifier1=001" \
  --data "force_sending=false" \
  --data "unsubscribe_link=true" \
  --data "channel=both"

# Envoi de plusieurs messages
curl -X POST \
  "https://communication.avis-locataire.com/v1/messages?auth_key=mykey&auth_secret=mysecret" \
  --header 'Content-Type: application/json' \
  --data-raw '[
    {
      "email": "exemple@email.com",
      "phone": "0602030405",
      "email_subject": "Email exemple",
      "email_content": "Ceci est un exemple%0ATest"
      "sms_content": "Ceci est un example%0ATest",
      "identifier1": "001",
      "force_sending": true,
      "channel": "both"
    }, {
      "email": "exemple2@email.com",
      "phone": "0702030405",
      "email_subject": "Email exemple2",
      "email_content": "Ceci est un exemple2%0ATest"
      "sms_content": "Ceci est un example2%0ATest",
      "identifier1": "002",
      "force_sending": false,
      "unsubscribe_link": true,
      "channel": "both"
    }
  ]'
```

> La commande ci-dessus renvoie un JSON structuré comme ceci

```js
// Exemple de réussite pour un envoi unique
{
  "status": 200,
  "message": "Message successfully queued"
}

// Exemple d'échec pour un envoi unique (destinataire invalide)
{
  "status": 400,
  "message": "invalid recipient"
}

// Exemple pour un envoi multiple
{
  "status": 200,
  "messages": [
    {
      "error": false,
      "email": "exemple@email.com",
      "phone": "0602030405",
      "message": "Message successfully queued"
    },
    {
      "error": true,
      "email": null,
      "phone": null,
      "message": "message can't be blank"
    }
  ]
}
```

Cette requête permet d'envoyer un message par SMS/Email ou les deux en même temps.

### Requête HTTP

`POST https://communication.avis-locataire.com/v1/messages`

### Paramètres de la requête pour un envoi unique

Nom | Description
--------- | -----------
auth_key | Clé d'authentification
auth_secret | Secret d'authentification correspondant à la clé ci-dessus
email | Email du destinataire
phone | Numéro de téléphone du destinataire
email_subject | Sujet de l'email
email_content | Contenu du message Email
sms_content | Contenu du message SMS
identifier(1-2) | Paramètres optionel pour identifier en interne le message/envoi
email_originator | Paramètre pour changer le nom de l'expediteur dans l'email
sms_originator | Paramètre pour changer le nom de l'expediteur dans le SMS
logo_url | Paramètre pour changer le logo afficher dans l'entête de l'email
force_sending | Forcer l'envoie même si le destinataire est désinscrit (optionnel)
unsubscribe_link | Paramètre optionel qui détermine si le liens de désincription doit être visible
channel | Canal utilisé, peut être `sms`, `email`, `both`, `sms_first` ou `email_first`
test_mode | Mode test, n'envoi pas réellement le message (optionnel)

### Paramètres URL de la requête pour un envoi multiple

Nom | Description
--------- | -----------
auth_key | Clé d'authentification
auth_secret | Secret d'authentification correspondant à la clé ci-dessus

Dans le cas d'un envoi multiple, le corps de la requête HTTP doit contenir un tableau JSON contenant des objets structurés comme ci-dessous.

Valeur | Description
--------- | -----------
email | Email du destinataire
phone | Numéro de téléphone du destinataire
email_subject | Sujet de l'email
email_content | Contenu du message Email
sms_content | Contenu du message SMS
identifier(1-2) | Paramètres optionels pour identifier en interne le message/envoi
force_sending | Forcer l'envoie même si le destinataire est désinscrit (optionnel)
unsubscribe_link | Paramètre optionel qui détermine si le liens de désincription doit être visible
channel | Canal utilisé, peut être `sms`, `email`, `both`, `sms_first` ou `email_first`
test_mode | Mode test, n'envoi pas réellement le message (optionnel)

<aside class="notice">
Note - Les messages sont envoyés de manière asynchrone
</aside>

## Liste des messages

```shell
curl -X GET \
  "https://communication.avis-locataire.com/v1/messages" \
  --data "auth_key=mykey" \
  --data "auth_secret=mysecret" \
  --data "with_error=true" \
  --data "limit=1" \
  --data "offset=0" \
  --data "identifier1=001" \
  --data "identifier2=test" \
  --data "from=2020-01-01T14:00:00Z" \
  --data "from=2020-01-31T14:00:00Z"
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
      "custom_identifier1": "001",
      "custom_identifier2": "test",
      "email_originator": null,
      "sms_originator": null,
      "force_sending": false,
      "unsubscribe_link": true,
      "has_error": false,
      "error_message": null,
      "created_at": "2020-01-18T15:31:12Z",
      "test_mode": false
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
identifier(1-2) | Limite aux messages avec les mêmes identitfiants
from | Retourne les messages après la date spécifié au format ISO8601
until | Retourne les messages avant la date spécifié au format ISO8601
test_mode | Filtre les messages selon leur mode (test ou non)

## Statut globale des messages

```shell
# Récupération du statut
curl -X POST \
  "https://communication.avis-locataire.com/v1/messages_status" \
  --data "auth_key=mykey" \
  --data "auth_secret=mysecret" \
  --data "identifier1=001" \
  --data "identifier2=test"
```

> La commande ci-dessus renvoie un JSON structuré comme ceci

```js
// Exemple de réussite
{
  "status": 200,
  "all_sent": true
}

// Exemple d'échec
{
  "status": 400,
  "message": "invalid identifier"
}
```

Cette requête permet de récupérer le statut global des messages filtré par l'`identifier`.

### Requête HTTP

`GET https://communication.avis-locataire.com/v1/messages_status`

### Paramètres de la requête pour un envoi unique

Nom | Description
--------- | -----------
auth_key | Clé d'authentification
auth_secret | Secret d'authentification correspondant à la clé ci-dessus
identifier(1-2) | Paramètres pour filtrer en interne le message/envoi

## Informations supplémentaires

Détail sur le champ `channel` :

Valeur | Action
--------- | -----------
sms | Envoi le message uniquement par SMS (numéro valide requis)
email | Envoi le message uniquement par email (adresse email valide requis)
both | Envoi le message par SMS et email
sms_first | Envoi le message par SMS si le numéro est présent sinon envoi par email
email_first | Envoi le message par email si l'adresse email est présente sinon envoi par SMS

# API Rendez-vous

## Webhook des flux

> Exemple de retour JSON du webhook

```js
{
  "status": "preset",
  "channel": "email",
  "start_at": "2020-06-12T12:00:00+02:00",
  "end_at": "2020-06-12T12:45:00+02:00",
  "duration": 45,
  "location": "6 rue de la Place, Paris 75001",
  "reminder_delay": 0,
  "reminder_status": false,
  "reminder_channel": "disabled",
  "recipient": {
    "first_name": "Jean",
    "last_name": "DUPONT",
    "email": "jean.dupont@email.com",
    "phone": null,
    "variables":[
      { name: "Bâtiment", content: "B" },
      { name: "Porte",  content: "845" }
    ]
  }
}
```

Liste des attributs du rendez-vous :

Nom | Description
--------- | -----------
status | Statut du rendez-vous (preset = prédéfini par le bailleur, approved = approuvé par le bailleur)
channel | Canal de communication (SMS ou Email ou les deux)
start_at | Date et heure de début du rendez-vous au format ISO8601
end_at | Date et heure de fin du rendez-vous au format ISO8601
duration | Durée du rendez-vous en minutes
location | Lieux du rendez-vous
reminder_delay | Délai avant le rendez-vous pour le rappel en heures
reminder_status | Statut du rappel (true = envoyé, false = pas encore envoyé)
reminder_channel | Canal de communication du rappel (SMS ou Email ou les deux)
recipient | Objet JSON contenant les informations du destinataire (cf. tableau ci-dessous)

Liste des attributs de l'objet JSON du destinataire :

Nom | Description
--------- | -----------
first_name | Prénom du destinataire
last_name | Nom de famille du destinataire
email | L'Email du destinataire
phone | Le numéro de téléphone du destinataire
variables | Tableau d'objet JSON contenant les informations des variables du destinataire (cf. tableau ci-dessous)


Liste des attributs de l'objet JSON des variables :

Nom | Description
--------- | -----------
name | Nom de la variable
content | Valeur pour la variable

# Codes d'erreurs

> Exemple d'une erreur interne du serveur

```json
{
  "status": 500,
  "message": "internal server error"
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

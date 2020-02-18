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
  --data "identifier=001"
  --data "message=Ceci est un exemple%0ATest"

# Envoi de plusieurs emails
curl -X POST \
  "https://communication.avis-locataire.com/v1/send_email?auth_key=mykey&auth_secret=mysecret" \
  --header 'Content-Type: application/json' \
  --data-raw '[
    {
      "recipient": "exemple@mail.com",
      "subject": "Email exemple",
      "identifier": "001",
      "message": "Ceci est un exemple%0ATest"
    }, {
      "recipient": "exemple2@mail.com",
      "subject": "Email exemple2",
      "identifier": "002",
      "message": "Ceci est un exemple2%0ATest"
    }
  ]'
```

```ruby
require 'uri'
require 'net/http'

auth_key = 'mykey'
auth_secret = 'mysecret'
recipient = 'exemple@mail.com'
subject = 'Email exemple'
identifier = '001'
message = 'Ceci est un exemple%0ATest'

url = URI('https://communication.avis-locataire.com/v1/send_email')
url.query = "auth_key=#{auth_key}&auth_secret=#{auth_secret}&recipient=#{recipient}&subject=#{subject}&identifier=#{identifier}&message=#{message}"

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

var auth_key = 'mykey'
var auth_secret = 'mysecret'
var recipient = 'exemple@mail.com'
var subject = 'Email exemple'
var identifier = "001"
var message = 'Ceci est un exemple%0ATest'

fetch(
  `${endpoint}?auth_key=${auth_key}&auth_secret=${auth_secret}&recipient=${recipient}&subject=${subject}&identifier=${identifier}&message=${message}`,
  requestOptions
).then(response => response.text())
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
identifier | Paramètre optionel pour identifier en interne le message/envoi
message | Contenu du message


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
identifier | Paramètre optionel pour identifier en interne le message/envoi
message | Contenu du message


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
  --data "identifier=001"
  --data "message=Ceci est un example%0ATest"

# Envoi de plusieurs SMS
curl -X POST \
  "https://communication.avis-locataire.com/v1/send_sms?auth_key=mykey&auth_secret=mysecret" \
  --header 'Content-Type: application/json' \
  --data-raw '[
    {
      "recipient": "0602030405",
      "identifier": "001",
      "message": "Ceci est un exemple%0ATest"
    }, {
      "recipient": "0702030405",
      "identifier": "002",
      "message": "Ceci est un exemple2%0ATest"
    }
  ]'
```

```ruby
require 'uri'
require 'net/http'

auth_key = 'mykey'
auth_secret = 'mysecret'
recipient = '0602030405'
identifier = '001'
message = 'Ceci est un exemple%0ATest'

url = URI('https://communication.avis-locataire.com/v1/send_sms')
url.query = "auth_key=#{auth_key}&auth_secret=#{auth_secret}&recipient=#{recipient}&identifier=#{identifier}&message=#{message}"

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

var auth_key = 'mykey'
var auth_secret = 'mysecret'
var recipient = '0602030405'
var identifier = "001"
var message = 'Ceci est un exemple%0ATest'

fetch(
  `${endpoint}?auth_key=${auth_key}&auth_secret=${auth_secret}&recipient=${recipient}&identifier=${identifier}&message=${message}`,
  requestOptions
).then(response => response.text())
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
identifier | Paramètre optionel pour identifier en interne le message/envoi
message | Contenu du message

### Paramètres URL de la requête pour un envoi multiple

Nom | Description
--------- | -----------
auth_key | Clé d'authentification.
auth_secret | Secret d'authentification correspondant à la clé ci-dessus.

Dans le cas d'un envoi multiple, le corps de la requête HTTP doit contenir un tableau JSON contenant des objets structurés comme ci-dessous.

Valeur | Description
--------- | -----------
recipient | Numéro de téléphone du destinataire
identifier | Paramètre optionel pour identifier en interne le message/envoi
message | Contenu du message

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
  --data "identifier=001" \
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
      "identifier": "001",
      "channel": "both"
    }, {
      "email": "exemple2@email.com",
      "phone": "0702030405",
      "email_subject": "Email exemple2",
      "email_content": "Ceci est un exemple2%0ATest"
      "sms_content": "Ceci est un example2%0ATest",
      "identifier": "002",
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
identifier | Paramètre optionel pour identifier en interne le message/envoi
channel | Canal utilisé, peut être `sms`, `email` ou `both`

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
identifier | Paramètre optionel pour identifier en interne le message/envoi
channel | Canal utilisé, peut être `sms`, `email` ou `both`

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
  --data "identifier=001" \
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
      "identifier": "001",
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
identifier | Limite aux messages avec le même identitfiant
from | Retourne les messages après la date spécifié au format ISO8601.
until | Retourne les messages avant la date spécifié au format ISO8601.

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

# Memberships

## Create

> The request xml can look like:

```xml
<membership> 
  <login>new_user</login> 
  <password>somepassword</password> 
  <email>new_user@yahoo.com</email> 
  <first-name>Joe</first-name>
  <last-name>Smith</last-name>
</membership>
```

> First and last name are optional. For example:

```shell
curl \
-u username:password \
-H 'Content-Type: application/xml' \
-X POST \
-d "
<membership> 
  <login>new_user</login> 
  <password>somepassword</password> 
  <email>new_user@yahoo.com</email> 
</membership>" http://subdomain.inklingmarkets.com/memberships.xml 
```

> Example of response:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<membership>
  <id type="integer">90103</id>
</membership>
```

`POST /memberships.xml`

Creates a user for your site. Need to be an admin of the site to use this service. Parameters include:

  * login
  * email
  * first_name
  * last_name
  * password
  * openid_url
  * email_new_market_alerts: If set to true, users get an email alert when new markets are created.

Store this membership ID for single sign on and other reporting/user management needs. Here is a list of current errors returned and their definitions:

  * **Alias has already been taken.** - When the user you are trying to create already exists at the Inkling site.
  * **Email is not properly formatted.** - An invalid email, either the wrong length, none supplied, or no @ sign.
  * **Email has already been registered.** - User already has a trading membership at this site.


## Show

> Request:

```shell
curl -u username:password http://subdomain.inklingmarkets.com/memberships/123.xml
```

> Response:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<membership>
  <id type="integer">965195095</id>
  <portfolio type="integer">250</portfolio>
  <worth type="integer">5000250</worth>
  <available-balance type="integer">4500000</available-balance>
  <user>
    <active type="boolean">true</active>
    <email>test_trader@example.com</email>
    <first-name>Mr.</first-name>
    <last-name>Trade</last-name>
    <login>trader</login>
  </user>
  <balance type="integer">5000000</balance>
</membership>
```

`GET /memberships/:membership_id.xml`

Retrieves information on a single user. An available balance is returned which is the users balance minus any risk the user has with short positions they own.


## Me

> Request:

```shell
curl -u username:password http://subdomain.inklingmarkets.com/memberships/me.xml
```

> Response:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<membership>
  <id type="integer">965195095</id>
  <portfolio type="integer">250</portfolio>
  <worth type="integer">5000250</worth>
  <available-balance type="integer">4500000</available-balance>
  <user>
    <active type="boolean">true</active>
    <email>test_trader@example.com</email>
    <first-name>Mr.</first-name>
    <last-name>Trade</last-name>
    <login>trader</login>
  </user>
  <balance type="integer">5000000</balance>
</membership>
```

`GET /memberships/me.xml`

Retrieves information on the current signed in user. An available balance is returned which is the users balance minus any risk the user has with short positions they own.


## Update

> Request:

```shell
curl \
-u user:password \
-H 'Content-Type: application/xml' \
-X PUT \
-d  "<membership> \
  <first-name>Joe</first-name>
  <last-name>Somebody</last-name>
</membership>" \
http://subdomain.inklingmarkets.com/memberships/:membership_id.xml
```

> Response:

```
Status: 200 OK
```

`PUT /memberships/:membership_id.xml`

Updates a previously created user. You must be using an admin account in order to change the information of someone that isn't you.


## List

> Example request:

```shell
curl -u username:password http://subdomain.inklingmarkets.com/memberships.xml
```

> Example response:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<memberships type="array">
  <membership>
    <id type="integer">965195095</id>
    <user>
      <active type="boolean">true</active>
      <email>test_trader@example.com</email>
      <first-name>Mr.</first-name>
      <last-name>Trade</last-name>
      <login>trader</login>
    </user>
    <balance type="integer">5000000</balance>
    <portfolio type="integer">4400000</portfolio>
  </membership>
  <membership>
    <id type="integer">900692218</id>
    <user>
      <active type="boolean">true</active>
      <email>test_trader2@example.com</email>
      <first-name>Mr.</first-name>
      <last-name>Trade2</last-name>
      <login>trader2</login>
    </user>
    <balance type="integer">5000000</balance>
    <portfolio type="integer">4500000</portfolio>
  </membership>
</memberships>
```

`GET /memberships.xml`

You can obtain a list of the memberships at your site and their balance and portfolio.


## Delete

> Example request:

```shell
curl \
-u user:password \
-H 'Content-Type: application/xml' \
-X PUT \
-d  "<balance> \
  <value>500000</value>
</balance>" \
http://subdomain.inklingmarkets.com/memberships/:membership_id/balance.xml
```

> Example response:

```
Status: 200 OK
```

`DELETE /memberships/:membership_id.xml`

Deletes a user.

## Update User's Balance

> For example, to reset the user's balance to $5,000:

```shell
curl \
-u user:password \
-H 'Content-Type: application/xml' \
-X PUT \
-d  "<balance> \
  <value>500000</value>
</balance>" \
http://subdomain.inklingmarkets.com/memberships/:membership_id/balance.xml
```

> Response:

```
Status: 200 OK
```


`PUT /memberships/:membership_id/balance.xml`

Update a user's balance. Every now and then you might decide to give another player more money. You must be using an admin account in order to change the information of someone that isn't you.
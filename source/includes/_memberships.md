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

`GET /memberships/:membership_id.xml`

Retrieves information on a single user. An available balance is returned which is the users balance minus any risk the user has with short positions they own. For example:
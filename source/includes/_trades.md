# Trades

If your intention is to make trades on behalf of your users make sure you are using a username/password with admin privileges. If you are making trades for yourself, use your own username/password and you don't need to pass in a membership_id in with the request.

## Quote

`POST /stocks/:stock_id/trades/quote.xml`

This is a way to see what a trade might cost. The trade isn't actually committed.

> If one of your users wants to buy 5 shares of a stock, the request should look something like this:

```
<trade>
  <quantity>5</quantity>
  <membership_id>1234</membership_id>
</trade>
```

> To sell shares just use negative numbers for "quantity". E.g.:

```
<trade>
  <quantity>-5</quantity>
  <membership_id>1234</membership_id>
</trade>
```

> To determine how many shares must be purchased/sold in order to change a stocks price to a desired value, specify a "desired_price" value:

```
<trade>
  <desired_price>6000</desired_price>
  <membership_id>1234</membership_id>
</trade>
```

> To see what a trade would cost for a user with membership_id 1234 when they buy 5 shares of stock with id 345:

```shell
curl \
-u username:password \
-H 'Content-Type: application/xml' \
-X POST \
-d  "<trade>
  <quantity>5</quantity>
  <membership_id>1234</membership_id>
</trade>" \
http://subdomain.inklingmarkets.com/stocks/345/trades/quote.xml
```

> Example response:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<buy>
  <final-price type="integer">5425</final-price>
  <paid type="integer">26999</paid>
  <starting-price type="integer">5375</starting-price>
  <quantity type="integer">5</quantity>
</buy>
```


## Create

`POST /stocks/:stock_id/trades.xml`



Creates a trade.

> If one of your users wants to buy 5 shares of a stock, the request should look something like this:

```
<trade>
  <quantity>5</quantity>
  <membership_id>1234</membership_id>
</trade>
```

> To sell shares just use negative numbers for "quantity". E.g.:

```
<trade>
  <quantity>-5</quantity>
  <membership_id>1234</membership_id>
</trade>
```

> For example, to complete a trade for a user with membership_id 1234 who wants to buy 5 shares of stock with id 345:

```shell
curl \
-u username:password \
-H 'Content-Type: application/xml' \
-X POST \
-d  "<trade>
  <quantity>5</quantity>
  <membership_id>1234</membership_id>
</trade>" \
http://subdomain.inklingmarkets.com/stocks/345/trades.xml
```

> Example response:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<buy>
 <created-at type="datetime">2008-03-31T11:54:01-07:00</created-at>
 <final-price type="integer">4571</final-price>
 <id type="integer">704139</id>
 <paid type="integer">22731</paid>
 <starting-price type="integer">4522</starting-price>
</buy>
```


## List

`GET /trades.xml`

> Request:

```shell
curl -u username:password http://subdomain.inklingmarkets.com/memberships/all/trades.xml
```

> Response:

```xml
<trades> 
  <buy> 
    <id type="integer">3</id> 
    <membership> 
      <id type="integer">10</id> 
    </membership> 
    <stock> 
      <id type="integer">9</id> 
    </stock> 
    <created-at type="datetime">2006-08-18T21:29:36-07:00</created-at> 
    <quantity type="integer">50</quantity> 
    <paid type="integer">250000</paid> 
  </buy> 
  <sell> 
    <id type="integer">4</id> 
    <membership> 
      <id type="integer">10</id> 
    </membership> 
    <stock> 
      <id type="integer">10</id> 
    </stock> 
    <created-at type="datetime">2006-08-18T21:29:36-07:00</created-at> 
    <quantity type="integer">50</quantity> 
    <paid type="integer">-250000</paid> 
  </sell> 
</trades>
```

Retrieves a list trades made on the site. If you are using this service as a normal user, you will be pulling all your own trades.

`http://subdomain.inklingmarkets.com/trades.xml`

However, if you use this service as an admin you have the ability to pull all the trades of all the users on your site. Just make sure you use a slightly modified url:

`http://subdomain.inklingmarkets.com/memberships/all/trades.xml`


# Stocks

## Create

`POST /markets/:market_id/stocks.xml`

Creates a stock for a previously created market. Parameters include:

  * **name** *(required)* - A name for you stock. (e.g. For “Who will win American Idol” this would be a contestants name.) 40 characters max.
  * **symbol** *(required)* - An abbreviation for this stock. Used for display on graphs. 5 characters max.
  * **starting-price** - The starting price for your stock. For Exclusive markets the prices are automatically set. For Futures, you should set the starting price to your best guess. (e.g. For a market “What will be the price of oil at the end of the month?”, a starting-price could be $150 )
  * **description** - Any specific information regarding this choice. (e.g. Background information on the American Idol contestant)

> Request:

```shell
curl \
-u username:password \
-H 'Content-Type: application/xml' \
-d  "<stock> \
  <name>Cubs</name> \
  <symbol>CUBS</symbol> \
</stock>" \
http://subdomain.inklingmarkets.com/markets/:market_id/stocks.xml
```

> Response:

```xml
Status: 201 Created
Location: http://subdomain.inklingmarkets.com/stocks/:stock_id.xml

<stock>
  ...
</stock>
```

## Show

`GET /stocks/:stock_id.xml`

> Request:

```shell
curl -u username:password http://subdomain.inklingmarkets.com/stocks/26395.xml
```

> Response:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<stock>
  <created-at type="datetime">2007-10-29T08:40:41-07:00</created-at>
  <description></description>
  <id type="integer">26395</id>
  <name>Boston Celtics</name>
  <price type="integer">6700</price>
  <price-before-resolution type="integer">6700</price-before-resolution>
  <refunded type="boolean" nil="true"></refunded>
  <resolved-at type="datetime" nil="true"></resolved-at>
  <starting-price type="integer">800</starting-price>
  <symbol>Celts</symbol>
  <eod-price type="integer">1786</eod-price>
</stock>
```

Retrieves the current price and related stock information for a single stock.

<aside class='notice'>`eod-price` stands for End of Day price. You can use that to tell how much the stock is up today since midnight Pacific time.</aside>


## Update

`PUT /stocks/:stock_id.xml`

Updates a stock in a previously created market.

> Request:

```shell
curl \
-u user:password \
-H 'Content-Type: application/xml' \
-X PUT \
-d  "<stock> \
  <name>Cubs</name> \
  <symbol>CUBS</symbol> \
  <starting-price>1000</starting-price>
</stock>" \
http://subdomain.inklingmarkets.com/stocks/:stock_id.xml
```

> Response:

```
Status: 200 OK
```


## Delete

`DELETE /stocks/:stock_id.xml`

Deletes an existing stock.

> Request:

```shell
curl \
-u username:password \
-H 'Content-Type: application/xml' \
-X DELETE \
http://subdomain.inklingmarkets.com/stocks/:stock_id.xml
```

> Response:

```
Status: 200 OK
```


## Cash-out

`POST /stocks/:stock_id/resolutions.xml`

Cashes out a stock with its final price. If this market is a mutually exclusive options market, cashing out the "winner" at $100 will cash out the remaining stocks automatically at $0.

> Request:

```shell
curl \
-u username:password \
-H 'Content-Type: application/xml' \
-d  "<resolution> \
  <price>0</price> \
</resolution>" \
http://subdomain.inklingmarkets.com/stocks/:stock_id/resolutions.xml
```

> Response:

```xml
Status: 201 Created

<resolution>
  ...
</resolution>
```

## Refund

`POST /stocks/:stock_id/refunds.xml`

Sometimes you'll need to refund a stock because the question was too vague or something came up unexpectedly that it would be unfair to pay anyone out. This function will go through remaining people who still have positions in this stock and pay them back what they originally paid for it.

> Request:

```shell
curl \
-u user:password \
-H 'Content-Type: application/xml' \
-X POST \
http://subdomain.inklingmarkets.com/stocks/:stock_id/refunds.xml
```

> Response:

```
Status: 201 Created
```

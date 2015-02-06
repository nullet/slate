# Markets

## List

`GET /markets.xml?params`

> Here's a request with the `recent` sort applied.

```shell
curl -u username:password http://subdomain.inklingmarkets.com/markets.xml?sort=recent
```

> And the response:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<markets type="array">
  <options>
    <created-at type="datetime">2008-06-15T21:13:26-07:00</created-at>
    <description>first_market</description>
    <starts-at type="datetime">2008-05-23T21:13:26-07:00</starts-at>
    <ends-at type="datetime">2008-06-23T21:13:26-07:00</ends-at>
    <exclusive type="boolean">true</exclusive>
    <id type="integer">1</id>
    <name>first_market</name>
    <updated-at type="datetime">2008-06-15T21:13:26-07:00</updated-at>
    <membership>
      <id type="integer">799466675</id>
    </membership>
    <stocks type="array">
      <stock>
        <created-at type="datetime">2008-06-15T21:13:26-07:00</created-at>
        <description>First stock description</description>
        <id type="integer">1</id>
        <name>First stock</name>
        <price type="integer">10</price>
        <price-before-resolution type="integer">10</price-before-resolution>
        <refunded type="boolean" nil="true"></refunded>
        <resolved-at type="datetime" nil="true"></resolved-at>
        <starting-price type="integer">10</starting-price>
        <symbol>FSD</symbol>
        <eod-price type="integer">1786</eod-price>
      </stock>
      <stock>
        <created-at type="datetime">2008-06-15T21:13:26-07:00</created-at>
        <description>Another stock description</description>
        <id type="integer">2</id>
        <name>First stock</name>
        <price type="integer">10</price>
        <price-before-resolution type="integer">10</price-before-resolution>
        <refunded type="boolean" nil="true"></refunded>
        <resolved-at type="datetime" nil="true"></resolved-at>
        <starting-price type="integer">10</starting-price>
        <symbol>FSD</symbol>
      </stock>
    </stocks>
  </options>
  <futures>
    <created-at type="datetime">2008-06-15T21:13:26-07:00</created-at>
    <description>futures_market</description>
    <ends-at type="datetime">2008-06-23T21:13:26-07:00</ends-at>
    <exclusive type="boolean" nil="true"></exclusive>
    <id type="integer">2002</id>
    <name>futures_market</name>
    <updated-at type="datetime">2008-06-15T21:13:26-07:00</updated-at>
    <membership>
      <id type="integer">799466675</id>
    </membership>
    <stocks type="array">
      <stock>
        <created-at type="datetime">2008-06-15T21:13:26-07:00</created-at>
        <description>Hunderd dollar stock description</description>
        <id type="integer">5</id>
        <name>Hundred Dollar Stock</name>
        <price type="integer">10000</price>
        <price-before-resolution type="integer">10000</price-before-resolution>
        <refunded type="boolean" nil="true"></refunded>
        <resolved-at type="datetime" nil="true"></resolved-at>
        <starting-price type="integer">10000</starting-price>
        <symbol>HDS</symbol>
      </stock>
      <stock>
        <created-at type="datetime">2008-06-15T21:13:26-07:00</created-at>
        <description>Stock with Position</description>
        <id type="integer">6</id>
        <name>Stock with Position</name>
        <price type="integer">10100</price>
        <price-before-resolution type="integer">10100</price-before-resolution>
        <refunded type="boolean" nil="true"></refunded>
        <resolved-at type="datetime" nil="true"></resolved-at>
        <starting-price type="integer">10000</starting-price>
        <symbol>SWP</symbol>
      </stock>
    </stocks>
  </futures>
</markets>
```

This is how you'll retrieve a list of all markets and their stocks from your site. You can also attach a `sort` parameter to your call. Sort options can be divided into two groups: Admin and General.

### Admin

  * **my** - These are all the markets (private or public) currently running (if you're an admin) that haven't been cashed out yet.
  * **my_resolved** - public/private markets that have been cashed out.
  * **pending** - public/private markets that have been started on but not finished or they are waiting for approval to be published.
  * **submitted** - public/private markets that have been submitted for approval.
  * **expired** - public/private markets that have reached their end date and need to be cashed out soon.

This group returns records that "belong" to the user or admin. If the user is an admin, it includes all the markets created on the site. If the user isn't an admin, the markets returned are only markets the user has created.

### General

  * **recent** - public markets or markets you've been invited too sorted in descending order of their "created_at" date.
  * **expiring** - public/invited markets are sorted in ascending order of their "ends_at" date.
  * **active** - public/invited markets are sorted in descending order of their number of positions held.
  * **closed** - public/invited only shows markets that have been fully cashed out.

This group pulls back records that are public or that the user has been invited to. If an admin account uses the web service with the second group of sorts, they will not see private markets that they haven't been invited to.

<aside class='notice'>`eod-price` stands for End of Day price. You can use that to tell how much the stock is up today since midnight Pacific time.</aside>

Each market question returned will be of type:

  * **binary** - just the answer of yes/no (e.g. Will the Sun rise tomorrow?)
  * **options** - has a selection of answers (e.g. Which contestant will win American Idol?)
  * **numberranges** - a selection of number ranges to choose from (e.g. How many Emmy Awards will Grey's Anatomy win? Possible answers: 0-1, 2-4, more than 4)
  * **futures** - answer to the question is a specific answer.
  * **dateranges** - a selection of date ranges to choose from (e.g. When will Grey's Anatomy release its fall schedule? Possible answers: June, July, August, September or later)
  * **datemarket** - answer is a specific date

  A reference for some of the fields seen to in the response:

  * **price** - current price
  * **price-before-resolution** - if the stock has been chased out, this was the price just before the final answer
  * **refunded** - true/false if this stock had to be refunded to traders
  * **resolved-at** - the datetime when this stock was cashed out or refunded
  * **starting-price** - the price that was original set for this stock to start trading at
  * **exclusive** - true/false. Some options markets are mutually exclusive (e.g. Which of these products will sell the most).

## Show

`GET /markets/:market_id.xml`

Retrieves the market and stock information for a single market. Similar data is retrieved as the market list service.

> Request:

```shell
curl -u username:password http://subdomain.inklingmarkets.com/markets/123.xml
```

> Response:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<options>
  <created-at type="datetime">2008-06-15T21:13:26-07:00</created-at>
  <description>first_market</description>
  <ends-at type="datetime">2008-06-23T21:13:26-07:00</ends-at>
  <exclusive type="boolean">true</exclusive>
  <id type="integer">1</id>
  <name>first_market</name>
  <updated-at type="datetime">2008-06-15T21:13:26-07:00</updated-at>
  <membership>
    <id type="integer">799466675</id>
  </membership>
  <stocks type="array">
    <stock>
      <created-at type="datetime">2008-06-15T21:13:26-07:00</created-at>
      <description>First stock description</description>
      <id type="integer">1</id>
      <name>First stock</name>
      <price type="integer">10</price>
      <price-before-resolution type="integer">10</price-before-resolution>
      <refunded type="boolean" nil="true"></refunded>
      <resolved-at type="datetime" nil="true"></resolved-at>
      <starting-price type="integer">10</starting-price>
      <symbol>FSD</symbol>
      <eod-price type="integer">1786</eod-price>
    </stock>
    <stock>
      <created-at type="datetime">2008-06-15T21:13:26-07:00</created-at>
      <description>Another stock description</description>
      <id type="integer">2</id>
      <name>First stock</name>
      <price type="integer">10</price>
      <price-before-resolution type="integer">10</price-before-resolution>
      <refunded type="boolean" nil="true"></refunded>
      <resolved-at type="datetime" nil="true"></resolved-at>
      <starting-price type="integer">10</starting-price>
      <symbol>FSD</symbol>
      <eod-price type="integer">1786</eod-price>
    </stock>
  </stocks>
</options>
```

## Create

`POST /markets.xml`

> Request:

```shell
curl \
-u username:password \
-H 'Content-Type: application/xml' \
-d  "<market> \
  <name>Which team will win the World Series?</name> \
  <ends-at>2008-05-05T02:20:30-05:00</ends-at> \
  <class-type>Exclusive</class-type> \
</market>" \
http://subdomain.inklingmarkets.com/markets.xml
```

> Response:

```xml
Status: 201 Created
Location: http://subdomain.inklingmarkets.com/markets/:market_id.xml

<market>
  ...
</market>
```

Creates a market. Parameters include:

  * **name** *(required)* - The question you are trying to predict. Between 6 and 130 characters.
  * **class-type** *(required)*:
    * **Options** – A multiple choice question can either have one right answer or multiple right answers. (e.g. Which teams will qualify for the World Cup?)
    * **Exclusive** – A mutually exclusive market determining probability of something occurring. (e.g. Who will win American Idol?)
    * **Futures** – Determine a specific number. (e.g. How many minivans will Chrysler sell in the 4th quarter?)
    * **Binary** – The answer to your question is yes or no. (e.g. Will the Detroit Tigers win the World Series?). For Binary markets there is no need to setup answers through the API because the default is always YES.
  * **starts-at** - A date (optional) the market begins trading. May be now or in the future. Must occur before ends-at date. If omitted, defaults to an immediate start. Uses xmlschema date-time format (CCYY-MM-DDThh:mm:ssTZD). Where TZD is Z or [+-]hh:mm. If self is a UTC time, Z is used as TZD. [+-]hh:mm is used otherwise.
  * **ends-at** - A date the market closes to trading. Uses xmlschema format (CCYY-MM-DDThh:mm:ssTZD) of a date. Where TZD is Z or [+-]hh:mm. If self is a UTC time, Z is used as TZD. [+-]hh:mm is used otherwise.
  * **scale** - for use in Futures (class-type)
  * **description** - Any background information people need to know to successfully understand the topic you are trying to forecast. Include any details about how your market will end (e.g. A source for which you’ll use to determine “How many minivans will Chrysler sell” could be a the quarterly report.)


## Update

`PUT /markets/:market_id.xml`

> Request:

```shell
curl \
-u username:password \
-H 'Content-Type: application/xml' \
-X PUT \
-d  "<market> \
  <name>Which team will win the 2008 World Series?</name> \
</market>" \
http://subdomain.inklingmarkets.com/markets/:market_id.xml
```

> Response:

```xml
Status: 200 OK
```

Updates a market.

## Additional Attributes

Some parameters can only be set with an update once a question has been created. These include `tag-list` and `category-ids`.

> `tag-list`: Multiple tags are allowed. Separate multiple tags with spaces.

```shell
curl \
-u username:password \
-H 'Content-Type: application/xml' \
-X PUT \
-d  "<market><tag-list>sports baseball</tag-list></market>" \
http://subdomain.inklingmarkets.com/markets/:market_id.xml
```

> `category-ids`: Add or update categories via a PUT with the id of the category you want to add:

```shell
curl \
-u username:password \
-H 'Content-Type: application/xml' \
-X PUT \
-d  "<market><category-ids>12</category-ids></market>" \
http://subdomain.inklingmarkets.com/markets/:market_id.xml
```

> One category is allowed per question. You can retrieve a list of category ids via:

```
PUT http://home.example.com/categories.xml
```

> You can create a new category via POST to '/categories.xml'. For example:

```shell
curl -H 'Content-Type: application/xml' -u username:password -d "NEWCATEGORY" / 
http://home.example.com/categories.xml
```

> Or delete a category via:

```
DELETE /categories/:category_id.xml
```

## Delete

`DELETE /markets/:market_id.xml`

Deletes a market.

> Request:

```shell
curl \
-u username:password \
-H 'Content-Type: application/xml' \
-X DELETE \
http://subdomain.inklingmarkets.com/markets/:market_id.xml
```

> Response:

```
Status: 200 OK
```

## Publish a Market

`POST /markets/:market_id/publications.xml`

Publishes a market after it's all ready. If the the publisher is an admin it's available for trading immediately. If the publisher isn't an admin, an admin will need to reissue the publish API call.

> Request: 

```shell
curl \
-u username:password \
-H 'Content-Type: application/xml' \
-X POST \
http://subdomain.inklingmarkets.com/markets/:market_id/publications.xml
```

> Response:

```xml
Status: 201 Created
Location: http://subdomain.inklingmarkets.com/markets/:market_id.xml

<market>
  ...
</market>
```

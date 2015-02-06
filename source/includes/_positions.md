# Positions

## List

`GET /positions.xml`

> An example request:

```shell
curl -u username:password http://subdomain.inklingmarkets.com/memberships/all/positions.xml
```

> Which returns:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<positions type="array">
  <position>
    <created-at type="datetime">2008-06-11T00:00:00-07:00</created-at>
    <id type="integer">13</id>
    <quantity type="integer">50</quantity>
    <resolved-at type="datetime" nil="true"></resolved-at>
    <total-cost type="integer">0</total-cost>
    <up type="integer">0</up>
    <stock>
      <id type="integer">5</id>
    </stock>
    <membership>
      <id type="integer">12</id>
    </membership>
  </position>
  <position>
    <created-at type="datetime">2008-06-11T00:00:00-07:00</created-at>
    <id type="integer">12</id>
    <quantity type="integer">20</quantity>
    <resolved-at type="datetime" nil="true"></resolved-at>
    <total-cost type="integer">1136</total-cost>
    <up type="integer">0</up>
    <stock>
      <id type="integer">68</id>
    </stock>
    <membership>
      <id type="integer">965195095</id>
    </membership>
  </position>
  <position>
    <created-at type="datetime">2008-06-11T00:00:00-07:00</created-at>
    <id type="integer">11</id>
    <quantity type="integer">-400</quantity>
    <resolved-at type="datetime" nil="true"></resolved-at>
    <total-cost type="integer">450000</total-cost>
    <up type="integer">0</up>
    <stock>
      <id type="integer">9</id>
    </stock>
    <membership>
      <id type="integer">29</id>
    </membership>
  </position>
</positions>
```

Retrieves everyone's portfolio. A portfolio is composed of the "positions" your users have in stocks on your site. If you are using this service as a normal user, you will be pulling all your own portfolio. Also includes recently "resolved" positions in options/futures that are less than 7 days old.

`http://subdomain.inklingmarkets.com/positions.xml`

However, if you use this service as an admin you have the ability to pull all the positions of all the users on your site. Just make sure you use a slightly modified url:

`http://subdomain.inklingmarkets.com/memberships/all/positions.xml`

To pull the positions of a specific user just use their membership id in the url:

`http://subdomain.inklingmarkets.com/memberships/:membership_id/positions.xml`

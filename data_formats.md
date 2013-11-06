---
title: "Data Formats"
layout: default
---

## Data formats

We support HTML, XML, and JSON data formats. To get a particular format, set your request's Accept header, or suffix the request with the correct file extension.

Slices also support the CSV and JSONP data formats. To receive JSONP, use the `.jsonp` file extension and the `$callback` clause.

### REST

Qu has a [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) API, which means three things:

* All resources in the system -- datasets, slices, and views of those slices -- have unique URLs that will always correspond to them.
* All resources in the system should have enough metadata attached to them that you can figure out how to process them without consulting external documentation.
* All resources in the system use hypermedia to point to related resources and more specific views of the currently viewed resource. This concept is often called "Hypermedia as the engine of application state" or [HATEOAS](https://en.wikipedia.org/wiki/HATEOAS).

### HTML

The HTML format is used mainly for exploring the API. It not only returns query data, but presents query forms for digging into the data. The text of the page should contain links to all other resources available from a page.

In addition to the page text indicating links to all other resources available from a page, the `<head>` element should contain `<link>` elements with appropriate `rel` attributes pointing to other resources.

### JSON

JSON uses the [Hypertext Application Language (HAL)][HAL] format to convey links to related resources. Links are found under the `_links` key and linked resources are found under the `_embedded` key. Here is an example of the format for the URL `/data/county_taxes/incomes.json`.

```json
{
  "_links": {
    "self": { "href": "/data/county_taxes/incomes.json" }
    "next": { "href": "/data/county_taxes/incomes.json?$page=2" },
    "last": { "href": "/data/county_taxes/incomes.json?$page=12" },
    "up": { "href": "/data/county_taxes.json" },
    "query": { 
      "href": "/data/county_taxes/incomes.{?format}?$where={?where}&$orderBy={?orderBy}&$select={?select}",
      "templated": true
    }
  },
  "total": 1149,
  "size": 100,
  "page": 1,
  "metadata": {
    // TODO - not yet determined
  },
  "results": [
{"interest_income":12695,"dividend_income":3802,"wages_and_salaries_income":885899,"adjusted_gross_income":1063207,"exceptions":53353,"tax_returns":20563,"county":"Tooele County","state_abbr":"UT"},
{"interest_income":215544,"dividend_income":82411,"wages_and_salaries_income":6475118,"adjusted_gross_income":8655581,"exceptions":435704,"tax_returns":157947,"county":"Utah County","state_abbr":"UT"},
{"interest_income":1642,"dividend_income":741,"wages_and_salaries_income":27261,"adjusted_gross_income":41007,"exceptions":2450,"tax_returns":1050,"county":"Wayne County","state_abbr":"UT"},
// ...
  ]
}
```

### CSV

CSV is a simple format to receive data, but a more difficult format for sending metadata.

Links to other resources are given with a `Link` header in the response. Metadata is not currently sent with CSV.

### XML

XML uses the [Hypertext Application Language (HAL)][HAL] format to convey links to related resources. Links are found under the `_links` key and individual datums are found under the `_embedded` key. Here is an example of the format for the URL `/data/county_taxes/incomes.xml`.

```xml
<resource href="/data/county_taxes/incomes.xml">
  <link rel="self" href="/data/county_taxes/incomes.xml" />
  <link rel="next" href="/data/county_taxes/incomes.xml?$page=2" />
  <link rel="last" href="/data/county_taxes/incomes.xml?$page=12" />
  <link rel="up" href="/data/county_taxes.xml" />
  <link rel="query"
        href="/data/county_taxes/incomes.{?format}?$where={?where}&$orderBy={?orderBy}&$select={?select}"
        templated="true" />
  <total>1149</total>
  <size>100</size>
  <page>1</page>
  <metadata>
    <!-- TODO not yet determined -->
  </metadata>
  <results>
    <result>
      <interest_income>12695</interest_income>
      <dividend_income>3802</dividend_income>
      <wages_and_salaries_income>885899</wages_and_salaries_income>
      <adjusted_gross_income>1063207</adjusted_gross_income>
      <exceptions>53353</exceptions>
      <tax_returns>20563</tax_returns>
      <county>Tooele County</county>
      <state_abbr>UT</state_abbr>
    </result>
    <!-- more results -->
  </results>
</resource>
```

[HAL]: http://stateless.co/hal_specification.html
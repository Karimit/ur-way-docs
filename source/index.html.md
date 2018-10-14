---
title: ع طريقك API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - http
  - shell

toc_footers:
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - verify
  - connect
  - errors

search: true
---

# Introduction

All results of API calls are <a href='http://jsonapi.org/format'>JSON API</a> compliant JSON. Except for a few simple requests that return simple JSON objects.

# Including Relationships

You can include resource relationships and their nested relationships to minimize the number of API calls.

Example:

  You can include a trip's driver when listing trips or when showing a specific trip by
  using the `include` param like this:

  `?include[]=driver`

  You can also include the driver's car (nested relationship):

  `?include[]=driver.car`

  <aside class="success">
    Including a nested relationship automatically includes its parent.

    i.e:
      `?include[]=driver.car` includes the driver so you do not need to ?include[]=driver
  </aside>

  <aside class="success">
    Each resource section will include a list of relationships that you can include with it.
  </aside>

  <aside class="warning">
    You can pass params with multiple values by appending the param name with brackets `[]`:

    RIGHT: ?include[]=driver&include[]=driver.car

    WRONG: ?include=driver,driver.car
  </aside>

# Selecting Fields

You can select only the fields that you are interested in from the main resource and the included relationships.

  Example:

  When listing trips, you maybe only interested in a trip's `cost` and `seats`. You can pass the fields param to
  select them like this:

  `?fields[trip][]=cost&fields[trip][]=seats`

  Where `trip` (in `fields[trip]`) is the singular, lowercase name of the resource type. (So it is `trip`, not `trips` nor `Trip`)

  If you have included the `car` relationship, then you can select its fields like this:

  `?fields[trip][]=cost&fields[trip][]=seats&fields[car][]=make&fields[car][]=model`

  <aside class="success">
    Each resource section will include a list of fields that can be selected, along with the authorization level required to access different fields if authorization is required.
  </aside>

  <aside class="warning">
    The resource type name can different from a relationship's or an attribute's name. For example, a `trip` has two relationships of type `User`, `driver` and `passengers`.

    And you can only use the resource type name with the fields param. This means that the following params are invalid and will be ignored:

    `?fields[driver][]=first_name&fields[passenger][]=first_name`

    The correct way to select driver and passenger fields is:

    `?fields[user][]=first_name`

    This also means that you can't select a different set of fields for each of the driver and the passengers relationships since the are of the same type.
  </aside>

# Filtering

You can use predefined resource filters that will be listed in the resource section.

Example:

  To list only active trips: `/trips?status=active` OR `/trips?status[]=active`

  To list active or pending trips: `/trips?status[]=active&status[]=pending`

# Ordering

Example:

Order trips by cost, ascending: `?order[]=cost&sort_order=asc`

Order trips by cost, then by id, descending: `order[]=cost&order[]=id&sort_order=desc`

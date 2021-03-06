---
---

{% include warning_box.html
   title="This is API fiction!"
   content="<p>This document provides the currently proposed API documentation for diaspora*. Please note that at this point in time, none of the methods are actually implemented.</p>

<p>ToDo:</p>

<ul>
  <li>Error codes and messages</li>
</ul>"
%}

# diaspora\* API documentation

diaspora\* aims to offer a sophisticated API for most social networking applications. Because of our decentralized nature, some things might be a little bit different than most other API implementations, especially regarding authentication. If you experience any issues, feel free to [get in touch][communication] with us.

## Media Types

Currently, the API only supports JSON data exchange, so all requests should have their `Accept` header set correctly:

~~~
Accept: application/json
~~~

If, for some reason, setting the `Accept` header is not possible, adding `.json` to the call URL will also work.

Unless otherwise noted, bodies submitted via `POST` requests should be JSON encoded. Parameters to `GET` routes are simple request URL variables.

## Value formats

| Type      | Description                              | Example                            |
| --------- | ---------------------------------------- | ---------------------------------- |
| GUID      | A network-wide, unique identifier.       | `298962a0b8dc0133e40d406c8f31e210` |
| timestamp | An ISO 8601 time and date with timezone. | `2016-02-19T02:13:41.863Z`         |

## Pagination

Some responses, especially those with large response sets or with a larger server load, might respond in a paginated way. Paginated responses will contain a `Link` header with API routes to request items *before* or *after* the items returned in the current response:

~~~
Link: <https://example.com/api/v1/example?page=2>; rel="next",
      <https://example.com/api/v1/example?page=42>; rel="last"
~~~

*Note*: Line breaks were added to increase the readability, the actual header will not contain line breaks.

Please **do not try to guess the pagination URLs**. Some resources like streams, might use timestamps or GUIDs instead of an increasing page counter.

### Possible `rel` values

| `rel`      | Description                                                         |
| ---------- | ------------------------------------------------------------------- |
| `first`    | Returns the first set of items in the requested set.                |
| `previous` | Returns the items *before* the items in the currently returned set. |
| `next`     | Returns the items *after* the items in the currently returned set.  |
| `last`     | Returns the last set of items in the requested set.                 |

### Limiting the amount of results per page

By default, API endpoints return up to 20 items on a page. This can be changed by using the `per_page` parameter. `https://example.com/api/v1/example?page=2&per_page=50` would, for example, return 50 items. Please note that, unless otherwise noted, this parameter is capped to a maximum value of `100`.

## API support

This document specifies API Version 1, which is supported by diaspora\* release X and newer. Version discovery should be done using [nodeinfo][nodeinfo] prior to making any requests to ensure the endpoints are available. If a compatible diaspora\* version was detected once, it is safe to assume a pod will stay compatible.

[communication]: https://wiki.diasporafoundation.org/How_we_communicate
[nodeinfo]: http://nodeinfo.diaspora.software/

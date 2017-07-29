---
title: Alumni Spaces API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - php
  - python
  - javascript

toc_footers:
  - <a href='mailto:andrew@alumnispaces.com'>Email Andrew for help</a>


includes:
  - people
  - transactions

search: true
---

# Introduction

Alumni Spaces maintains two public APIs. The contentAPI for accessing content created by groups and the vaultAPI for accessing transaction data from Alumni.Fund.  At the moment, only vaultAPI is documented.  If you need access to contentAPI, let us know.

# Authentication

> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

To access any Alumni.Fund data, you'll first need a valid API key from Alumni Spaces - contact our team to be issued a school/user-specific key.

There are two methods of authentication for all API requests: basic HTTP authentication and a custom header for your API key.

For all requests, pass a basic HTTP auth using:

`username: As9laxE8pKhP4yYZ`

`password: wXMV0U5bV1Mcpsv4kCqTgYBJeXzW`

You will also need to pass a custom header: `x-alumnispaces-key` with the value of your API key.

`x-alumnispaces-key: yourapikeystring`

Any request that does not contain both authentication methods will be rejected.

<aside class="notice">
You must replace <code> yourapikeystring </code> with your personal API key. Contact Alumni Spaces to get a key.
</aside>
<aside class="notice">
API Keys are limited by scope and verb permissions. Your key will be limited in scope to the records pertinent to your institution and will likely be limited to GET requests only, even though DELETE and PUT requests are documented here. If you feel that you need write access, please discuss with our team.
</aside>

# Alumni.Fund Vault Records

## People and Transactions

Alumni.Fund retains two entities: `Person` and `Transaction`. In the plural, they are referred to as `People` and `Transactions`.

As you might imagine, a `Person` is the instigator of a `Transaction`. Each `Person` has a unique ID which is the only part of their identity that is stored with the `Transaction` record.  

At the moment, we do not de-duplicate or combine `Person` records, so `Tyrion Lannister` might have 5 different `Person` records to match the 5 transactions he's conducted. This will change in the future and the schema reflects a future in which People will contain data from multiple transactions.


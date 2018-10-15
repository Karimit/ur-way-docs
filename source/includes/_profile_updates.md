# Profile Update

All of user's profile attributes are optional. Some become required and will be validated if the `profile_complete` boolean attribute is set to true.

```http
  PUT /user
  Content-Type: application/json
```

```json
  {
    "addresses_attributes":{"0": {"city":"c1","location":"l1"}, "1": {"city": "c2","location":"l2"}},
    "birthdate":"1988-09-13 00:00:00",
    "first_name":"a name",
    "last_name":"a last name",
    "gender": "female",
    "profile_complete":"true"
  }
```

```http
HTTP/1.1 200 OK
```

```json
{
    "data": {
        "id": "23",
        "type": "user",
        "attributes": {
            "first_name": "a name",
            "last_name": "a last name",
            "birthdate": "1988-09-13",
            "uid": "+201061797842",
            "spare_phone": null,
            "phone": "+201061797842",
            "gender": "female",
            "balance": 0,
            "verified_user": null,
            "verified_driver": true,
            "identity_verified": false,
            "phone_verified": true,
            "profile_complete": true,
            "created_at": "2018-08-30T23:03:58.999Z",
            "updated_at": "2018-10-15T00:37:23.237Z",
            "trips_count": 0,
            "tickets_count": 0,
            "notes": null,
            "new_user": false,
            "rating_avg": null,
            "picture_url": "http://localhost:3000/rails/active_storage/blobs/eyJfcmFpbHMiOnsibWVzc2FnZSI6IkJBaHBFdz09IiwiZXhwIjpudWxsLCJwdXIiOiJibG9iX2lkIn19--57e37815432b95f9896bace14d114a2810797f29/user2.jpg",
            "id_picture_one_url": null,
            "id_picture_two_url": null,
            "dl_picture_one_url": null,
            "dl_picture_two_url": null
        },
        "relationships": {
            "addresses": {
                "data": [
                    {
                        "id": "31",
                        "type": "address"
                    },
                    {
                        "id": "64",
                        "type": "address"
                    }
                ]
            },
            "cars": {
                "data": [
                    {
                        "id": "16",
                        "type": "car"
                    }
                ]
            }
        }
    },
    "included": [
        {
            "id": "31",
            "type": "address",
            "attributes": {
                "city": "c1",
                "location": "l1",
                "latitude": 33.5014498,
                "longitude": 36.2468128
            },
            "relationships": {
                "super_address": {
                    "data": {
                      "id": "10",
                      "type": "super_address"
                    }
                }
            }
        },
        {
            "id": "64",
            "type": "address",
            "attributes": {
                "city": "c2",
                "location": "l2",
                "latitude": null,
                "longitude": null
            },
            "relationships": {
                "super_address": {
                    "data": {
                        "id": "31",
                        "type": "super_address"
                    }
                }
            }
        }
    ]
}
```

Endpoint

put /api/user

User Profile Attributes

attribute | type | required
--------- | ---- | --------
birthdate | date | false
first_name | string | if profile_complete is true
last_name | string | if profile_complete is true
phone | string | true
gender | string [male, female] | false
spare_phone | string | false
profile_complete | boolean | false
addresses | nested attribute | at least 1 address if profile_complete is true

Nested Attributes

  Address
  You can update a user's associated addresses by including the `addresses_attributes` param with the user update params like this:

`json
{ addresses_attributes: { "0": { "city": "...", "location": '...' } } }
`

  * Adding an address (or many addresses) to user's addresses

The following will add 2 addresses to the user addresses list:

`http
put /api/user
`

`json
{
  addresses_attributes: {
    "0": { "city": "c1", "location": 'l1' },
    "1": { "city": "c2", "location": 'l2' }
  }
}
`

  * Updating an address (or many addresses)

  To update an existing address, add the address `id` with its new attribute values like this:

`http
put /api/user
`

`json
{
  addresses_attributes: {
    "0": { "id": "1", "city": "new city", "location": 'new location' },
    "1": { "id": "2", "city": "new city", "location": 'new location' },
  }
}
`

  * Removing an address (or many addresses)

  To remove an address, add the address `id` with the `_destroy` params like this:

`http
put /api/user
`

`json
{
  addresses_attributes: {
    "0": { "id": "1", "_destroy": "true" }
  }
}
`

  Errors

  Status | Error Code | Meaning
  ------ | ---------- | -------
  404 | record_not_found | `addresses_attributes` has an object with an `id` that does not point to an address that the user has.


Validations & Errors

  Attribute Validations:

  1. spare_phone: should be a valid phone and unique if present

  2. birthdate: on or before thirteen years ago if present

  3. first_name, last_name: required if profile_complete. Should be less than 30 chars if present

  4. addresses: user should have at least one address if profile_complete is true

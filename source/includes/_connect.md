# Connect

A single endpoint to register or sign in a user with only a phone number. You will also need to provide the verification_token acquired in `Verify`

## connect

```http
  POST /users/verification/start
  Content-Type: application/json
  {
    "phone": "+201061797840"
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
            "first_name": "John",
            "last_name": "Smith",
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
            "updated_at": "2018-10-14T01:23:51.355Z",
            "trips_count": 0,
            "tickets_count": 0,
            "notes": null,
            "new_user": false,
            "rating_avg": null,
            "picture_url": "http://example.com/rails/active_storage/blobs/eyJfcmFpbHMiOnsibWVzc2FnZSI6IkJBaHBFdz09IiwiZXhwIjpudWxsLCJwdXIiOiJibG9iX2lkIn19--57e37815432b95f9896bace14d114a2810797f29/user2.jpg",
            "id_picture_one_url": null,
            "id_picture_two_url": null,
            "dl_picture_one_url": null,
            "dl_picture_two_url": null
        },
        "relationships": {
            "addresses": {
                "data": [
                    {
                        "id": "36",
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
    }
}
```

The `verification_token` expires in 5 minutes.

If `connect` request fails (due to an expired or incorrect verification_token), the application needs to start the verification process again.

Endpoint

`POST /connect`

Errors

Status | Error Code | Meaning
------ | ---------- | -------
422 | invalid_data | invalid phone or verification_token
422 | phone_taken | Phone number is taken. This can very rarely happen if for example th same user sent two request in quick succession.

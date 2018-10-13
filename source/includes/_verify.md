# Verify

First, `start` the verification process to obtain a verification token. Then `verify` using the token you received.

## start

> 1.start: Obtain a verification code:

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

Endpoint

  `POST /users/verification/start`

Response

  `Empty`

Errors

Status | Error Code | Meaning
------ | ---------- | -------
422 | invalid_phone | phone is missing or invalid
500 | sending_verification_failed | Verification service failed

## verify

> 2.verify: verify phone with the verification code you received.

```http
  POST /users/verification/verify
  Content-Type: application/json
  {
    "phone": "+201061797840"
    "verification_code": "1234"
  }
```

```http
HTTP/1.1 200 OK
```

```json
{
    "message": "Verification code is correct.",
    "success": true,
    "verification_token": "RJdBcCVdXDzKZvrigMNUlw"
}
```

Endpoint

  `POST /users/verification/verify`

Response

  `verification_token`: A token that you will need to login or sign up.

Errors

Status | Error Code | Meaning
------ | ---------- | -------
422 | invalid_phone | phone is missing or invalid
422 | verification_failed | verification_code is invalid or expired
422 | missing_verification_code | verification_code is missing

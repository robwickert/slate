# Errors


The Marin API uses normal HTTP response codes to indicate success or failure.

HTTP Status Code | Meaning 
---------- | -------
200 | OK
400 | Bad Request 
401 | Unauthorized 
403 | Forbidden 
404 | Not Found 
500 | Internal Server Error
503 | Service Unavailable

```json
{
    "error": {
        "code": "INVALID_AUTH_TOKEN",
        "message": "Invalid auth token"
    }
}
```

If your request fails you will recieve additional information about the nature of the failure in the `error` response field.

Marin Error Code | Meaning 
---------- | -------
INVALID_AUTH_TOKEN | The access token provided in the request header is not valid

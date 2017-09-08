# Errors

If your request fails you will recieve an error response which details the reason for the failure.

```json
{
    "error": {
        "code": "INVALID_AUTH_TOKEN",
        "message": "Invalid auth token"
    }
}
```

Error Code | Meaning
---------- | -------
400 | Bad Request 
401 | Unauthorized
403 | Forbidden 
404 | Not Found 
500 | Internal Server Error
503 | Service Unavailable
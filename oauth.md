# Oauth 
## OAuth 2.0 roles
```
There are primarily four kinds of roles present in OAuth 2.0, which are the following:
• Resource owner 
• Resource server
• Client
• Authorization server
```

### Resource owner
```
in the OAuth 2.0 flow, the resource owner is simply the user that is interested in 
granting a registered OAuth application to access their account.
```
### resource server:
```
The resource server is the server handling authenticated requests after the application has obtained an access token on behalf of the resource owner .
Simply put, a resource server allows/denies access of a specific resource to an application.
```

### Client or application
```
A client is simply an application registered to the provider (say Facebook/Google+)and is used by the third party (say http://www.packt.com) 
to access or manipulate a user's or resource owner's data. This concludes that a client 
is merely an application which allows the third party to request on behalf of the 
resource owner to the OAuth provider.

```

### Authorization server
```
An authorization server is capable of granting or denying a client an access token. 
The authorization server authenticates the resource and, generally through various 
interactions, issues an access token to the client if everything goes well


A resource server and authorization server are closely knit and when in the same 
web application, often referred to as an OAuth API
```
## concepts

### The application

```
The application or client must be registered on the OAuth provider's website. The 
registration process involves the third party fill-out details, such as application 
name, website link, logo, configuration data, and so on. Once the registration is done, 
an application is assigned a unique identifier called the client ID
```

### Redirect URI

```
Every application must redirect to a pre-determined URI once the OAuth flow is 
complete. By default, the authorization server rejects redirect_uri mismatches 
between application configuration and the actual one provided. The redirect URI 
is a crucial component of the OAuth flow, and hijacking this can result in nasty 
outcomes, which we'll see in upcoming sections of this chapter
```

### Access token
```
An access token is a secret token allotted to the application and is tied to a particular 
user with specific permissions. The resource server expects an access token every 
time a request is made to it
```

### Client ID
```
The client ID is a unique identifier that is returned when the application is registered 
successfully. It is not secret information and is crucial in the working of OAuth 
applications. Different OAuth implementations refer to the client ID differently, for 
example, Application ID
```

### Client secret
```
Client secret is a unique token generated during the registration process and is tied 
to the client ID. As the name suggests, a client secret is private information and 
shouldn't be exposed. It is used internally while generating access tokens.
```

## Receiving grants
```
There are different kinds of authorization 
flows, two common ones of which are as follows:
• Authorization grant
• Implicit grant
```

### Authorization grant
```
An authorization grant consists of an authorization link, which looks like 
the following:

https://www.example.com/oauth/authorize?response_type=code&client_
id=CLIENT_ID&redirect_uri=CALLBACK_URL&scope=read

Let's break down the different components here:

• response_type: When set to code, the OAuth authorization server expects 
the grant to be of authorization grant type

• client_id: This is the client ID/app ID of the application

• redirect_uri: This contains a URL in percent-encoded form, and after the 
initial flow is complete, the authorization server redirects the flow to the 
specified URL

• scope: This refers to the level of access needed; this is implementation 
specific and varies

Example : 

Visiting the following link for an example:
https://www.example.com/oauth/authorize?client_id=2190698099&redirect_
uri= https%3A%2F%2Ftest.com%2Fredirect&response_
type=code&scope=read

results in a prompt inside the browser , As soon as the user allows the permission, the page redirects to the following:
https://test.com/redirect?code=af8SFAdas

the code we got can be exchanged now for an access token ,  this is generally done server side as a client secret must be involved

- Access Token = Auth Code + Client ID + Client Secret + Redirect URI
 
a POST request is sent to the authorization server with the preceding 
information: the authorization code, client ID, and client secret

https://www.example.com/ /oauth/token?client_id=2190698099&client_
secret=adb12hge&grant_type=authorization_code&code=af8SFAdas&redirect_
uri= https%3A%2F%2Ftest.com%2Ftoken

now the token is returned to https://test.com/token in JSON format, 
such as the following:

{ "access_token":" token" }

The authorization grant flow ends here. The third party can now access the resources 
of the user by sending appropriate API calls along with the access token to the 
resource server

```

### Implicit grant

```
The implicit grant link looks like the following:

https://www.example.com/oauth/authorize?response_type=token&client_
id=CLIENT_ID&redirect_uri=CALLBACK_URL&scope=read,write


It possesses traits similar to an authorization grant, but the major difference here is 
the response_type parameter, which is set to token.

This instructs the authorization server that the type we're going to use is implicit; other parameters work the same 
way as in the authorization grant.

See the following link for more information:

https://www.example.com/oauth/authorize?client_id=2190698099&redirect_
uri= https%3A%2F%2Ftest.com%2Ftoken&response_
type=token&scope=read,write

Loading the preceding link results in the permission prompt.
As soon as the prompt is allowed, the authorization server immediately redirects to the 
URL in redirect_uri with the access token in the URL itself, preceded by a hash for example 
(#), similar to this:

https://test.com/token#access_token=EAACEdEose0cBAE3vD

From now on, the third party can communicate with the resource server using 
this token.
```


## Exploiting Oauth
### Token hijacking (client):

```
If you are building an OAuth client,  
Thou shall register a redirect_uri as much as specific as you can or simply less formally
"The registered redirect_uri must be as specific as it can be".

DO register https://yourouauthclient.com/oauth/oauthprovider/callback 
NOT JUST https://yourouauthclient.com/ or https://yourouauthclient.com/oauth

if the oauth provider adopts an allowing subdirectory validation strategy, this could be exploited

In order to exploit this issue an attacker (me in this case) can create a public post ,link or something that can trigger an http request at https://yourouauthclient.com/profile/post , to https://Attacker.com/ leaked the token in the referer header


```

### Token hijacking (server):

```
be aware of the inherent risks involved in using 
redirect_uri in a grant situation where it can be made to redirect to a different 
location than the one allowed, thereby hijacking the access tokens

if the authorization server doesn't reject redirect_uri mismatches 
between application configuration and the actual one provided this is exploitable and we can steal the token by providing our attacker url .
```

### Oauth redirection bypass
```
-(server) Directory traversal trick , assume that we can save certain files of our choice under the allowed domain: 

http://example.com/token/callback/../../our/path
http://example.com/token/callback/.%0a./.%0d./our/path
http://example.com/token/callback/%252e%252e/%252e%252e/our/path
/our/path///../../http://example.com/token/callback/
http://example.com/token/callback/%2e%2e/%2e%2e/our/path

- try homograph attack

- subdomain ? tld suffix ?

- if subdomains are accepted in redirect url try :
http://example.com%2f%2f.victim.com
http://example.com%5c%5c.victim.com
http://example.com%3F.victim.com
http://example.com%23.victim.com
http://victim.com:80%40example.com
http://victim.com%2eexample.com

- check state parameter, doest the check happens ? :
this happen when there's a functionality like adding an account through oauth 
pause right after authorizing. You will then come across a request such as: https://yourtweetreader.com?code=asd91j3jd91j92j1j9d1 
After you receive this request, you can then drop the request because these codes are typically one-time use.
You can then send this URL to a logged-in user, and it will add your account to their account.

- brute force client secret ?

```


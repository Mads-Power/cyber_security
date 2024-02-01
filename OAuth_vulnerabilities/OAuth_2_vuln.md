source: https://portswigger.net/web-security/oauth
oAuth 2.0 code injection - Authorization injection attack:  
remiodiation: pixie?
reusing access tokens, the application/client may fail to expire used tokens. If the URL with the access token is leakd, then the attacker may be able to reuse the token. can lead to full account takeover.

Insecure redirect URI- "redirect_uri" not validated: failing to validate the redirect_uri properly. Can allow attacker to construct an OAuth login link, where the redirect_uri is an attacker-controlled domain. onece the victim logs in with the link, the attacker can capture the access token.

## CSRF

If the state parameter is not present, then the entire OAuth flow is vulnerable.
Regular CSRF bypasses may also work for eg; removing the state parameter.
Account Squatting: an attacker can register with the email address of the victim(this attack will not work if app has email verification). next, the victim logs in using Google/FB with the same email address. the attacker account and the victim's account are then tied to the same account.
Identifying OAuth authentication.
If you see an option to log in using you account from a different website it is a trong indicator that OAuth is used. Most reliable way to check is to proxy your traffic through Burp Suite and check the corresponding HTTP messages when you use this login option Regardless of wich OAuth grant type is being used, the first request of the flow will always be a request to the /authorization endpoint containing a number of query parameters that are used specifically for OAuth. In particular, keep an eye out for the client_id, redirect_uri, and response_type parameters.

## Recon

Once you know the hostname of the authorization server, you should always try sending a GET request to the following standard endpoints:
/.well-known/oauth-authorization-server
/.well-known/openid-configuration
These will often return a JSON configuration file containing key information, such as details of additional features that may be supported. This will sometimes tip you off about a wider attack surface and supported features that may not be mentioned in the documentation.

## OAuth grant types

The OAuth grant type determines the exact sequence of steps that are involved in the OAuth process. The grant type also affects how the client application communicates with the OAuth service at each stage, including how the access token itself is sent. For this reason, grant types are often referred to as "OAuth flows".
An OAuth service must be configured to support a particular grant type before a client application can initiate the corresponding flow. The client application specifies which grant type it wants to use in the initial authorization request it sends to the OAuth service.
There are several different grant types, each with varying levels of complexity and security considerations. We'll focus on the "authorization code" and "implicit" grant types as these are by far the most common.

## Authorization code grant type

![Authorization code grant type!](/assets/Authorization%20code%20grant%20type.png)

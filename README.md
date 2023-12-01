## Table of Contents
* [1 Introduction](#1-introduction)
  * [1.1 Glossary](#11-glossary)
  * [1.2. References](#12-references)
* [2 Client registration](#2-client-registration)
* [3 API specifications](#3-api-specifications)

# 1 Introduction
Welcome to the documentation for our Attribute Collection service, an innovative solution that leverages the OpenID Connect (OIDC) protocol for seamless authentication and secure attribute sharing transactions. This service is part of our ongoing research efforts, focusing on age verification in a pilot phase.

**Service Goals:**   
* Trusted Claims, Privacy-Preserving: Our primary aim is to provide trusted claims about individuals while safeguarding their privacy. We ensure that no personal data is disclosed, and only the minimum necessary information, as confirmed by the end-user, is shared.    
* Simplified Integration: We make it easier for our customers by offering a standard-based unified API that supports various authentication methods, such as Smart-ID, Mobile-ID, and future popular eIDs. This simplifies integration, streamlining the user experience.   
* Enhanced Security: We prioritize the security of relying parties (RP-s). By adhering to well-known OpenID and IETF standards and protocols, we ensure that your data and transactions are protected and follow best practices in the industry.

This documentation will guide you through the integration, configuration, and use of our age verification service. Whether you're a developer, a relying party, or simply interested in learning more, this documentation is your gateway to harnessing the power of our service. Let's get started on this journey to better age verification and data security.
## 1.1 Glossary
* Attribute - Any piece of information related to a person that can be optionally shared during an OIDC authentication transaction, providing additional user details. 
* ACS (Attribute Collection Service) - A service developed by SK to offer attribute storage, calculation and sharing functionalities, allowing for the secure and controlled exchange of user information during authentication.
* RP (Relying Party) - An organization or service that utilizes the SK authentication gateway service to authenticate its users and verify their age.
* OIDC (OpenID Connect) - A protocol used for central authentication, facilitating secure user verification and attribute sharing.

## 1.2. References

# 2 Client registration
> [!IMPORTANT]
> To initiate the registration process, we kindly request you to get in touch with our **Sales team**. You can find their contact information on our [Contact Us](https://www.skidsolutions.eu/contact/) page.

Each client has to be registered with the following required info:

| Parameter          | Description                                                                                        | Info                                   |
|--------------------|----------------------------------------------------------------------------------------------------|----------------------------------------|
| client-id          | OIDC client ID (Basic authentication user and `client_id` parameter in the authentication request) |                                        |
| client-secret      | Client secret used for client authentication (Basic authentication)                                |                                        |
| redirect-uri[]     | A list of allowed callback URI's whitelisted for this client.                                      | 1 or many URIs                         |
| ip-patterns[]      | A list of allowed IP patterns allowed for this client to access /token and /par endpoints.         | `192.168.12.*`, `192.168.*`, `0.0.0.0` |
| name               | Client full name, shown in frontend                                                                | Sample RP                              |
| logo               | Logo encoded in base64, preferably svg for removing issues with different user screen resolutions  | `base64 string`                        |
| header-color       | Frontend menu bar color, in hex format                                                             | `#ff0000`                              |
| tab-color          | Frontend tab-bar color (tabs for different auth methods), in hex format                            | `#009639`                              |
| button-color       | Frontend confirmation button color in input form, in hex format                                    | `#00ff00`                              |
| allowed-countries  | A list of allowed countries this client requires access to                                         | `EE`, `LV`, `LT`                       |

**Example 1:**

```
client-id: sample_rp_1
client-secret: changeme1
redirect-uris: https://sample-rp.com/callback
name: Sample RP
logo: 'data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iaXNvLTg4NTktMSI/Pg0KPCEtLSBHZW5lcmF0b3I6IEFkb2JlIElsbHVzdHJhdG9yIDE5LjAuMCwgU1ZHIEV4cG9ydCBQbHVnLUluIC4gU1ZHIFZlcnNpb246IDYuMDAgQnVpbGQgMCkgIC0tPg0KPHN2ZyB2ZXJzaW9uPSIxLjEiIGlkPSJMYXllcl8xIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHhtbG5zOnhsaW5rPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hsaW5rIiB4PSIwcHgiIHk9IjBweCINCgkgdmlld0JveD0iMCAwIDQ5Ni4xNTkgNDk2LjE1OSIgc3R5bGU9ImVuYWJsZS1iYWNrZ3JvdW5kOm5ldyAwIDAgNDk2LjE1OSA0OTYuMTU5OyIgeG1sOnNwYWNlPSJwcmVzZXJ2ZSI+DQo8cGF0aCBzdHlsZT0iZmlsbDojRDYxRTFFOyIgZD0iTTI0OC4wODMsMC4wMDNDMTExLjA3MSwwLjAwMywwLDExMS4wNjMsMCwyNDguMDg1YzAsMTM3LjAwMSwxMTEuMDcsMjQ4LjA3LDI0OC4wODMsMjQ4LjA3DQoJYzEzNy4wMDYsMCwyNDguMDc2LTExMS4wNjksMjQ4LjA3Ni0yNDguMDdDNDk2LjE1OSwxMTEuMDYyLDM4NS4wODksMC4wMDMsMjQ4LjA4MywwLjAwM3oiLz4NCjxwYXRoIHN0eWxlPSJmaWxsOiNGNEVERUQ7IiBkPSJNMjQ4LjA4MiwzOS4wMDJDMTMyLjYwOSwzOS4wMDIsMzksMTMyLjYwMiwzOSwyNDguMDg0YzAsMTE1LjQ2Myw5My42MDksMjA5LjA3MiwyMDkuMDgyLDIwOS4wNzINCgljMTE1LjQ2OCwwLDIwOS4wNzctOTMuNjA5LDIwOS4wNzctMjA5LjA3MkM0NTcuMTU5LDEzMi42MDIsMzYzLjU1LDM5LjAwMiwyNDguMDgyLDM5LjAwMnoiLz4NCjxnPg0KCTxwYXRoIHN0eWxlPSJmaWxsOiM1QjUxNDc7IiBkPSJNMjk0LjE4MSwxMDIuMDYzYzAuMDA2LTQuMzI0LTMuNTAxLTcuODI5LTcuODI2LTcuODI5Yy00LjMxNywwLTcuODI1LDMuNDk5LTcuODI1LDcuODI5djY3LjgxMQ0KCQljMCw0LjMzLDMuNTA4LDcuODI5LDcuODI1LDcuODI5YzQuMzI1LTAuMDA2LDcuODMyLTMuNTA1LDcuODMyLTcuODI5TDI5NC4xODEsMTAyLjA2M3oiLz4NCgk8cGF0aCBzdHlsZT0iZmlsbDojNUI1MTQ3OyIgZD0iTTM1OS43NTksMTM2Ljg0NmMtMy4wNTItMy4wNTgtOC4wMTItMy4wNTgtMTEuMDcsMGwtNDcuOTQ5LDQ3Ljk1Mg0KCQljLTMuMDU4LDMuMDU4LTMuMDUxLDguMDEzLDAsMTEuMDdjMy4wNjIsMy4wNTIsOC4wMSwzLjA1MiwxMS4wNzEsMGw0Ny45NDgtNDcuOTU4DQoJCUMzNjIuODE2LDE0NC44NTksMzYyLjgyMywxMzkuOTA1LDM1OS43NTksMTM2Ljg0NnoiLz4NCgk8cGF0aCBzdHlsZT0iZmlsbDojNUI1MTQ3OyIgZD0iTTM5My4wNDYsMjAwLjkySDMyNS4yM2MtNC4zMjQsMC03LjgyNiwzLjUxMS03LjgyNiw3LjgyOWMwLDQuMzI0LDMuNTA1LDcuODIyLDcuODI2LDcuODIyaDY3LjgxNg0KCQljNC4zMjEsMCw3LjgyNi0zLjQ5OCw3LjgyNi03LjgyMkM0MDAuODc1LDIwNC40MzEsMzk3LjM2NywyMDAuOTIsMzkzLjA0NiwyMDAuOTJ6Ii8+DQoJPHBhdGggc3R5bGU9ImZpbGw6IzVCNTE0NzsiIGQ9Ik0zMzguNTQ0LDI3Mi4xMzFMMjIzLjAyMSwxNTYuNjE0Yy03LjM1Ny03LjM3LTE5LjMwOS03LjM1Ny0yNi42NiwwLjAwNw0KCQljLTUuOTk1LDUuOTkyLTcuMTA2LDE1LjAxMi0zLjM0NiwyMi4xM2wtNjQuNTcxLDEyNy43OTdjLTAuMTUxLDAuMTUtMC4yODUsMC4zMTMtMC40MjUsMC40N2MtMC4xNTksMC4xNDItMC4zMjIsMC4yNzYtMC40NzQsMC40MjgNCgkJbC0yOC45NjcsMjguOTY2Yy00LjM4NSw0LjM5Mi00LjM4NSwxMS41MDUsMCwxNS44OWw0Ni4zMzQsNDYuMzM3YzQuMzg1LDQuMzc5LDExLjUwMSw0LjM3OSwxNS44ODcsMGwyOC45NjYtMjguOTcyDQoJCWMwLjE0NC0wLjE0NCwwLjI2OS0wLjI5NywwLjQwMy0wLjQ0NWMwLjE2Ny0wLjE0OCwwLjMzNy0wLjI4OSwwLjQ5Ni0wLjQ0OGw4Ljc3OC00LjQzNmMyLjQyMSw0LjAzNiw1LjM2Myw3Ljg0NCw4Ljg0LDExLjMyMw0KCQljMjIuOTA4LDIyLjkxMiw2MC4wNTIsMjIuOTEyLDgyLjk2LDBjMTcuMzI5LTE3LjMzMiwyMS41MzktNDIuODAxLDEyLjY1LTY0LjA5OGwxNS45MDMtOC4wMzYNCgkJYzYuNDIyLDEuOTIxLDEzLjY2OCwwLjM1MywxOC43NS00LjcyM0MzNDUuOTA2LDI5MS40NTMsMzQ1LjkwNiwyNzkuNDk2LDMzOC41NDQsMjcyLjEzMXogTTI3NC42NDUsMzU5LjA2Nw0KCQljLTEzLjc0MywxMy43NDMtMzYuMDI0LDEzLjc1LTQ5Ljc3NCwwYy0xLjY2OS0xLjY2OC0zLjEzMS0zLjQ2NS00LjM5NS01LjM1N2w2Mi4zNzctMzEuNTE3DQoJCUMyODcuMzUxLDMzNC42MzIsMjg0LjYyLDM0OS4wOTcsMjc0LjY0NSwzNTkuMDY3eiIvPg0KPC9nPg0KPHBhdGggc3R5bGU9ImZpbGw6I0Q2MUUxRTsiIGQ9Ik04NS44NTEsNjAuMzk0Yy05LjA4Niw3Ljg2LTE3LjU5NiwxNi4zNy0yNS40NTcsMjUuNDU2bDM0OS45MTQsMzQ5LjkxNA0KCWM5LjA4Ni03Ljg2MSwxNy41OTYtMTYuMzcsMjUuNDU2LTI1LjQ1Nkw4NS44NTEsNjAuMzk0eiIvPg0KPGc+DQo8L2c+DQo8Zz4NCjwvZz4NCjxnPg0KPC9nPg0KPGc+DQo8L2c+DQo8Zz4NCjwvZz4NCjxnPg0KPC9nPg0KPGc+DQo8L2c+DQo8Zz4NCjwvZz4NCjxnPg0KPC9nPg0KPGc+DQo8L2c+DQo8Zz4NCjwvZz4NCjxnPg0KPC9nPg0KPGc+DQo8L2c+DQo8Zz4NCjwvZz4NCjxnPg0KPC9nPg0KPC9zdmc+DQo='
header-color: '#ff0000'
tab-color: '#6cd649'
button-color: '#c621be'
ip-patterns:
    - 0.0.0.0
allowed-countries:
  - 'EE'
```

# 3 API specifications
## Authentication flow
### Initial request (`/par`)

OIDC requests are initiated via PAR (pushed authentication request). For this, a `POST` request with
content type `application/x-www-form-urlencoded` is sent to `OIDC_SERVICE/par` with the request body.

**Example 1:** Request person's full name and a verification that the person is over 18 years old:
```json
{
    "response_type": "code",
    "client_id": "CLIENT_ID",
    "redirect_uri": "REDIRECT_URI",
    "state": "STATE",
    "nonce": "NONCE",
    "scope": "openid name age_over",
    "age_comparator": "18",
    "code_challenge": "CODE_CHALLENGE",
    "code_challenge_method": "S256",
    "sid_confirmation_message": "FREETEXT",
    "mid_confirmation_message": "FREETEXT",
    "mid_confirmation_message_format": "GSM-7"
}
```

**Example 2:** Request person's given name, family name, and age:
```json
{
    "response_type": "code",
    "client_id": "CLIENT_ID",
    "redirect_uri": "REDIRECT_URI",
    "state": "STATE",
    "nonce": "NONCE",
    "scope": "openid given_name family_name age",
    "code_challenge": "CODE_CHALLENGE",
    "code_challenge_method": "S256",
    "sid_confirmation_message": "FREETEXT",
    "mid_confirmation_message": "FREETEXT",
    "mid_confirmation_message_format": "GSM-7"
}
```

| Parameter                       | Required                                               | Value                                                                                                                                                                                                                                                                                                           | Description                                                                                                                                                                                                                                                                                                                                                                                         |
|---------------------------------|--------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| scope                           | Yes                                                    | `openid` scope is mandatory to include by all clients.<br/>Other available scopes are: `personal_code`, `given_name`, `family_name`, `name`, `birthdate`, `age`, `age_over`, and `age_under`.<br/><br/>For age verification scopes, `age_over` and `age_under`, the parameter `age_comparator` must be provided | A space-separated list of strings defines the scope of the authentication. All supported scope values must be explicitly configurable per client. By default, only `openid` shall be supported for client.<br/>An HTTP 400 `invalid_scope` error is returned if the scope parameter value contains a scope that is not permitted by the client registration                                         |
| age_comparator                  | No (Yes when scope includes `age_over` or `age_under`) | int                                                                                                                                                                                                                                                                                                             | Age to do the age_over/age_under comparison                                                                                                                                                                                                                                                                                                                                                         | 
| response-type                   | Yes                                                    | Value must be `code`                                                                                                                                                                                                                                                                                            | The authorization code flow is                                                                                                                                                                                                                                                                                                                                                                      |
| client-id                       | Yes                                                    | Relying-party ID                                                                                                                                                                                                                                                                                                | A pre-registered relying-party identifier                                                                                                                                                                                                                                                                                                                                                           |
| redirect-uri                    | Yes                                                    | Relying-party callback URL                                                                                                                                                                                                                                                                                      | A pre-registered relying party callback URL                                                                                                                                                                                                                                                                                                                                                         |
| state                           | Yes                                                    | unique random string                                                                                                                                                                                                                                                                                            | RP generated opaque value used to maintain state between the request and the callback. Can be used to mitigate replay and CSRF attacks                                                                                                                                                                                                                                                              |
| nonce                           | Yes                                                    | unique random string                                                                                                                                                                                                                                                                                            | RP generated unique string. If provided, will be returned as a nonce claim in the id_token. Can be used to mitigate replay attacks                                                                                                                                                                                                                                                                  |
| ui_locales                      | No                                                     | Must be one of `et`, `en`, `lv` (or multiple in order of importance, separated by spaces)                                                                                                                                                                                                                       | End-User's preferred language for authentication. Determines the language used on the login page. The first found match with supported values will be set as the app language. If not set (or none are supported), app tries to determine the UI language from browser data etc. and defaults to `en`, if detection fails.                                                                          |
| code-challenge                  | Yes                                                    | Base64 encoded hash of a unique challenge                                                                                                                                                                                                                                                                       | Base64 encoded hash of a unique challenge (the code_verifier)                                                                                                                                                                                                                                                                                                                                       |
| code-challenge-method           | Yes                                                    | Must be `S256`                                                                                                                                                                                                                                                                                                  | The algorithm by which the challenge shall be verified on the server side.                                                                                                                                                                                                                                                                                                                          |
| sid_confirmation_message        | No                                                     | String with information for users for the SID authentication app                                                                                                                                                                                                                                                | Free text (max length 200 characters) value shown to Smart-ID users in the app during code choice. This can be the name of your service or provide more detailed info about the current interaction (like users name or order number etc).                                                                                                                                                          |
| mid_confirmation_message        | No                                                     | String with information for users for the MID authentication app                                                                                                                                                                                                                                                | Free text (max length 40 characters for `GSM-7`, 20 characters for `UCS-2`) value shown to Mobile-ID users before asking authentication PIN.                                                                                                                                                                                                                                                        |
| mid_confirmation_message_format | No                                                     | Must be one of `GSM-7` or `UCS-2`                                                                                                                                                                                                                                                                               | Specifies which characters and how many can be used in `mid_confirmation_message`.<br/>`GSM-7` allows `mid_confirmation_message` to contain up to 40 characters from standard GSM 7-bit alpabet including up to 5 characters from extension table ( €[]^&#124;{}\ ).<br/>`UCS-2` allows up to 20 characters from UCS-2 alpabet (this has all Cyrillic characters, ÕŠŽ šžõ and ĄČĘĖĮŠŲŪŽ ąčęėįšųūž). |

The oidc-service shall validate the authentication request and respond with a URI that the public
client can use to start authentication:

```
HTTP/1.1 201 Created
Cache-Control: no-cache, no-store
Content-Type: application/json
    
{
   "request_uri": "urn:ietf:params:oauth:request_uri:8Tw6nn6BAvoHBS5VM7M1UvndAAHdM5",
   "expires_in": 90
}
```
Please pay attention that client id and client secret should be passed in `Authorization` header of `/par` request in a format that OAuth 2.0 foresees for client secret authorization method.  
According to OAuth 2.0 framework, `Authorization` header must be in the `Authorization: Basic encodedString` format, where the `encodedString` is a result of Base64 encoding of OAuth client’s `clientID:clientSecret`.

Using the returned link, the app can open end-user's browser with the corresponding url.

### Authentication redirect (`/authorize`)

The app should open the link with an external user agent as recommended by current OAuth2 for native
clients best practices:

```
GET /authorize?client_id=EinLKYAAMqPr2Tw &request_uri=urn:ietf:params:oauth:request_uri:8Tw6nn6BAvoHBS5VM7M1UvndAAHdM5
```

After this, the user is redirected to the login service (located at `OIDC_SERVICE/login`)

### Response after successful authentication

A standard HTTP 302 authentication response redirect is returned to the end-user if a
successful authentication session exists. The redirect points user's browser back to RP's registered
redirect_uri where the RP shall verify the value of the state parameter.

```
HTTP/1.1 302 Found Location: https://client.example.org/callback?code=GaqukjWp4vvzEWHnLW7phLlwkpB0 &state=WDm4nExm1ADHzIEwoPxQ0KjBwnnk6NIrq178fU4rBDU
```

| Parameter    | Required | Value                | Description                                                                                                                 |
|--------------|----------|----------------------|-----------------------------------------------------------------------------------------------------------------------------|
| code         | Yes      | unique random string | A single-use, client-bound, short-lived authorization code, that can be used at OIDC token endpoint to redeem the id-token. |
| state        | Yes      | unique random string | RP's state parameter value specified in the Authorization Request                                                           |
| redirect_uri | Yes      | URL                  | RP's redirect_uri specified in the Authorization Request                                                                    |

### Requesting user info (`/token`)
Upon receiving the authorization code from the successful authentication response and having performed all checks required by OIDC core specification (state, csrf), the RP backend service posts a backchannel request to the oidc-service token endpoint:

```
POST /token HTTP/1.1
Host: auth.sk.ee
Content-Type: application/x-www-form-urlencoded
Authorization: Basic c2FtcGxlX2NsaWVudF8xOjNkc8OEMDIrMSwubTExMmxrMjPDtmxrw7ZsazMyMw

grant_type=authorization_code
&code=GaqukjWp4vvzEWHnLW7phLlwkpB0
&code_verifier=yJhbGciOiJSUzI1NiIsImtpZ
&redirect_uri=https%3A%2F%2Fclient.example.org%2Fcallback
```

| Parameter     | Required | Value                        | Description                                                                                                |
|---------------|----------|------------------------------|------------------------------------------------------------------------------------------------------------|
| grant_type    | Yes      | Must be `authorization_code` | Static value required by the OIDC core protocol.                                                           |
| code          | Yes      | Unique random string         | Authorization code value returned from the OIDC-service                                                    |
| code_verifier | Yes      | Unique random string         | The sha256 hash that was used as an input value for the code_challenge sent in the authentication request. |
| redirect_uri  | Yes      | URL                          | Must be identical to the parameter value that was included in the initial Authorization Request            |

The JSON response contains a signed JWT in the field id_token that contains claims about the authenticated end-user: 
```
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: no-store
Pragma: no-cache
 
{
    "access_token": "SlAV32hkKG",
    "token_type": "Bearer",
    "refresh_token": null,
    "expires_in": 3600,
    "id_token": "eyJhbGciOiJSUzI1NiIsImtpZ...LrLl0nx7RkKU8NXNHq-rvKMzqg"
}
```

| Parameter    | Value                | Description                                      |
|--------------|----------------------|--------------------------------------------------|
| access_token | Unique random string | An opaque random string                          |
| token_type   | `Bearer`             | Access token type. Will be `Bearer`              |
| expires_in   | 600                  | Access token expiration time in seconds          |
| id_token     | JWT                  | A serialized and signed JWT with end-user claims |

The RP must validate the id_token signature and claims as specified in the OIDC core specification.

| Claim                             | Description                                                                                        |
|-----------------------------------|----------------------------------------------------------------------------------------------------|
| sub                               | Subject identifier                                                                                 |
| given_name, family_name, ...  etc | Standard claims that describe the end-user                                                         |
| jti                               | A unique identifier for the token, which can be used to prevent reuse of the token.                |
| iss                               | Issuer URL. Issuer identifier for the issuer of the response.                                      |
| aud                               | Audience that this ID Token is intended for. Contains the Relying Party ID as an audience value.   |
| iat                               | Time at which the JWT was issued.                                                                  |
| exp                               | Expiration time on or after which the ID Token MUST NOT be accepted for processing.                |
| nonce                             | String value. Used to associate a Client session with an ID Token, and to mitigate replay attacks. |
| amr                               | Authentication Methods References.                                                                 |

## Error tracing

User interface generates a random id on load (example: `95b69c87-aacc-49b3-9577-5a76491792e7`) that is used to trace requests made to services (like getting session information, initializing authentication etc). If a user is experiencing problems and records the incident number shown in the error message, it can be used to help diagnose the problem.

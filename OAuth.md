# Common information

Not good:

    To provide your password to another app for authorization access to antother app;
Good: 

    To provide some 'key' instead which provides some your resourses you agree with. Additionaly you can take back the key. 


## OAuth 2.0 vocabullary

Authorization code

: It's authorization flow when one app provide some another app link to login there and get the key.

Resource owner (Identity)

: It's a anyone or anything, who is owner of authorizated data.

Client

: An app or anything like that who want use your data from another app.

Authorization Server (If it supports OIDC it called also an Identity provider)

: An app who knows **resourse** owner and has his account

Resource Server

: Place with resourses wich **client** want to use. (Sometimes Authorization
Server and Resource Server are the same server)

Redirect URI (Callback URL)

:  This is an URL to which **Authorization Server** will redirect **Clinent** when
access to resources for **Client** is granted.

Responce Type

: The type information **Client** expects to receive. Usually it's 'Code' or
'Authorization Code'

Scope

: Granual permissions the **client** wnats

Consent

: **Authorization Server** tries to agree with **resourse owner** about **Scope** for **Client** permission

Client Id (App id)

: This is an Id to identify for **Authorization Server** of **Client**

Client Secret (App secret)

: This is a password known between only **Client** and **Authorization Server**
to share information privetly behind the scenes.

Authorization Code

: Short lived authorization conde that **Authorization Server** sends to the
**Client**. Then **Client** going to send to **Authorization Server** along with
**Client Secret** in exchange for an **access token**

Access token

: It's a Key that **Client** use to communicate with **Resource Server**

## OIDC

OAuth was designed only for **Authorization**!!! for granting an access to data.
It gives to client a key and the key doesn't tell who are you or anything like
that. 

So there is an OPENID CONNECT (OIDC) which wraps/extends the OAuth and keep the
profile information about the person who is loged in.

Intead the key OIDC give to app something like a badge. The badge gives to
**Client** not only information about permission but additionaly it give
information about who you are.

OIDC establish the login session (Authentication)

OIDC enabels an scenario when a user's login can be used for a multiple
applications also known as **Single Sign On** (SSO)

*Example*

Like atm and card. The card keeps information about your account and some
metadata like name or expiration date which is used by ATM and related services
to give you money.

OPENID

: Will be passed with the first request to **Authorization Server** as
information about the need of OPENID

ID TOKEN (IDENTITY TOKEN) or a specific formated json string known as JSON WEB
TOKEN (JWT)

: will be contained inside a response with **Access Token** and then **Client**
will be able to decode information to retreive usefull and unerstandable
information for the **Client**

CLAIMS

: It's the data inside JWT

Thanks to OIDC the **Client** can ask **Authorization Server** for some
additional information like email address using **Access Token**

---
layout: post
title: How does OAuth2.0 work in the real world?
date: '2012-12-27 18:38:00'
tags:
- from-wordpress
---

OAuth 2.0 needs no introduction (read the [excellent introduction in the IETF spec](http://tools.ietf.org/id/draft-ietf-oauth-v2-23.html#rfc.section.1) if you need one). This blog is an easy to read explanation of OAuth 2.0 protocol with explanations on how Facebook has implemented it. In the interest of making it easier to read, I'll explain this as if it were a 3-act play.

Let me first set the plot of this play. There is a User, who wants an Application (lets say yet another super-innovative photo sharing application) to do something\_useful. However, and here comes the twist, in order to do something\_useful the Application needs access to some of the User's resources (say some photos) which are held for safekeeping by the Resource Server, played in this drama by facebook. As you can discern from the plot - there are three main characters in this play -

1. User
2. Application (App)
3. Resource Server (ReS) aka Facebook (FB)

However, as you will soon see, there is a fourth hidden character - the Authorization Server (AuthS) who has the most pivotal part to play!

According to OAuth 2.0, this play (like most plays before it) is composed of three acts. Like I mention above, this play kicks off by the User asking the App to do something\_useful.

USER --- GET /something\_useful --\> APP

## 3 Act Overview

This section gives an overview of the 3 acts, without going into the dialogues and the script of each act.

**_ACT 1: User gives App an authorization grant._**

According to the OAuth 2.0 protocol, App will need an authorization grant from the User when requesting access to the User's protected resources. In an ideal world, the User would have a stack of such authorization grants lying around on her hard drive that she can just give to the App. The reality is obviously quite different as we'll see later in this blog.

**_ACT 2: AuthS gives the App an access\_token in exchange for the authorization grant._**

This is where the Authorization Server makes his first appearance. The cool thing about the AuthS is that all three characters in our play - User, App, and Resource Server trust the AuthS with their secrets. We will see how this plays a pivotal role in our play later in this blog.

**_ACT 3: ReS gives App access to protected resources in exchange for the access token._**

The Resource Server trusts the access\_token issued by the AuthS (see how the trust between AuthS and ReS comes into play!). It lets the App have access to the User's resources for a short while.

## The Script

Let us now delve into the details of these acts.

### ACT 1: User gives App an authorization grant.

User and App are on the stage.

_ **User:** Do something\_useful for me._

USER --- GET /something\_useful --\> APP

_ **App:** I cannot, unless you give me your authorization grant._

Now, in the simple version of this play, the User has a stack of these authorization grants lying around. But in this more "real world" enactment, if the App were to say something silly like this, she would just go to another app. So instead our savvy App tells the User how to get an authorization grant by doing an HTTP redirect to the AuthS. Note, all the parameters of the GET request here. The state parameter is something that will help the App from being a victim of CSRF further down the road.

USER \<-- 302 https://fb.com/oauth? client\_id=:app\_id& redirect\_uri=http://app/something\_useful& state=xxx -- APP

Now, AuthS makes his entry and he is actually the same as the ReS aka Facebook! Without explicitly doing so, this redirect URL causes the User to ask the AuthS aka Facebook for the authorization grant.

_ **User** (to facebook): Give me the authorization grant._

USER --\> GET /oauth?client\_id=:app\_id&redirect\_uri=http://app/something\_useful&state=xxx --- AUTHS (FB)

_ **AuthS** : Give me your secret (facebook login and password), here is the login form._

USER \<-- 200 login form --- AUTHS (FB)

_ **User:** Now, I trust you Authorization Service, after all I have asked you to protect some of my resources for me. Here is my secret._

USER --- POST user fb creds ---\> AUTHS (FB)

Now, if the User were to just receive the authorization grant from the AuthS, she would get fed-up with this nonsense and just stop using the App. So the wise AuthS redirects the user to the App instead. Remember, the AuthS knows where to redirect because it received the redirect\_url from the User, who in turn received it from the App.

_ **Auths** : Go to the App and give him this packet. He'll know what to do with this._

USER \<-- 302 http://app/something\_useful?code=:authgrant&state=xxx --- AUTHS

_ **User** (to App): Here take this authorization grant. Now, do that something\_useful for me._

USER --- GET /someting\_useful?code=:authgrant&state=XXX ---\> APP

Act ends here.

### ACT 2: Authorization Server gives the App an access\_token

Authorization Server and App are on the stage.

_ **App** : Here take this package containing the authorization grant and give me the access\_token so I can borrow User's protected resources. Oh and I trust you with my own secrets as well. So here are my creds (app id and app secret), so you know it is indeed me, and can hand me the access\_token._

APP --- GET https://fb.com/oauth/access\_token? client\_id=:app\_id& redirect\_uri=:xxx& client\_secret=:app\_secret& code=:authgrant ---\> AUTHS (FB)

AuthS opens the authorization grant package. We can see that it contains a message from the User authorizing the App to get an access\_token. And it is signed by the Authorization Service himself. Authorization Server validates his own signature, makes sure that the App requesting it is the same app mentioned in the authorization grant, and then hands over the access token to the App.

_ **AuthS** : Hmm..everything checks out fine. Here is the access\_token._

APP \<-- 200 access\_token and expiry --- AUTHS (FB)

End of Act 2.

### ACT 3: Resource Server gives App access to protected resources

App and Resource Server are on the stage.

_ **App** : Here is the access\_token. Let me borrow the User's protected resources._

APP --- GET /me?access\_token=:access\_token ---\> RS (FB)

The ReS trusts the access\_token that was provided by the Authorization Server (obviously as he was the Authorization Server) and grants temporary access to the App. Note, the App does not have to authenticate himself to the ReS. App authentication only happens with the AuthS.

APP \<--- 200 User's resources --- RS (FB)

## Conclusion

And with that this merry tale comes to an end. Here is the snapshot of all the HTTP request/responses at a glance.

USER --- GET /something\_useful --\> APP USER \<-- 302 https://fb.com/oauth? client\_id=:app\_id& redirect\_uri=http://app/something\_useful& state=xxx -- APP USER --\> GET /oauth?client\_id=:app\_id&redirect\_uri=http://app/something\_useful&state=xxx --- AUTHS (FB) USER \<-- 200 login form --- AUTHS (FB) USER --- POST user fb creds ---\> AUTHS (FB) USER \<-- 302 http://app/something\_useful?code=:authgrant&state=xxx --- AUTHS USER --- GET /someting\_useful?code=:authgrant&state=XXX ---\> APP APP --- GET https://fb.com/oauth/access\_token? client\_id=:app\_id& redirect\_uri=:xxx& client\_secret=:app\_secret& code=:authgrant ---\> AUTHS (FB) APP \<-- 200 access\_token and expiry --- AUTHS (FB) APP --- GET /me?access\_token=:access\_token ---\> RS (FB) APP \<--- 200 User's resources --- RS (FB)


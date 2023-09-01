# Sam-web-template
<https://build.envato.com/my-apps/>
Envato API
OverviewAPISign inTerms
API Documentation
Computers talking to computers.
Let the fun begin!
Welcome
This is where the fun begins. Time for you to get familiar with the new Envato Market API. You'll find here all the documentation, from basic information to the how-tos and the beloved lines of code. That's all you need to start playing around with our API, unlock the superpowers, release the kraken, hack away.

The Basics
What is an API?
See up there, where we say "Computers talking to computers"? That's kind of the TL;DR version of what an API is. The acronym stands for Application Programming Interface. Want to build an app that can access the data and/or features of another application? An API is a set of procedures and functions that will allow you to do so.
What do we mean by "app"?
You guessed it - Envato Market has an API. This means you can build an app that hooks into Envato Market via the API and accesses account data (such as sales count) and/or features (such as search) - and makes life easier for you and potentially millions of users! This could be a mobile app, a website, or any single service that allows users to access Envato Market's resources.
What services are available?
Here is a list of the data and features you can access via the API. Stay tuned for more!
Discovering content. You can access all resources available on Envato Market, including:
The item and comment search feature, with all of our new facets and filtering options;
Details on authors and their items;
Public collections (as well as your private ones).
Account information. This includes:
Your account details, including your balance;
Your purchases: you'll be able download the items you've recently bought;
Your sales: including verifying purchase codes of items you've sold.
Apps currently available
Want to see some of the apps that our community has already created? Have a look, be amazed!
Get Started
Register your app
You surely hate nasty annoying bots just as much as we do. So you'll understand why we ask you to register every app you create - we want to make sure that our API is only used to build awesome products for our community. It won't take long anyway. We'll ask you to provide a few details including the name and a description of your app, as well as the permissions that the user will need to grant in order to use it. To use the API, you'll need to agree to our API Service Terms and Conditions - we promise that they're friendly and easy to read, so make sure you look them over before you register.
Your application key
Once you've registered your app (and got us super excited because we can't wait to see what you're creating there), you'll get an application key. It's a unique code that allows you to connect with our API. You can always find all your registered applications in the "Manage your apps" page. Remember to keep the key secret - you don't want other people to use the API on your behalf!
If you're not going to build an app that requires useres to log in, and want to access the API directly yourself, you can simply create a personal token.
Authenticating users
With the old API, authentication was a bit of a pain. Users had to go to Envato Market, log in, retrieve an API key and then copy-paste it into your app. Not anymore! With the new API, users will be redirected from your app to a login page that we will provide, and once they have successfully logged in we'll send them back to the app. If it's the first time they've logged in to your app, they will also have to grant the permissions your app requires.
You can find more detailed info about the whole OAuth flow below.
Share your work
Once you've created your app, you'll sure want to let our community know that there's a new neat product to help them make the most out of Envato Market. Shoot us the link, and we'll showcase it on our Market Blog!
Authenticating with OAuth
The Envato Market API supports the full OAuth authentication flow, so that you can allow users of your app to sign in with their Envato Account in order to access the API on their behalf. Using OAuth authentication is simple:
First, register your application so that users know which permissions your app needs. Make a note of the secret application key, and OAuth client ID, that you'll be given during registration - they'll come in handy.
Before your app accesses the Envato API, you'll need to authenticate the user. Your app should redirect the user to the following location:
https://api.envato.com/authorization?response_type=code&client_id=[CLIENT ID]&redirect_uri=[REDIRECT URI]
You can replace [CLIENT ID] with the OAuth Client ID of your app from the My Apps page, and [REDIRECT URI] with the fully-qualified URL you'd like to send the user back to once authentication is successful (this must match the Confirmation URL setting configured when you created your app - you can edit it from the above page).
After the user has logged in and given their permission, the Envato API will redirect them back to your application on the Confirmation URL provided, with a single-use authentication code provided in the query string (eg. http://your.app/callback?code=abc123...). You must use this code to request an access token from the API, by sending the following POST request from your server (encoded as application/x-www-form-urlencoded), replacing [CODE] with the code you've just received, [CLIENT_SECRET] with your secret application key, and the other fields as necessary:
POST <https://api.envato.com/token?grant_type=authorization_code&code=[CODE]&client_id=[CLIENT_ID]&client_secret=[CLIENT_SECRET]>
The server will respond with an access token:
example of token 
{
  "refresh_token": "GBdxWsxo1CqAK9yCneH75wgkXw1q7bio",
  "token_type": "bearer",
  "access_token": "c0lQ2WLYW9qAZ9RH12cH1fJPzVWSscXP",
  "expires_in": 3600
}
You can use the access token, which is valid for up to an hour, to make requests to the API on behalf of the logged-in user, by adding it as a bearer token to the Authorization header on any subsequent requests:
curl -H "Authorization: Bearer c0lQ2WLYW9qAZ9RH12cH1fJPzVWSscXP" https://api.envato.com/v1/market/private/user/account.json

{
  "account": {
    "image": "https://0.s3.envato.com/files/100000009/0006893824_192.jpg",
    "firstname": "Test",
    "surname": "User",
    "available_earnings": "0.00",
    "total_deposits": "0.00",
    "balance": "0.00",
    "country": "Australia"
  }
}
After the one-hour period is up and the access token has expired, you can continue using the API on the user's behalf with the refresh token that was originally returned. You can use the refresh token to request a new, valid access token with the request below, and then use it for subsequent requests to the API:
POST https://api.envato.com/token grant_type=refresh_token& refresh_token=[REFRESH_TOKEN]& client_id=[CLIENT_ID]& client_secret=[CLIENT_SECRET]
The server will respond with a new access token:
{
  "token_type": "bearer",
  "access_token": "x0lQ2WLYW9qAZ9RH12cH1fJPzVWSscXZ",
  "expires_in": 3600
}
Note: the Envato API also supports the alternate, implicit OAuth authentication flow, if you are developing a browser-based app and don't wish to include the application's secret key in a place where it could be read by users.
Authenticating with a Personal Token
It may be that your app doesn't require to assume the role of a third-party user to access the Envato API - for example, if your app just accesses public data, such as performing searches for items on Envato Market. In this situation, rather than the complication of implementing OAuth and requiring users to log in and grant permissions to your app, you can simply generate a personal token, and then insert it directly into the authorization header on any API requests:
curl -H "Authorization: Bearer oVi4yPxk1bJ64Y2qOsLJ2D2ZlC3FpK4L" https://api.envato.com/v1/market/total-items.json

{
  "total-items": {
    "total_items": "7411418"
  }
}
Rate Limiting
All requests to the Envato API are dynamically rate limited. As such are unable to provide static values as these will vary between clients. To determine when we enforce these rate limits we use a combination of factors, including, but not limited to:
Number of previously made requests
Requested endpoint (some requests require more resources than others so we limit these more tightly)
Client IP reputation
If you do manage to cross the acceptable number of requests you are permitted to make, you will receive a HTTP 429 response. Included in this response is a HTTP header Retry-After that will denote in seconds how long you have to wait until you're able to make requests again. It's important that if you're building automated systems to make requests on your behalf, that you take this into account and handle this scenario gracefully in your application.
API Method Listing
<openworkspacesource@gmail.com>

OverviewAPIMy appsTerms and Conditions


OverviewAPISign inTerms
API Services Terms and Conditions
Welcome to the Envato API Service
Hi, we’re Envato, and welcome to the Envato Market Application Programming Interface (‘API’). If you’re looking to make an app or a website (‘application’) in connection with Envato Market, then you’ve come to the right place. We want to encourage everyone to get creative, and get involved in our online community.
If you create an application, we’d love to hear about it! We can help you make your application accessible to other members by putting it up on the API site at build.envato.com.
When we say ‘we’, ‘us’ or ‘Envato’ it’s because that’s who we are and we own and run the sites. When we refer to the ‘sites’ we’re talking about the Envato Market platform at market.envato.com, and all of the creative environments that we offer through that platform. When we say you, it includes any organisation you work for if you’re accessing the API in the course of your employment.
We’d love to help you out with your application by giving you access to our API. We think it’s important that we’re all on friendly terms first though, so you’ll need to join us as an Envato member before we can get started (don’t worry, it’s free!). Head over here to read the Envato Market Terms and find out about being a member.
This agreement explains the specific rules that apply to using the API (‘user terms’). Make sure to carefully read these user terms, together with the Envato Market Terms and our Privacy Policy, because by using the API you’re making a legal commitment to us to follow them, and any use of the API that doesn’t comply is unauthorised.
What's ours (is not yours)
Our talented developers have put together the API to make things easier for people like you to get creative. That means we own the API and any rights attached to it.
Our user-generated material, like items, usernames, email addresses, product descriptions, avatars, and related images or information (‘information’), generally belongs to our members, or might belong to third parties, but they’ve given us the right to use it in certain circumstances - including letting you access and use it in accordance with our user terms.
Use of Envato Brand
You can use the word ‘Envato’ in the product description, or in a descriptive subtitle to tell users what it does, e.g. "AuthorWorks – The Envato Market Author App".
You can’t however include the word ‘Envato’ in the application name itself. We don’t endorse your application (even if it’s really cool), so please don’t suggest we made it, or that it’s official in any way.
All your applications need to acknowledge your use of the Envato API, and give us a shout-out by including the “Powered by Envato API” logo in the application.
Your rights and obligations
Subject to your compliance with these user terms, we grant you a limited, non-sub-licensable, non-transferable, non-exclusive, revocable, license to use the API solely as necessary to create and run your applications (‘license’).
What you must do:
You have to be an Envato member if you want to be a part of our community and, importantly, if you want to use the API. As long as you use the API, or any content or information accessed through it (and as long as your application is available for use), you need to be a member.
All your applications must be registered with us and require the relevant key to access any part of the API (application key).
If a member is required to log in to their Envato member account to use your application, you must direct them to the existing OAuth log-in page, as specified in our documentation. You must not require a member to log in through any other page or process.
Always act in good faith in your use of the API, the content, and the information.
What you can't do:
Don’t use the API, the content, or the information in any manner or for any purpose that violates any law or regulation, or the rights of any person, including but not limited to intellectual property rights, rights of privacy, or rights to personality.
Don’t attempt to circumvent any security measures in relation to the API or the sites and don’t use the API in a manner that disrupts or otherwise negatively affects other people’s ability to use the sites or their member accounts.
Don’t share your application key with anyone else, or use someone else’s key to access the API.
Don’t cache or store any information or content other than for reasonable periods and in order to develop and provide your application. Don’t display item information or product information and/or related images which are more than six (6) hours older than they are on the sites. This could be misleading to users.
Don't make any modifications or amendments to our content. Don’t use our content in any way that could be construed as misleading or deceptive, or as an attempt to pass yourself off as us.
Don’t use the API for a commercial purpose, except in compliance with our rules about commercial use, below.
Commercial use
You may want to charge people a fee to use your application. It’s okay if you do, but there are a few things you need to keep in mind:
All paid applications must be made available for purchase through CodeCanyon. This isn’t exclusive (you can also make it available through any other platform or store) but will ensure that all of our members can access your application!
You have to comply with any third party terms that apply if you sell your application through another platform.
Make sure that you are complying with the terms of any third-party licence that might apply to other material (like code or graphics) you used to create the application, and check to see that you are allowed to sell a product using that material. You may need to pass along any rights or releases.
Privacy
Some of the information that can be accessed through use of the API is publically available information, which you can generally get through the use of a personal token. Some of the information can only be accessed through the use of your own application key, and by requiring another member to log in to your application. In both cases, we expect you to handle that information with care and only in compliance with these user terms, and with the standards set-out in our Privacy Policy.
When you are authenticated to access another member’s account, you may have access to personal information about that member. Don’t try to access, use, or disclose that information for any purpose other than one your application requires, and for which you have the member’s permission. Make sure that your application is secure and doesn’t provide anyone with the ability to access information that they shouldn’t.
Don’t ask members for their passwords, names, or any personal information through correspondence or in your application. If you do collect that sort of information, you have to comply with all relevant laws about privacy and data handling, and any other applicable law, and you are solely responsible for the consequences.
Right to change, suspend, or terminate
We might make changes to the API or the sites, or to our content, or how our content or the information is displayed or described, at any time. Those changes might interfere with the operation of your application but, unfortunately, that’s a risk you agree to accept if you use the API. We aren’t liable for any related loss, cost, or damage – see our liability & indemnity section below.
Although we can change any of our user terms, or the terms of our license, at any time, we will take reasonable steps to let you know when we do so.
We can suspend or terminate your membership at any time for any breach of these user terms or if we think you application adversely affects our reputation or the sites in any way. You can terminate your membership at any time. Suspension or termination of your membership for any reason will mean you can’t use the API or any content or information gained through the API any more, but the rest of the user terms will still apply. You must immediately disable your application.
Your indemnity to us
You are liable and responsible for:
Any applications you may create, develop, change or distribute;
your use of the API, the content, and the information;
your disclosure of any of the information;
the services, message, content, information, software or other materials you provide through the application;
your breach of any intellectual property rights belonging to others;
your breach of these user terms; and
your breach of any industry code, regulation, third party contract or terms of service, or law that may apply.
We have no responsibility to you or to any other person for all liabilities, costs, expenses (including legal fees) and loss arising from third party claims due to any of the matters set out in this section and you agree to indemnify us, our directors, officers, employees and agents from all losses. This means that you protect us from costs and claims that happen because of your actions on the site.
We reserve the right, at our own expense, to assume the exclusive defence and control of any matter otherwise subject to indemnification by you, and in such case, you agree to cooperate with our defence of such claim.
You should carefully assess whether the API, the sites, the content, and the information are suitable for your needs. Everything provided by us through the Envato API is given on an ‘as is’ basis and without warranties, either express or implied.
We strive to have the API available to you 24 hours a day, seven days a week but you know how the internet works: occasionally you might not be able to access the Envato API, and this might happen for any reason, at any time, with or without notice, or at our absolute discretion. We might also change aspects of how the API works. We will not be liable to you for any loss you suffer as a result of these things.
Legal stuff
We’re from Australia, and that’s where our Envato API Service is based. The laws of Victoria, Australia govern these user terms, and you submit to the jurisdiction of the courts here by using the API.
Definitions
Looking for the meaning of a word or phrase? Here are the words we’ve defined in these user terms and where you can find their meaning. If you can’t find the meaning of a particular term we use here, you might find it defined in the Envato Market Terms.
Definition	Clause
API	3
application key	11(b)
content	6
information	7
item	Envato Market Terms
member	Envato Market Terms
sites	2
user terms	4

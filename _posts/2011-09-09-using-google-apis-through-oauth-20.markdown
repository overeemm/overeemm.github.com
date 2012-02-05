---
layout: posts
title: Using Google APIs through OAuth 2.0
categories: [C#, .NET, OAuth, gdocerous]
---

Lately I’ve been working on a web application that connects to Google docs: gdocerous. During the development I’ve stumbled up on OAuth and other authorization protocols. This blog post is a small write-up of the experience.

As the major cloud provider, Google has lots of content available through their applications. Fortunately, they also have a nice API with lots of support and samples. But also lots of ways to connect and authenticate. They support AuthSub (their own proprietary protocol), OAuth 1.0 and OAuth 2.0 (still in beta), but also a plain user name and password connection. They do recommend that you use OAuth, and because the specs for OAuth 2.0 seem stable enough, I would recommend to use the 2.0 version. This post shares some example code on how to use it in a .NET web application.

![OAuth logo](/images/oauth.png)

# OAuth 2.0 in short

The OAuth 2.0 protocol is an evolution from OAuth 1.0 (really? yes it is!). If you are interested in knowing why a new version was necessary, I suggest that you read the following article from Eran Hammer-Lahav: Introducing OAuth 2.0.

The global idea is that you redirect a user to the OAuth-server. There he can authenticate and grant access to the application that redirected him in the first place. The user gives access for a certain scope. In the case of Google, this scope can be Google docs, gmail or some other application. It is of course important that the scope for which access is requested is as narrow as possible. After the user has authorized the application, the OAuth-server redirects to the application, giving an authorization code along. This code can be used to request an access token. And finally, the token can be used to access APIs and resources.

The main advantage is that the user does not have to give his user name and password to the application. Instead the application requests access and receives a short-lived access token. This makes it easy to revoke the access in a later stage, without having to change any credentials.

Is all good in the Google OAuth 2.0 world? No, not really. Some things remain hard. For instance using Google’s IMAP or SMTP. Although the protocol documentation is clear, there is no easy way to use this from .NET.
Show me the code!

So how do you get started with these APIs? Let me walk you through the basic steps.

First, register your app in the Google APIs console. This gives you a clientid and clientsecret. It also allows you to give your application more trust by supplying a description and logo. This way users can recognize the application that asks for trust.

On requesting authorization, your application starts with redirecting the user to https://accounts.google.com/o/oauth2/auth, following the description of Using OAuth 2.0 to Access Google APIs. This part is not that hard. Users are shown a recognizable Google page on which they grant access for your application.

After these two steps we need to write some real code. First retrieve the code from the request en get an access token from Google.

    string queryStringFormat = @"code={0}&client_id={1}&client_secret={2}&redirect_uri={3}&grant_type=authorization_code";
    string postcontents = string.Format(queryStringFormat
                                       , HttpUtility.UrlEncode(code)
                                       , HttpUtility.UrlEncode(clientid)
                                       , HttpUtility.UrlEncode(clientsecret)
                                       , HttpUtility.UrlEncode(redirect_url));
    HttpWebRequest request = (HttpWebRequest)HttpWebRequest.Create("https://accounts.google.com/o/oauth2/token");
    request.Method = "POST";
    byte[] postcontentsArray = Encoding.UTF8.GetBytes(postcontents);
    request.ContentType = "application/x-www-form-urlencoded";
    request.ContentLength = postcontentsArray.Length;
    using (Stream requestStream = request.GetRequestStream())
    {
        requestStream.Write(postcontentsArray, 0, postcontentsArray.Length);
        requestStream.Close();
        WebResponse response = request.GetResponse();
        using (Stream responseStream = response.GetResponseStream())
        using (StreamReader reader = new StreamReader(responseStream))
        {
            string responseFromServer = reader.ReadToEnd();
            reader.Close();
            responseStream.Close();
            response.Close();
            return SerializeToken(responseFromServer);
        }
    }

The result is a JSON string that, with the help of DataContractJsonSerializer, can be serialized into

    public class GoogleOAuthToken
    {
        public string access_token;
        public string expires_in;
        public string token_type;
        public string refresh_token;
    }

The given access token comes with an expiration time. To extend your timeperiod, you can use the refresh token to request a new access token. The code to do this is similar to the example above.

After this we can finally get to work. Let’s get a list of all folders from google docs.

    DocumentsRequest request = new DocumentsRequest(new RequestSettings("your appname", "oauth access token"));
    foreach (Document doc in request.GetFolders().Entries)
    {
        /* do something */
    }    

This snippet uses the Google Data Protocol, using the C# library available from google code. The Data Protocol it self is REST-inspired, but the library gives a nice object-oriented wrapper around that.

The library wraps all major APIs from Google, so anything is possible.

Have fun coding!

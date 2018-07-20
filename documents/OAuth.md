# Implementing OAuth in Databox


## Authentication Callback

The callback url is passed to the OAuth authenication process and allows the authentication page to redirect the user 
back to the driver. This url differs between the app and web versions of the container manager and so the container 
manager passes the url to the driver as a query parameter when the root ui page is requested. 

The twitter driver extracts this from the url and passes it to twitter when the twitter login process is started.

eg.
```
https://localhost/driver-twitter/ui?oauth=http%3A%2F%2Flocalhost%2Fdriver-twitter%2Fui
```

## Redirecting to the Authentication Page

Many oauth procedures don't allow you to display the page which grants permission from within a frame or a WebView. 
These are exactly the technologies the container manager uses to display the driver's user interface. So, the driver 
needs to escape from the frame to display the page. You can do this by posting a message to the container manager.

```javascript
window.parent.postMessage({ type:'databox_oauth_redirect', url: 'https://api.twitter.com/oauth/authenticate?oauth_token=' + token}, '*');
```

In the app, will use a version of Safari (iOS) or Chrome (Android) to open the given page, where the web version will
redirect the browser. 

In the twitter driver, the authentication process is triggered from a post request. An otherwise empty web page is 
returned from the request with the above code in a script tag to redirect once the post request finishes.

See the [twitter driver](https://github.com/ktg/driver-twitter/tree/oauth) for an example of how OAuth can be implemented

---
title: Calling APIs
description: This tutorial will show you how to use Auth0 to authenticate with your own API server.
---

<%= include('../../../_includes/_package', {
  org: 'auth0-samples',
  repo: 'auth0-ios-objc-sample',
  path: '08-Calling-APIs',
  requirements: [
    'CocoaPods 1.1.1',
    'Version 8.2 (8C38)',
    'iPhone 6 - iOS 10.2 (14C89)'
  ]
}) %>

> This tutorial will enable you to delegate all your authentication needs on Auth0, leaving your API server to handle only your core business processes.
> To achieve this, we will send an Auth0 token to the server through the `Authentication` header of the request. The server in turn will have to check with Auth0 servers to corroborate that this token is valid. We will not go into detail about this latter part, since there are other very good tutorials that cover the server side of this process ([like this one](https://github.com/auth0-samples/auth0-angularjs2-systemjs-sample/tree/master/Server)). You can check out [the official documentation](/api/authentication) for further information on authentication API on the server-side.

> We'll assume you are already familiar with Auth0 and how to Sign up and Login using Lock or Auth0 Toolkit. **If you're not sure, check out the [login tutorial](/quickstart/native/ios-objc/01-login) first.**


### 1. Get the Token

To authenticate with Auth0 we will use Lock. It is no different than any other authentication with Lock:

```objc
A0Lock *lock = [A0Lock sharedLock];
A0LockViewController *controller = [lock newLockViewController];
controller.onAuthenticationBlock = ^(A0UserProfile *profile, A0Token *token) {
    // Save that token for later   
};

[self presentViewController:controller animated:YES completion:nil];
```

### 2. Present It to Your Server

Once you have the user's token, you can use it to authenticate with your own server. Just remember to attach it to your request header like this:

```objc
NSURL* url =  [NSURL URLWithString:@"https://whateversyourAPI.com"];
NSMutableURLRequest* request = [[NSMutableURLRequest alloc] initWithURL:url];

NSString *token = self.token.idToken;
[request addValue:[NSString stringWithFormat:@"Bearer %@",token] forHTTPHeaderField:@"Authorization"];

// Set up your request (method, body, etc)

[[[NSURLSession sharedSession] dataTaskWithRequest: request completionHandler:^(NSData * _Nullable data, NSURLResponse * _Nullable response, NSError * _Nullable error) {
    // Parse the response        
}] resume];
```

Notice that the way you configure your authorization header should match the standards that you're using in your API. This is just an example of what it could look like.

### Done!

That's it.

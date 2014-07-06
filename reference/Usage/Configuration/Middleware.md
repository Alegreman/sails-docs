# Middleware

Sails is fully compatible with Express/ Connect middleware-- in fact, it's all over the place.  Much of the code you'll write in Sails is effectively middleware; most notably [controller actions]() and [policies]().


### Installing Express Middleware In Sails

One of the really nice things about Sails apps is that they can take advantage of the wealth of already-existing Express/Connect middleware out there.  But a common question that arises when people actually try to do this is: _"Where do I `app.use()` it?"_.

In most cases, the answer is to install the Express middleware as a custom HTTP middleware in [`sails.config.http.middleware`]().  This will trigger it for ALL HTTP requests to your Sails app, and allow you to configure the order in which it runs in relation to other HTTP middleware.


### HTTP Middleware

Sails also utilizes an additional [configurable middleware stack]() just for handling HTTP requests.  Each time your app receives an HTTP request, the configured HTTP middleware stack runs in order.

> Note that this HTTP middleware stack is only used for "true" HTTP requests-- it is ignored for **virtual requests** (e.g. requests from a live Socket.io connection.)



#### Conventional Defaults

Sails comes bundled with a suite of conventional HTTP middleware.  You can, of course, disable, override, rearrange, or append to it, but the pre-installed stack is perfectly acceptable for most apps in development or production.  The default HTTP middleware stack is run every time Sails receives an incoming HTTP request; in the order listed below:


 HTTP Middleware Key       | Purpose
 ------------------------- | ------------
 startRequestTimer         | todo
 cookieParser              | todo
 session                   | todo
 bodyParser                | todo
 handleBodyParserError     | todo
 compress                  | todo
 methodOverride            | todo
 poweredBy                 | todo
 $custom                   | todo
 router                    | todo
 www                       | todo
 favicon                   | todo
 404                       | todo
 500                       | Error-handling middleware <!-- todo: expand this -->



### Configuring HTTP Middleware

To configure a custom HTTP middleware function, define a new HTTP key `sails.config.http.middleware.FOO` and set it to the configured middleware function, then add the string name ("FOO") to your `sails.config.http.middleware.order` array wherever you'd like it to run in the middleware chain (a good place to put it might be right before "cookieParser"):

E.g. in `config/http.js`:

```js
  // ...
  middleware: {
    
    // Define a custom HTTP middleware fn with the key `foobar`:
    foobar: function (req,res,next) { /*...*/ next(); },

    // Define another couple of custom HTTP middleware fns with keys `passportInit` and `passportSession`
    // (notice that this time we're using an existing middleware library from npm)
    passportInit    : require('passport').initialize(),
    passportSession : require('passport').session(),

    // Override the conventional cookie parser:
    cookieParser: function (req, res, next) { /*...*/ next(); },


    // Now configure the order/arrangement of our HTTP middleware
    order: [
      'startRequestTimer',
      'cookieParser',
      'session',
      'passportInit',            // <==== passport HTTP middleware should run after "session"
      'passportSession',         // <==== (see https://github.com/jaredhanson/passport#middleware)
      'bodyParser',
      'handleBodyParserError',
      'compress',
      'foobar',                  // <==== we can put this stuff wherever we want
      'methodOverride',
      'poweredBy',
      '$custom',
      'router',
      'www',
      'favicon',
      '404',
      '500'
    ]
  }
  // ...
```


<!--

  TODO:

### Advanced Express Middleware In Sails

You can actually do this in a few different ways, depending on your needs.



Generally, the following best-practices apply:

If you want a middleware function 
 
+ If you want a piece of middleware to run only when your app's explicit or blueprint routes are matched, you should include it as a policy.
+ this will run passport for all incoming http requests, including images, css, etc.

If you want a middleware function to run for all you should include it at the top of your `config/routes.js` as a wildcard route.  for your controller (both HTTP and virtual) requests
-->





<docmeta name="uniqueID" value="middleware198259">
<docmeta name="displayName" value="Middleware">

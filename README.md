nodejs-passport-facebook-example
=======================================

## Install

    $ git clone git@github.com:kpman/nodejs-passport-facebook-example.git
    $ cd nodejs-passport-facebook-example
    $ npm install
    $ npm start


## Usage

#### app.js

    // add passport
    passport = require('passport');
    FacebookStrategy = require('passport-facebook').Strategy;
    // add session
    var session = require('express-session');

    // add session and passport setting
    app.use(session({
        secret: 'nodejs-passport-facebook-example',
        resave: true,
        saveUninitialized: true
    }));
    app.use(passport.initialize());
    app.use(passport.session());

    // passport setting
    passport.serializeUser(function(user, done) {
      done(null, user);
    });

    passport.deserializeUser(function(obj, done) {
      done(null, obj);
    });

    // Use the FacebookStrategy within Passport.
    passport.use(new FacebookStrategy({
        clientID: FACEBOOK_APP_ID,
        clientSecret: FACEBOOK_APP_SECRET,
        callbackURL: "http://localhost:3000/auth/facebook/callback"
      },
      function(accessToken, refreshToken, profile, done) {
        // asynchronous verification, for effect...
        process.nextTick(function () {
          return done(null, profile);
        });
      }
    ));

#### index.js

    router.get('/auth/facebook',
      passport.authenticate('facebook'),
      function(req, res){
        // The request will be redirected to Facebook for authentication, so this
        // function will not be called.
      });

    router.get('/auth/facebook/callback', 
      passport.authenticate('facebook', { failureRedirect: '/' }),
      function(req, res) {
        res.redirect('/account');
      });

    router.get('/account', ensureAuthenticated, function(req, res) {
      console.log(req.user);
      res.render('account', { user: req.user });
    });

    router.get('/logout', function(req, res){
      req.logout();
      res.redirect('/');
    });

    function ensureAuthenticated(req, res, next) {
      if (req.isAuthenticated()) { return next(); }
      res.redirect('/')
    }

## Credits

  - [Jared Hanson](http://github.com/jaredhanson)
  - [Daniel Tseng](http://github.com/kpman)

## License

[The MIT License](http://opensource.org/licenses/MIT)

Copyright (c) 2014 Daniel Tseng <[http://code.kpman.cc/](http://code.kpman.cc/)>

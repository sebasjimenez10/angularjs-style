# Introduction

The goal of this style guide is to present a set of best practices and style guidelines for large AngularJS applications developed between 5 - 9 team members. These best practices are collected from:

0. AngularJS source code.
0. Source code or articles I've read.
0. My own experience working in Talos Digital.

In this style guide you won't find common guidelines for JavaScript development. Such can be found at:

0. [Google's JavaScript style guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
0. [Mozilla's JavaScript style guide](https://developer.mozilla.org/en-US/docs/Developer_Guide/Coding_Style)
0. [GitHub's JavaScript style guide](https://github.com/styleguide/javascript)
0. [Douglas Crockford's JavaScript style guide](http://javascript.crockford.com/code.html)
0. [Airbnb JavaScript style guide](https://github.com/airbnb/javascript)
0. [John Papa](https://github.com/johnpapa/angular-styleguide)
0. [@toddmotto](https://github.com/toddmotto/angularjs-styleguide)

## Directory structure

Since a large AngularJS application has many components it's best to structure it in a directory hierarchy.

I come from the world of Ruby on Rails development workflow where directory hierarchy comes in a MVC manner:
```
.
├── app
│   ├── assets
│   │   ├── images
│   │   └── stylesheets
│   ├── controllers
│   │   └── *
│   ├── helpers
│   │   └── *
│   ├── models
│   │   └── *
│   └── views
│       └── *
└── lib
```

and so many tutorials out there, but in the AngularJS development workflow we found our self with hundreds of files in the same directory without any order nor convention, having to spent a lot of time and effort naming things, searching files, trying to figure out if we need a Service or Factory and where it's going to be placed. So we, Talos Digital, found our own path to win at AngularJS.

0. Split the application in components and sub-components.

  in our example we are going to have
  - users
  - sites
  - authorization

0. Base on components make the top routes
  ```javascript

    '/user'

    '/site'

    '/auth'
  ```

0. Then base on sub-components make the inner routes from top routes
  ```javascript

    // /user
    '/user/billing-history'
    '/user/billing-info'
    '/user/dashboard'
    '/user/invoices'
    '/user/payment'
    '/user/profile'
    '/user/subscription'
    '/user/transactions'

    // /site
    '/site/plan'

    // /auth
    '/auth/forgot-password'
    '/auth/login'
    '/auth/register'
  ```

0. Finally we map the routes to directory hierarchy. having somthing like this.
  ```javascript
  .
  └── app
      └── components
          ├── application
          ├── auth
          │   ├── forgot-password
          │   ├── login
          │   └── register
          ├── site
          │   └── plan
          └── user
              ├── billing-history
              ├── billing-info
              ├── dashboard
              ├── invoices
              ├── payment
              ├── profile
              ├── subscription
              └── transactions
  ```

But wait a moment, in large AngularJS applications we just not have only components, we also have Services, Factories, Models, Directives and More. So we end wrapping our components modules in a `scripts` directory:
  ```javascript
  .
  └── app
      └── scripts
          ├── components
          │   ├── application
          │   │   └── alert
          │   ├── auth
          │   │   ├── forgot-password
          │   │   ├── login
          │   │   └── register
          │   ├── site
          │   │   └── plan
          │   └── user
          │       ├── billing-history
          │       ├── billing-info
          │       ├── dashboard
          │       ├── invoices
          │       ├── payment
          │       ├── profile
          │       ├── subscription
          │       └── transactions
          ├── directives // application common directives
          │   ├── autocomplete-directive
          │   ├── billing-form-directive
          │   ├── loading-directive
          │   ├── mobile-field-header-directive
          │   ├── nav-bar-directive
          │   ├── social-buttons-directive
          │   └── upload-directive
          ├── filters // application common filters
          └── services // application common services
  ```
  So every new intern could find the source `file`/`directory` of a new feature based on given `url`, `component`/`sub-component` (he/she) needs to modify.

  You may question, what kind of files are in each component directory inside directory hierarchy?
  > R/ each component/sub-component logic (controller) and views goes inside.

  ```javascript
  app/scripts/components/
  ├── application
  │   ├── alert
  │   │   ├── alert-controller.js
  │   │   └── alert.html
  │   └── main-controller.js
  ├── auth
  │   ├── forgot-password
  │   │   ├── forgot-password-controller.js
  │   │   └── forgot-password.html
  │   ├── login
  │   │   ├── login-controller.js
  │   │   └── login.html
  │   └── register
  │       ├── register-controller.js
  │       └── register.html
  ├── site
  │   ├── edit-site.html
  │   ├── plan
  │   │   ├── plan-controller.js
  │   │   └── plans.html
  │   ├── site-controller.js
  │   └── site.html
  └── user
      ├── billing-history
      │   ├── billing-history-controller.js
      │   └── billing-history.html
      ├── billing-info
      │   ├── billing-info-controller.js
      │   └── billing-info.html
      ├── dashboard
      │   ├── dashboard-controller.js
      │   └── dashboard.html
      ├── invoices
      │   ├── invoices-controller.js
      │   └── invoices.html
      ├── payment
      │   ├── card-payment-controller.js
      │   ├── card-payment.html
      ├── profile
      │   ├── profile-controller.js
      │   ├── profile.html
      │   ├── profile-edit.html
      │   └── profile-tab.html
      ├── subscription
      │   ├── subscriptions.html
      │   └── subscriptions-controller.js
      └── transactions
          ├── transactions.html
          └── transactions-controller.js
  ```

  And this is our final project directory hierarchy
  ```javascript
  .
  └── app
      ├── images
      ├── scripts
      │   ├── components
      │   │   ├── application
      │   │   │   └── alert
      │   │   ├── auth
      │   │   │   ├── forgot-password
      │   │   │   ├── login
      │   │   │   └── register
      │   │   ├── site
      │   │   │   └── plan
      │   │   └── user
      │   │       ├── billing-history
      │   │       ├── billing-info
      │   │       ├── dashboard
      │   │       ├── invoices
      │   │       ├── payment
      │   │       ├── profile
      │   │       ├── subscription
      │   │       └── transactions
      │   ├── directives // application common directives
      │   │   ├── autocomplete-directive
      │   │   ├── billing-form-directive
      │   │   ├── loading-directive
      │   │   ├── mobile-field-header-directive
      │   │   ├── nav-bar-directive
      │   │   ├── social-buttons-directive
      │   │   └── upload-directive
      │   ├── filters // application common filters
      │   └── services // application common services
      └── styles // application common styles
  ```

## Naming Conventions

Use consistent names for all components following a pattern that describes the component's feature then its type. Our encourage pattern is `feature-type.js`. There are 2 names for most assets:

> the file name (`profile-controller.js`) give us
  ```javascript
  .controller('ProfileCtrl')
  ```
  but if your file is inside a main component, lets say `user` you should name your registered component name with
  ```javascript
  .controller('UserProfileCtrl');
  ```
  notice the CamelCase.

```javascript
// services
auth-service.js

// factories
user-factory.js

// controllers
site-controller.js

// directives
social-buttons-directive.js

// models (optional but recommended)
user-model.js

// views
billing-info.html

// multiple views for same component
profile.html
profile-edit.html
profile-tab.html

// configuration
payment-gateway-config.js

// routes (optional but recommended)
app.routes.js // or just in app.js

/**
 * tests, this is the only one who change, because we use karma.js
 * with jasmine.js, so by default it runs all *.spec.js files
 */
{{SAMENAME}}.spec.js
```

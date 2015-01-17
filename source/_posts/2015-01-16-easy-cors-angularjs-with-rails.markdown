---
layout: post
title: "Easy CORS AngularJS with Rails"
date: 2015-01-06 14:21:41 -0500
comments: true
categories: 
---
Setting Up CORS is quite simple with angularjs and rails.

You are going to have to edit both the frontend and backend. 

#Frontend Angular Portion:
For the frontend portion, all you have to do is inject $httpProvider into your config and add 2 lines of code. See below for an example:

```javascript

angular.module('ultimateJobApplierApp', [
  'ngCookies',
  'ngResource',
  'ngSanitize',
  'ngRoute',
  'ngClipboard',
  'restangular',
])
  .config(function ($routeProvider, $httpProvider) {
     // Enable CORS
    $httpProvider.defaults.useXDomain = true;
    delete $httpProvider.defaults.headers.common['X-Requested-With'];
  })
```

#Rails Backend Portion:
For the rails back-end portion, start by adding the rack-cors gem to your gemfile. Then bundle.

After that, just edit your config/application.rb

```ruby
module JobloggerApi
  class Application < Rails::Application
    # Configure Rack CORS
    config.middleware.use Rack::Cors do
      allow do
        origins '*'
        resource '*', :headers => :any, :methods => [:get, :post, :options]
      end
    end
  end
end
```

Restart your app and CORS should now be enabled!

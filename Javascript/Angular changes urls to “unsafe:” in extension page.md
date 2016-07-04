# Angular changes urls to “unsafe:” in extension page

### Question

I am trying to use angular with a list of apps, and each one is a link to see an app in more detail (apps/app.id):

```html
<a id="{{app.id}}" href="apps/{{app.id}}" >{{app.name}}</a>
```

Every time I click on one of these links, Chrome shows the URL as `unsafe:chrome-extension://kpbipnfncdpgejhmdneaagc...../apps/app.id`

Where does the `unsafe` come from?

### Answer

You need to explicitly add URL protocols to Angular's `whitelist` using a regular expression. Only `http`, `https`, `ftp` and `mailto` are enabled by default. 
Angular will prefix a non-whitelisted URL with `unsafe:` when using a protocol such as chrome-extension:.

A good place to whitelist the chrome-extension: protocol would be in your module's config block:

```javascript
var app = angular.module( 'myApp', [] )
.config( [
    '$compileProvider',
    function( $compileProvider )
    {   
        $compileProvider.aHrefSanitizationWhitelist(/^\s*(https?|ftp|mailto|chrome-extension):/);
        // Angular before v1.2 uses $compileProvider.urlSanitizationWhitelist(...)
    }
]);
```

The same procedure also applies when you need to use protocols such as `file:` and `tel:`.

Please see the AngularJS [$compileProvider API documentation](http://docs.angularjs.org/api/ng.%24compileProvider) for more info.

# 链接

[Angular changes urls to “unsafe:” in extension page](http://stackoverflow.com/questions/15606751/angular-changes-urls-to-unsafe-in-extension-page)

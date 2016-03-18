Angular 1.x Style Guide
=======================

Architecture
------------

  * build system
  * components
  * namespaces

Modules
-------

### one file, one module

Do not spread module definition between several files. It causes that files
must be loaded in a specific order while having the whole module in a single
files angular will figure out the correct time to load each module once its
dependencies are ready

### dependencies

Even if angular module dependency mechanism makes global dependencies overriding
local ones, it is good to specify for each module which are its dependencies.
Doing so we can read a file and figure out what are its dependencies in case we
need to reuse it

Services
--------

  * `factory` recipe
  * `$injector.get` for DI

Directives
----------

```js
angular.module('app.log', [])
.directive('appLog', function ($injector) {
  'use strict';

  var $parse = $injector.get('$parse');

  var controller = function ($scope, $element, $attrs) {
    var appLog = $parse($attrs.appLog)($scope);

    $element.on('click', function () {
      console.log(appLog());
    });
  };

  return {
    scope: true,
    controller: controller
  };
});
```

  * use controller
  * use inherited scope
  * use a 3-letter prefix
  * use DI from `controller` function only for dynamic injection:
    * `$scope`
    * `$element`
    * `$attrs`
  * use DI from `directive` function through `$injector.get` for the rest of services
  * use explicit attribute bindings
  * use `($scope, $element, $attrs)` in that order when using the 3 services

### Config object

Directives with several config options should offer a single attribute to pass a config object

### Custom templates

Directives with associated template should offer the possibility to define custom templates

3rd party components
--------------------

  * namespaces
  * build system (minification, annotation, ...)
  * publish to bower

# strictDI mode

## Overview

We've used the magic of dependency injection a lot, but did you know it actually is a massive performance overhead? Let's use strict DI to reduce that overhead.

## Objectives

- Describe strictDI mode and Angular's dependency injection annotations
- Use strictDI mode

## Dependency Injection

Let's say we've got a controller that injects `$scope`, `$timeout` and `UserService`.

```js
function SomeController($scope, $timeout, UserService) {

}

angular
	.module('app')
	.controller('SomeController', SomeController);
```

When we plug this controller into Angular, Angular checks what dependencies we wants and creates a `$inject` array on our controller:

```js
function SomeController($scope, $timeout, UserService) {

}

SomeController.$inject = ['$scope', '$timeout', 'UserService'];

angular
	.module('app')
	.controller('SomeController', SomeController);
```

This is where the overhead comes in! Angular has to parse the string version of our function, filter out all the unnecessary parts and compile an array of the needed dependencies.

## strictDI

strictDI means that Angular will no longer create that `$inject` property for us - we must specify it ourselves, removing any unnecessary overhead.

We can turn on strictDI by putting the directive `ng-strict-di` on the same element we put `ng-app` on.

```
<div ng-app="app" ng-strict-di>
</div>
```

Now, any custom filter/service/directive/controller/etc that does not have the `$inject` property defined will throw an error. If we've annotated all of our functions correctly (saving Angular a load of time), it'll work exactly like before - just that little bit faster!
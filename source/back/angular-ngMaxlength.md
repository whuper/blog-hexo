####  [ng-maxlength screws up my model](https://stackoverflow.com/questions/17075969/ng-maxlength-screws-up-my-model)

> I'm trying to do a simple textarea with "so many chars remaining" along with validation. when I use ng-maxlength to validate my form, it resets my charcount as soon as the length hits the max length. Here's the [plunkr](http://plnkr.co/edit/aCT8I4sBPOoNABQ8q3nB) Any workarounds?



```html
 <body ng-controller="MainCtrl">
    <div ng-form="noteForm">
      <textarea ng-maxlength="15" ng-model="result"></textarea>
      <p>{{15 - result.length}} chars remaining</p>
      <button ng-disabled="!noteForm.$valid">Submit</button>
    </div>
  </body>
```



> Funny thing, but for me (using Angular 1.4.7 + TypeScript 1.8.7) usage of HTML attribute "maxLength" had same effect as for mentioned here "ng-maxLength": after setting some big value (bigger than defined limit) directly into model, value in model becomes undefined. From my point of view, this is Angular bug (just like [user3232182](http://stackoverflow.com/users/3232182/user3232182) mentioned above). Probably even not linked to ng-maxLength directve, but to the whole binding mechanism. – [bkg](https://stackoverflow.com/users/705427/bkg) [Apr 20 '16 at 7:13](https://stackoverflow.com/questions/17075969/ng-maxlength-screws-up-my-model?answertab=active#comment71220160_17075969) 



---






When your textarea exceeds 15 characters, `result` becomes `undefined` — that's just how the `ng-min/maxlength` directives work. I think you'll have to write your own directive. Here is a directive that will block input after 15 characters:

```html
<textarea my-maxlength="15" ng-model="result"></textarea>
```

```js
app.directive('myMaxlength', function() {
  return {
    require: 'ngModel',
    link: function (scope, element, attrs, ngModelCtrl) {
      var maxlength = Number(attrs.myMaxlength);
      function fromUser(text) {
          if (text.length > maxlength) {
            var transformedInput = text.substring(0, maxlength);
            ngModelCtrl.$setViewValue(transformedInput);
            ngModelCtrl.$render();
            return transformedInput;
          } 
          return text;
      }
      ngModelCtrl.$parsers.push(fromUser);
    }
  }; 
});
```




When your textarea exceeds 15 characters, `result` becomes `undefined` — that's just how the `ng-min/maxlength` directives work. I think you'll have to write your own directive. Here is a directive that will block input after 15 characters:

```js
<textarea my-maxlength="15" ng-model="result"></textarea>
app.directive('myMaxlength', function() {
  return {
    require: 'ngModel',
    link: function (scope, element, attrs, ngModelCtrl) {
      var maxlength = Number(attrs.myMaxlength);
      function fromUser(text) {
          if (text.length > maxlength) {
            var transformedInput = text.substring(0, maxlength);
            ngModelCtrl.$setViewValue(transformedInput);
            ngModelCtrl.$render();
            return transformedInput;
          } 
          return text;
      }
      ngModelCtrl.$parsers.push(fromUser);
    }
  }; 
});
```

------

> **Update:** to allow more than 15 characters, but disable the submit button when the count exceeds 15:

```js
link: function (scope, element, attrs, ngModelCtrl) {
  var maxlength = Number(attrs.myMaxlength);
  function fromUser(text) {
      ngModelCtrl.$setValidity('unique', text.length <= maxlength);
      return text;
  }
  ngModelCtrl.$parsers.push(fromUser);
}
```



---



> As the [doc](https://docs.angularjs.org/api/ng/type/ngModel.NgModelController#$validate) says, the $validate function set model to undefined when validity changes to invalid.
>
> But, we can still prevent this behavior simply by adding `allowInvalid: true` to the ng-model-options.
>
> So, just modify your code like:



```html
<body ng-controller="MainCtrl">
    <div ng-form="noteForm">
        <textarea ng-maxlength="15" ng-model="result" 
            ng-model-options="{ allowInvalid: true }"></textarea>
        <p>{{15 - result.length}} chars remaining</p>
        <button ng-disabled="!noteForm.$valid">Submit</button>
    </div>
</body>
```
#Backbone.Marionette.Traits

Traits is designed to help you more efficiently write modular Marionette views.  By designing simple traits, you will have a much more modular and extensible codebase.

###Start with building a trait builder

Builds a checkbox for a given selector and classname.
May be instantiated multiple times in any given view.

```js
// Factory for producing checkbox traits
var checkbox = function(selector, className, responses){

	var cleanSelector = selector.replace('#', 'id-')
								.replace('.', 'class-')
								.replace('>', 'desc-')
								.replace('<', 'ansc-');
	//var cleanSelector = sanitize(selector);
	var checkboxName = 'checkbox-' + cleanSelector;
	var triggerSelector = 'click ' + selector;
	var triggerEvent = 'click:checkbox:' + selector;
	var eventFn = 'onClickCheckbox' + cleanSelector;

	var trait = {
		ui: {},
		triggers: {},
		initialize: function(){
			this.listenTo(triggerEvent, this[eventFn], this);
		}
	};
	trait.ui[checkboxName] = selector;
	trait.triggers[triggerSelector] = triggerEvent;
	trait[eventFn] = function(){
		trait.ui[checkboxName].toggleClass(className);
		//should call pre and post functions as well as on/off
	}
	return trait;
};
```

###Build the ItemView

This pattern should make code reuse a breeze with marionette.

```js
var checkboxTrait = checkbox('.checkbox', 'active', responses);

// Example view with checkbox trait
var MyView = Backbone.Marionette.ItemView.extend({
	initialize: function(){
		...
	}
}).withTraits(checkboxTrait);
```


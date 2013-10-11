#Backbone.Marionette.Traits

Traits is designed to help you more efficiently write modular Marionette views.  By designing simple traits, you will have a much more modular and extensible codebase.

##Usage:

```js
// Factory for producing checkbox traits
var checkboxTraitFactory = function(selector, className){

	var cleanSelector = selector.replace('#', 'id-')
								.replace('.', 'class-')
								.replace('>', 'desc-')
								.replace('<', 'ansc-');
	
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
	}
	return trait;
};

// Example Trait
var checkboxTrait = checkboxTraitFactory('.checkbox', 'active');

// Marionette View
var MyView = Backbone.Marionette.ItemView.extend({
	initialize: function(){
		...
	}
}).withTraits(checkboxTrait);

```
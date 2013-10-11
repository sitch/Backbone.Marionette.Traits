Backbone.Marionette.Traits

Usage:

```javascript

var details = {
 	ui:{
 		checkbox: '.checkbox' 
 	},
 	triggers: {
 		'click .button': 'click:button'
 	},
	initialize: function(){
		this.listenTo('click:button', this.onClick, this);
	},
	onClick: function(){
		...
	}
}

var MyView = Backbone.Marionette.ItemView.extend({
	initialize: function(){
		...
	}
}).withTraits(details);

```
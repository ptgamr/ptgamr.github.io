---
layout: post
title: My favourite Modules Pattern & JQuery Plugin boilerplates
date: 2014-11-26 20:02:41.000000000 +13:00
---
###The Reavealing Module Pattern
```language-javascript 
    //define your app's namespace
    var app = app || {};
    
    //your module inside 'app' namespace
    var app.myModule = (function ($) {

        // 'this' refers to global window

        //private variable
        var secret = {
        	message: 'I am pretty awesome'
        };
        
        //private function
        var _tellTheSecret = function() {
        	return secret.message;
        }
        
        //public function
        function init() {
        	//init the module base on the present of the element on the page
            
            if ($('#some-element').length) {
            
            	//your initialize code here
                //...
            }
        }
		
        // variables and functions private unless attached to API below
        // define the public API
        var API = {};
        API.init = init;

        return API;
    }(jQuery));
```

###JQuery Plugin Boilerplate
```language-javascript 
    ;(function($) {
        // wrap code within anonymous function: a private namespace

        // replace 'MyPlugin' and 'myplugin' below in your own plugin...

        // constructor function for the logical object bound to a
        // single DOM element
        function MyPlugin(elem, options) {

            // remember this object as self
            var self = this;
            // remember the DOM element that this object is bound to
            self.$elem = $(elem);

            // default options
            var defaults = {
                msg: "You clicked me!"
            };
            // mix in the passed-in options with the default options
            self.options = $.extend({}, defaults, options);

            // just some private data
            self.count = 1;

            init();

            // initialize this plugin
            function init() {

                // set click handler
                self.$elem.click(click_handler);
            }

            // private click handler
            function click_handler(event) {
                alert(self.options.msg + " " + self.count);
                self.count += 1;
            }

            // public method to change msg
            function change_msg(msg) {
                self.options.msg = msg;
            }

            // define the public API
            var API = {};
            API.change_msg = change_msg;
            return API;
        }

        // attach the plugin to jquery namespace
        $.fn.myplugin = function(options) {
            return this.each(function() {
                // prevent multiple instantiation
                if (!$(this).data('myplugin'))
                    $(this).data('myplugin', new MyPlugin(this, options));
            });
        };
    })(jQuery);
```

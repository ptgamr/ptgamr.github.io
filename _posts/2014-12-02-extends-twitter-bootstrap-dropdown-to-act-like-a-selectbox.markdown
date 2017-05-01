---
layout: post
title: Add Select Box feature to Twitter Bootstrap Dropdown
date: 2014-12-02 07:39:21.000000000 +13:00
---
Twitter Bootstrap comes with a very nice Javascript Plugin for Dropdown. What you just need to do is mark up the HTML, include the `bootstrap.js` and you'll have a nicely dropdown working.

I guess a lot of you had ever wanted that Dropdown acts like a selectbox, which means when you click to a menu item, that item will be selected, and the dropdown will be updated with that value.

There is already a great plugin outthere:[bootstrap select](http://silviomoreto.github.io/bootstrap-select/), which has a full featured set of what a Modern Selectbox should do. It also support you to search the options by passing `data-live-search="true"` to the markup.

This plugin convert a selectbox into a dropdown, which leads to these that you might not like:

* In an application where the HTML are rendered before the scripts running, user will first see the default selectbox when openning the page

* It is hard for us to customize the output of a selectbox. What if we want to achive this kind of button?
http://getbootstrap.com/components/#btn-dropdowns-split
It is just a different markdown for bootstrap's dropdown.

I want something work similar, but simpler. So i decided to extend the existing bootstrap dropdown plugins to add the select box behavior:

```language-javascript
(function ($) {

    var Dropdown = $.fn.dropdown.Constructor;
    var toggle   = '[data-toggle="dropdown"]';

    var getParent = function ($this) {
        return $this.parent();
    };

    var select = function(e) {
        if (event.type === 'keydown' && !/13/.test(e.which)) return;

        var $this = $(this);

        e.preventDefault();
        e.stopPropagation();

        if ($this.is('.disabled, :disabled')) return;

        var $parent  = getParent($this);
        var isActive = $parent.hasClass('open');

        var desc = ' li:not(.divider):visible a';
        var $items = $parent.find('[data-select="true"]' + desc);

        if (!$items.length) return;

        var index = $items.index(e.target);

        var $selected = $items.eq(index);
            selectedText = $items.eq(index).text();

        //previous selected
        var $previousSelect = $this.find('.selected');

        if (!$selected.hasClass('selected')) {
            $selected.parent('li').addClass('selected');
            $parent.find('.btn').first().text(selectedText);

            $previousSelect.removeClass('selected');
        }

        //focus to the selectbox after select
        $parent.find(toggle).dropdown('toggle').trigger('focus');

        //trigger the event
        $parent.trigger(e = $.Event('selected.bs.dropdown', {relatedTarget : this}));
    };

    $.extend(true, Dropdown.prototype, {select: select});

    $(document)
        .on('keydown.bs.dropdown.data-api', '[data-select="true"]', Dropdown.prototype.select)
        .on('click.bs.dropdown.data-api', '[data-select="true"]', Dropdown.prototype.select);

})(jQuery);
```

### What does the code do?

* First it introduce a new data attribute: `data-select="true"` on the `.dropdown-menu` list

* It bind a special handler called `select` to the `keydown` & `click` event on the `[data-select="true"]`

* This way, the original plugin will be kept untouched. Also its behavior. You only add the new functionality when you wanted


### Usage & Contribution

I've created a [Github](https://github.com/ptgamr/bootstrap-dropdown-select-extension) repository for it. Please come and have a look. If you have any idea to improve it, don't hesitate to fork it.

Cheers.

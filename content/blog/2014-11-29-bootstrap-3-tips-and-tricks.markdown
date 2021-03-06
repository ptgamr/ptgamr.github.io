---
layout: post
title: Bootstrap 3 Tips and Tricks
date: 2014-11-29 11:00:31.000000000 +13:00
---
##Optionally Remove the Gutter Padding from Columns

Bootstrap lets you customize and compile your own build based on your needs. This is anything from colors, container sizes, and to gutter padding size. However, sometimes you’ll come across an instance where you just want a single row with no padding. Most cases you’ll just individually select the column and kill off the padding with CSS. However, you can build your own utility class helper to do this on the fly. This utility class will cover all column sizes and still supports full responsiveness. Here’s the CSS snippet: 

```language-css
.no-gutter > [class*='col-'] {
    padding-right:0;
    padding-left:0;
}
```

And here’s how you can use it in your HTML:

```language-markup
<div class="row no-gutter">
    <div class="col-md-4">
        ...
    </div>
    <div class="col-md-4">
        ...
    </div>
    <div class="col-md-4">
        ...
    </div>
</div>
```

##Custom Container Sizes

Like it or not, sometimes you’ll be working on a design that doesn’t follow a grid system or a designer who likes to push the boundaries farther and use different grid column sizes on different rows throughout a page and / or site. Bootstrap columns automatically fill to it’s parent container. So if you need a smaller container or a larger container, you can easily extend the Bootstrap core and make your own container classes. Here’s an example:

```language-css

@media (min-width: 768px) {
    .container-small {
        width: 300px;
    }
    .container-large {
        width: 970px;
    }
}
@media (min-width: 992px) {
    .container-small {
        width: 500px;
    }
    .container-large {
        width: 1170px;
    }
}
@media (min-width: 1200px) {
    .container-small {
        width: 700px;
    }
    .container-large {
        width: 1500px;
    }
}
```

Now the only thing you need to worry about is overflow issues if say a container you made is larger then the device window. For example with the .container-large class, if a user has a browser window of 1200px, it will create this nasty overflow issue. Here’s the fix for that.

```language-css
.container-small, .container-large {
    max-width: 100%;
}
```

##Use Heading Classes to Save Time and Stay Semantic

A design sometimes will have elements that don’t always match best practice for having semantic markup. What I mean by this is usually your h1 tag is followed by an h2 and so on. This isn’t necessarily a bad thing when this happens, it’s just something to watch out for because at the end of the day, one of the most important things to make sure of is that your markup is structured in a way that is friendly to read for Google and other Bots.

You can swap out Headings styles with different classes. Take a look at the example code below to see it in action:

```language-markup
<h1 class="h2">Heading 1 that looks like Heading 2</h1>
<h2 class="h4">Heading 2 that looks like Heading 4</h2>
<h3 class="h1">Heading 3 that looks like Heading 1</h3>
```

##Responsive Video Embeds that Maintain Aspect Ratio
When Bootstrap 3.2 came out, it came out with an additional helper class to make it easier to make iframes (like YouTube embeds) responsive while maintaining a certain aspect ratio. It’s extremely easy to use these, just add the following code to your markup:
```language-markup
<!-- 16:9 aspect ratio -->
<div class="embed-responsive embed-responsive-16by9">
    <iframe class="embed-responsive-item" src="//www.youtube.com/embed/ePbKGoIGAXY"></iframe>
</div>

<!-- 4:3 aspect ratio -->
<div class="embed-responsive embed-responsive-4by3">
    <iframe class="embed-responsive-item" src="//www.youtube.com/embed/ePbKGoIGAXY"></iframe>
</div>
```

Notice how the iframe doesn’t include frameborder="0". This is because Bootstrap will actually normalize any ugly outlines for you with this helper class.

##Responsive Utility Classes Need a Display Property

With Bootstrap 3.2 being released, they updated the way responsive utility classes work. Normally you would just assign `.visible-xs`, `.visible-sm`, `.visible-md`, and `.visible-lg` to hide or show certain elements at different device widths. Now, you actually have to specify a “state” as well of either block, inline, or inline-block. This maximizes your control, but if you’re confused on which on to use, just use block. Here’s some examples:

```language-css
/* With * being xs, sm, md, or lg */
.visible-*-block {}
.visible-*-inline {}
.visible-*-inline-block {}
```

##Extend the Class, Don’t Override It
I often see front-end code where developers completely override Bootstrap styles to customize buttons and other elements rather than extending the existing classes. This gets messy real fast. It’s much easier to maintain if you just extend the existing classes. For example, let’s make a completely flat and yellow button called .btn-yellow to demo the principle.

```language-css
.btn-yellow {
  background: rgb(250, 255, 140);
  color: #574500;
  border: none;
  -moz-box-shadow: none !important;
  -webkit-box-shadow: none !important;
  box-shadow: none !important; /* !important tags aren't necessarily always bad */
}
.btn-yellow:hover, .btn-yellow:focus {
  background: rgb(252, 255, 179);
}
.btn-yellow:active {
  background: rgb(247, 255, 71);
}
```

This way you can still use things like .btn-lg or .btn-block to customize your buttons. Now, I’ll also make a class called .btn-xxl:

```language-css
.btn-xxl {
  padding: 20px 26px;
  font-size: 35px;
  border-radius: 8px;
}
```

Building a base CSS by extending the classes can prove to be extremely helpful in the long run and can really speed up your front-end development.

##* { box-sizing: border-box; }
My favorite thing about Bootstrap 3 versus 2 is that it adopted the new box-sizing: border-box property for all elements and pseudo-elements (:before and :after). This might be more obvious to some, but what this does is changes the way the CSS box-model works. When this is enabled the width and height properties include the padding and border, but not the margin. In laymen terms, if you add padding, your element isn’t going to grow in size. Instead, the element’s contents gets pushed closer and closer towards the center of the box.

Bootstrap 2 didn’t use this method, and I imagine people people who recently switch will be thrown off by this since it goes against how most people are taught CSS in the first-place. After you get used to it though, it starts to make responsive web development a lot easier and will probably be your new default even without Bootstrap. I think the idea to do it this way was first introduced by Paul Irish from this post. 

##Your Respond.js include might be all wrong

Respond.js is is an excellent polyfill for bringing media queries to legacy browsers (for this case, getting Bootstrap 3 to work with IE8). Making sure that respond.js is even actually working will make your cross-browser tests much easier. I learned that Respond.js can fail based on two things:

1. Where it is located in the document
2. If you’re loading your styles from a CDN

###Load respond.js after your CSS
If for whatever reason you are loading respond.js before your styles, this will cause the polyfill to fail. You need to load respond.js after your styles. If you have inline styles, respond.js should technically be loaded after that as well.

###A CDN can create cross-domain errors
Respond.js works by requesting a copy of your CSS via AJAX. If you have your CSS on a different domain, such as a CDN, per security settings of browsers, it won’t be able to load the CSS assets. It’s recommended that you create a proxy-page to enable cross-domain communication. You can read about doing that at their official docs. The quickest and easiest solution though is to just host the Bootstrap 3 assets and the Respond.js assets yourself (don’t use a CDN). This is a quick an easy fix that would work for most people. You should evaluate the pros and cons of ditching the CDN for your specific project since using the CDN has obvious performance benefits.

Source: [scotch.io](http://scotch.io/bar-talk/bootstrap-3-tips-and-tricks-you-still-might-not-know)

---
layout: post
title: How Pinterest construct their layout?
date: 2014-08-07 22:02:08.000000000 +12:00
---
I love Pinterest because of the Pins layout. And recently, i developed a small Mobile App for choosing Hairstyles, i want to mimic Pinterest layout for my image gallery.

And here is how Pinterest do it (the algorithm):

    #Beforehand:

    - Absolutely position the pin containers
    - Determine column width
    - Determine margin between columns (the gutter)
    
    #Setup an array:

    - Get the width of the parent container; calculate the # of columns that will fit
    - Create an empty array, with a length equaling the # of columns. Use this array to store the height of each column as you build the layout, e.g. the height of column 1 is stored as array[0]
    
    #Loop through each pin:

    - Put each pin in the shortest column at the moment it is added
    - "left:" === the column # (index array) times the column width + margin
    - "top:" === The value in the array (height) for the shortest column at that time
    
    Finally, add the height of the pin to the column height (array value)
    The result is lightweight. In Chrome, laying out a full page of 50+ pins takes <10ms
   
This is enough for for a fork on Github, but i found [Masonry](http://masonry.desandro.com), just serve my purpose well. So why we have to re-invente the wheel? :-)

Next comming post, i will tell you how i set up Masonry in my AngularJS application, together with lazy loading ([Echo.js](https://github.com/toddmotto/echo) from TodMotto).

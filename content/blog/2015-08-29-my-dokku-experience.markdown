---
layout: post
title: My dokku experience
date: 2015-08-29 23:18:54.000000000 +12:00
---
I have to say it is pretty cool. It helps to build own Heroku & mae the deployment while developing faser.

Something that sucks at the beginning:

##### Make nginx working
VHOST file must in /home/dokku to enable NGINX virtual hosts configuration

##### Deploy to root domain

Simple, just named your app <your.domain.com> then push

Eg. `git remote add dokku dokku@<ip.address>:<domain.example.com>`


##### MongoDb linking
`dokku mongodb:create <app> <db>` is not only create the database, but also create a user for app to access the database.

Therefore, if you have another app want to access the same database, `dokku mongodb:link <new_app> <db>` just not work. You'll get `auth failed` error.

Solution: run `mongodb:create <new_app> <db>` first, then `mongodb:link`




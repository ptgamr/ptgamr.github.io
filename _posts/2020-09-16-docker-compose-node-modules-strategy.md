---
layout: post
title: 'docker-compose: replace or retain node_modules fodler?'
date: 2020-09-16
---

You already Dockerize your frontend setup? Great! Now come to the point that you might want to use `docker-compose` to run your web app in the local development environment.
I am quite sure at some point you will scratch your head with `node_modules` folder: **how it end up to be where it is?**, **where should I do the `npm install`?** and all that... 

I hope this post will make your life a little bit easier.

Let say, you have this `Dockerfile` setup like bellow:

```
FROM node:10-alpine

WORKDIR /code

COPY package.json .
COPY package-lock.json .

ENV CI=1
RUN npm ci

```

That is, if you do `$ docker build mywebsite .`, it will actually go ahead and install the `node_modules` inside the image. You can check it for your self:

```
$ docker run mywebsite ls -la /code/node_modules

// your node_modules content will be listed here.
```

Now, to run it with `docker-compose`, you will need something like this:

```
version: '3'

services:
  frontend:
    build: .
    command: npm start
    volumes:
      - .:/code
    ports:
      - "4200:4200"
```

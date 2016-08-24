# Trying out Meteor

The JS frameworks I've looked at previously all suffered from overconfiguration and exposing too much details. Error reporting was usually obscure, and there were too many ways to do the same thing and no clearly defined conventions. That made me feel that they are taking it all too serious and that there is not much point in using node.js unless you cannot learn any other language but Javascript. You need realtime stuff and performance? Pick Go or Elixir. Have a general-purpose website to code - look at Ruby on Rails. A CMS - go Wordpress. Where is the actual sweet spot for node frameworks?

And then [Meteor][3] got in my way. It just works out of the box, provides nice command line tool with readable output and definetly establishes a "meteor way" of doing things.


## Installation

On Arch linux Meteor is available with an AUR package:

```shell
$ yaourt -S meteor-js
```

Here you get the first surprize: 150Mb to download a JS framework?! WAT? Looks like it has a lot of stuff bundled in, including large number of npm modules. Interesting approach, does that mean it has everything needed for development?


## Tutorial

OK, let's check out the [official tutorial][1].

```shell
meteor create simple-todos
cd simple-todos
meteor npm install
meteor
```

Heh, it's started Mongo for me. Looks like developer guys are getting increasingly lazy. I bet the next step is to bundle Docker directly into the framework.

So the app is running at http://localhost:3000/ and when I change source files the page is reloaded automagically. Cool.

Then I just continued with the tutorial, poked it here and there and added support for todo items images. Most of the pros and cons of this framework come from it's architecture. Here are some of my impressions.

## Use of MongoDB

It infuences Meteor's design a lot. Generally, all the advantages and disadvantages of the said database apply here.

Mongo is schemaless, which is both good and bad. You don't need migrations, which eliminates some hassle, but makes you either tolerate inconsistent data or take care of updating it somehow.

Another key point is lack of transactions. Yes it supports atomicity, but the sad truth is: once you have complex business logic you'll suffer from not having real transactions. It's possible to go without them, but at additional development cost.

Having to use mongo query language takes time to adapt, but is very powerful.

## Websockets

By default Meteor app does not expose any API, instead it uses websockets connection to monitor data changes. Due to this user facing content updates as soon as it changed in the database.

Sounds cool, but you'll inevitably run into all the websockets joy: network topology problems, firewalls, performance issues.

## Error reporting

Though Meteor has gone a long way from Express' way of articulating problems, you still can get a giant, absolutely misleading trace in your console, even if your only fault is misspelling a template name.

## Native app

Didn't dig deep into that, but Meteor advertises native iOS and Android support and the app you build in the tutorial does work in Android simulator. But I haven't noticed any significant benefit compared to running website in mobile browser.

Anyway the ease of creating native apps is impressive.

## Opiniated framework

Meteor is very opiniated. It provides you with a "right way" to do things, and if you follow it, everything goes smoothly. There is a good design principle that it simple stuff should be done easily and complex stuff should be possible to implement with some thought. Meteor definitely sticks to that principal, which is a sign of a good framework.

Good example is account management. Need users to be able to sign in? Just drop in a package. Facebook authentication? Another package. And since you can customize it, it's hard to find situation when you have to implement sign up/sign in functionality yourself.

## Testing

Tests are essential, and in Meteor are dead simple to write and run.


## Event handlers work for dynamically created elements

There is a callback handling new task form submit:

```javascript
Template.body.events({
  'submit .new-task'(event) {
    // ...
  },
});
```

It is attached to the form element we have in our `body.html` template:

```html
<form class="new-task">
  <input type="text" name="text" placeholder="Type to add new tasks" />
</form>
```

But what happens if that element was not present at application load? Let's load the app and create another new task form using developer toolbar.

And yes, form submit still works and creates a new task. Awesome!

## File uploads

Due to presence of Mongo the recommended way of handling files in Meteor is to use GridFS. I used [meteor-file-collection][4] adapter which allows to manage files almost as an ordinary collection. It's pretty straightforward, but for a 'real world' application you'll probably want to setup a separate upload endpoint.

# Conclusion

What I can say about Meteor: it's astounding! This approach to web application building IMHO has some brilliant future ahead. Of course this comes at a cost: the resulting application is heavy and when things go wrong, you'll have hard time exploring the framework code. But those are just the other side of the coin.

Meteor can be a good choice both for prototyping and building small to medium general purpose websites. I'm in.


[1]: https://www.meteor.com/tutorials/blaze/creating-an-app "Official tutorial"
[2]: https://www.meteor.com/tutorials/blaze/templates
[3]: https://www.meteor.com/
[4]: https://github.com/vsivsi/meteor-file-collection

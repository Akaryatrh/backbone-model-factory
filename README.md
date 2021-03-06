# Backbone.ModelFactory

Provides a factory for generating model constructors which will never produce multiple instances of a model with the same unique identifier. It allows a developer to manage model sharing between views more easily by maintaining an internal cache of model instances based on the value of their `idAttribute` property.

In short, when you create a new instance of a model created with `Backbone.ModelFactory` and give it an `id` it will never create duplicate instances of the same model with a given `id`.

    var user1 = new User({id: 1});
    var user2 = new User({id: 1});
    var user3 = new User({id: 2});

    console.log(user1 === user2); // true
    console.log(user3 === user1); // false

## Inclusion

`Backbone.ModelFactory` supports three methods of inclusion.

1. Node:

        var Backbone = require('backbone-model-factory');

2. AMD/[RequireJS](http://requirejs.org):

        require(['backbone-model-factory'], function (Backbone) {
            // Do stuff...
        });

3. Browser Globals:

        <script src="path/to/backbone.js"></script>
        <script src="path/to/backbone-model-factory.js"></script>

## Usage

Rather than extending `Backbone.Model`, constructors are created by a call to `Backbone.ModelFactory`. Instead of this:

    var User = Backbone.Model.extend({
      defaults: {
        firstName: 'John',
        lastName: 'Doe'
      }
    });

...do this:

    var User = Backbone.ModelFactory({
        defaults: {
            firstName: 'John',
            lastName: 'Doe'
        }
    });

`Backbone.ModelFactory` also supports inheritance, so model constructors can extend each other by providing a model constructor as the first argument:

    var Admin = Backbone.ModelFactory(User, {
        defaults: {
            isAdmin: true
        }
    });

### Consequences of Extending Models

Models created with `Backbone.ModelFactory` will *not* share their unique-enforcement capabilities with models which they extend or which extend them. For example, using the `User` and `Admin` models above giving each the same ID would *not* result in the same object:

    var user = new User({id: 1});
    var admin = new Admin({id: 1});

    console.log(user === admin); // false

The ability to share instances between models in an inheritance chain is a feature which is actively being considered, but it leaves a lot of open questions.

## Tests

### Inclusion

There are 3 files in `/test/inclusion` and they account for the 3 supported methods of including this module. To execute these tests, simply open the HTML files in a browser or install the npm module `backbone` and run `node node-module.js`.

### Unit

[Mocha](http://visionmedia.github.com/mocha/) unit tests exist in `test/test.js`. To run, simply do `mocha` from the project root.

## Inspriation

This is inspired by SoundCloud's approach detailed in [Building the Next SoundCloud](http://backstage.soundcloud.com/2012/06/building-the-next-soundcloud/) under "Sharing Models between Views."

# Rethinking AngularJS App Structure

AngularJS is a fantastic framework for rapidly developing web applications. After using Angular for my last 3 projects, I noticed a drastic improvement in my productivity.

However, I have noticed a lot of disagreement in the developer community about how to structure Angular applications. If you compare several of the largest Angular boilerplates, you'll see that each of them has a significantly different app structure:

- [Angular Seed](https://github.com/angular/angular-seed)
- [Mean.io](http://mean.io/#!/)
- [Angular App](https://github.com/angular-app/angular-app)
- [ngbp](https://github.com/ngbp)
- [generator-angular-fullstack](https://github.com/DaftMonk/generator-angular-fullstack)
- [generator-cg-angular](https://github.com/cgross/generator-cg-angular)
- [angular-sprout](https://github.com/thedigitalself/angular-sprout)
- [kickstrap](http://getkickstrap.com/)
- [Angular Ultimate Seed](https://github.com/pilwon/ultimate-seed)

Although the Angular Seed is the officially recommended starter for AngularJS projects, developers quickly realized that this structure does not scale to larger sized projects. As a result, there has been an explosion of boilerplates like the ones above, with almost every project and company using a different implementation.

As a software engineer, I am a strong believer in [convention over configuration](http://en.wikipedia.org/wiki/Convention_over_configuration). Well established conventions allow developers to focus on developing application logic rather than reinventing the wheel. If a developer has to spend large amounts of time deciphering a new application architecture, that is incredibly inefficient.

Luckily, the Angular community has begun to address these concerns. The official AngularJS blog recently came out with a post about [AngularJS best practice app structure](https://docs.google.com/document/d/1XXMvReO8-Awi1EZXAXS4PzDzdNvV6pGcuaF4Q9821Es/pub), and several other bloggers have made similar recommendations:

- [Cliff Meyers](http://cliffmeyers.com/blog/2013/4/21/code-organization-angularjs-javascript)
- [John Papa](http://www.johnpapa.net/angular-app-structuring-guidelines/).
- [This incredible Github thread](https://github.com/yeoman/generator-angular/issues/109)
- [Briant Ford](http://briantford.com/blog/huuuuuge-angular-apps)
- [Gert Hengeveld](https://medium.com/opinionated-angularjs/9f01b594bf06)

However, these posts still left me with a lot of questions about how to actually _implement_ a structure like this.

So with this blog post, I hope to continue the conversation, compare some of the different approaches, and make some of my own recommendations based on what I have experienced in the battlefield.

## Organize unique features by state/route

One of the common trends you see in a lot of these applications architectures (including the AngularJS blog recommendations) is to organize files by feature, not by type. This provides several advantages:

- **Files easy to find** - Easy for developers to learn and find everything they need to get started. It matches the navigation hierarchy of the app.
- **Easy to move features** - What if I want to move a feature to a different part of the application? With this structure, I can easily copy and past the whole directory with just a few configuration changes.
- **Easy to add new files** - I am often hesitant to add new modules/features when there is a lot of configuration involved. Having all your files in one place make configuration significantly easier.
- **Edit in confidence** - When you make a change, you can easily see all the other files that will be affected. From my experience, this makes the development workflow more productive.

But what exactly constitutes a "feature"? Is a feature an entire page? Part of a page? A directive? A service?

In my projects, I find it intuitive to organize my features into two categories: state-specific features (only used once) and common features (reusable many times). All my state features naturally coincide with the "states" used in the [Angular UI Router](https://github.com/angular-ui/ui-router), or by routes if using ngRoute...

```
app
|-- states/
|   |-- state1/
|   |   |-- state1.js <= module, state config, controller
|   |   |-- state1.unit.js
|   |   |-- state1.html
|   |   |-- state1.css
|   |   |-- state1.e2e.js <= integration test
|   |-- state2/
|   |   |-- state2.js
|   |   |-- state2.unit.js
|   |   |-- state2.html
|   |   |-- state2.css
|   |   |-- state2.e2e.js
```

Each state directory a contains *only* the code/features that are specific to that state. This includes

- The module definition
- The state/route configuration
- The state controller
- The state template
- The state styles
- State-specific directives, filters, services (not reused anywhere)
- The state unit tests
- The state integration (e2e) test

Within each state, you can also add directives, filters, and anything else that is unique to that state:

```
app
|-- states/
|   |-- state1/
|   |   |-- state1.js
|   |   |-- state1.unit.js
|   |   |-- state1.html
|   |   |-- state1.css
|   |   |-- state1-edit-product-modal.js
|   |   |-- state1-edit-product-modal.unit.js
|   |   |-- state1-edit-product-modal.html
|   |   |-- state1-graph-directive.js
|   |   |-- state1-graph-directive.css
|   |   |-- state1-graph-directive.html
|   |   |-- state1-graph-directive.unit.js
|   |   |-- state1-product-filter.js
|   |   |-- state1-product-filter.unit.js
|   |   |-- state1.e2e.js
```
Notice how these elements are namespaced with the state name in front of them. This practice is incredibly helpful once the project expands.

If we want to add any child states, we can easily nest it within the parent state like this:

```
app
|-- states/
|   |-- state1/
|   |   |-- state1.js
|   |   |-- state1.unit.js
|   |   |-- state1.html
|   |   |-- state1.css
|   |   |-- state1.e2e.js
|   |   |-- child-state/
|   |   |   |-- child-state.js
|   |   |   |-- child-state.unit.js
|   |   |   |-- child-state.html
|   |   |   |-- child-state.css
|   |   |   |-- child-state.e2e.js
```

Voila! Now you have a scalable structure where you can easily expand and nest as many states as you need, without confusion or namespacing conflicts.

## Common Features

In addition to reusable vendor components, developers often create their own reusable components for use throughout the project. This includes services, directives, LESS elements, filters, or whatever else you might need.

This is where the common directory comes into play. Whether you call them "components" (AngularJS recommendation), "core", "shared", or "common" (my preference), this directory is the place to look for any custom-built, reusable components.

Here's an example...

```
app
|-- vendor/
|-- common/
|   |-- directives/
|   |   |-- map-directive/
|   |   |   |-- map-directive.js
|   |   |   |-- map-directive.unit.js
|   |   |   |-- map-directive.html
|   |   |   |-- map-directive.css
|   |-- filters/
|   |   |-- capitalize-filter/
|   |   |   |-- capitalize-filter.js
|   |   |   |-- capitalize-filter.unit.js
|   |   |   |-- capitalize-filter.html
|   |   |   |-- capitalize-filter.css
|   |-- less/
|   |   |-- buttons.less
|-- states/

```
Some projects (like ngbp) move the styles into an entirely separate directory. To me, this doesn't make sense since the CSS files are mixed alongside the JS and HTML files throughout every other part of the project.

Note, if this section starts to overflow with too many components, it can be subdivided into even more subdirectories...

```
app
|-- common/
|   |-- directives/
|   |   |-- graphs/
|   |   |   | -- product-graph/
|   |   |   | -- customer-graph/
|   |   |   | -- pie-chart/
```

## Managing all the modules

With everything organized into features, we can hook it all together with modules.

The [angular documentation for modules](https://docs.angularjs.org/guide/module) says

> We recommend that you break your application into multiple modules like this:

> - A module for each feature
- A module for each reusable component (especially directives and filters)
- And an application level module which depends on the above modules and contains any initialization code.

I couldn't have said it better myself. Using one module per feature is a fantastic idea because it

- **simplifies testing** - you test each module in isolation, so you have fewer dependencies to mock out.
- **simplifies dependency management** - just by looking at the module definition, you can see all other modules that it depends on.
- **simplifies debugging** - AngularJS injection and module loading bugs can be notoriously difficult to fix. This structure makes it easy to "comment out" modules until you isolate the bug.

#### Modules for State or Common Features

Here is an example of how I usually set up a module in my projects for a state or common feature:

```language-javascript
angular.module('stateName', [
    // vendor dependencies
    'ui.router',
    'ui.bootstrap',

    // common dependencies
    'filters.capitalize',
    'services.lodash',

    // state dependencies
    'state.substate1',
    'state.substate2'
])
```

Notice, the dependencies are organized logically from more general vendor dependencies at the top, to specific state/feature dependencies at the bottom. This allows developers to quickly identify where each module originated.

#### Application-level module

Angular recommends an application-level module to kick everything off. Often, this module just loads the top-level states, and sets some app-wide configuration.

```language-javascript
angular.module('app', [
    // state dependencies
    'state1',
    'state2'
])
```

## Making templates modular

Many Angular projects use the [html2js grunt task](https://github.com/karlgoldstein/grunt-html2js) to precompile templates and load them from the root path. The problem is, in order to load a template in this new modular structure, it looks something like this...

```language-javascript
$stateProvider.state('home.productList', {
  url: '/product-list',
  views: {
    "product-list": {
      controller: 'HomeCtrl',
      templateUrl: 'states/home/product-list/product-list.tpl.html'
    }
  }
});
```
Wow! `states/home/product-list/product-list.tpl.html`, is quite a mouthful! Not to mention, this template depends on other parts of the file structure. What if I changed `home/` to `dashboard/`? Suddenly all of my templates within `home/` will be broken. Or what if I want to move the `product-list` module to another location? Believe me, this can create a massive headaches.

Some people add [Browserify](http://browserify.org/) or [RequireJS](http://requirejs.org/) to deal with this issue. But in my opinion, adding another module system on top of Angular's built-in module system is an unecessary layer of complexity (although theoretically, they could be compatible).

Angular 2.0 should address some of these issues. But in the meantime, I found another solution.

With AngularJS, we can manually name templates by wrapping it in a `text/ng-template` script tag. Therefore, we can simplify this problem significantly if we name the templates the same as the state/feature that it is associated with (`home.productList`)? For example:

```
<script type="text/ng-template" id="home.productList.html">
  <h1>This is a template</h1>
</script>
```

Then, I use the [grunt htmlbuild task](https://github.com/spatools/grunt-html-build) to inject all of my custom-named templates into my index.html.

```language-javascript
htmlbuild: {
  templates: {
    src: 'app/index.html',
    dest: 'public/',
    options: {
      sections: {
        templates: 'app/{common,states}/*.html',
      },
    }
  }
}
```

The template can then be loaded like this:


```language-javascript
$stateProvider.state('home.productList', {
  url: '/product-list',
  views: {
    "product-list": {
      controller: 'HomeCtrl',
      templateUrl: 'home.productList.html'
    }
  }
});
```

Much better!

## Making LESS, SASS, and CSS modular

Another frequent problem in modular AngularJS applications is managing your feature-specific styles. Since Angular modules have no way of including css files, we need to find some other method.

Some projects group the CSS files in a separate directory to completely bypass this problem (like Angular App). However, this solution goes against the modular structure used by everything else in the application.

And in other projects, people use RequireJS or Browserify to load the assets, but again, I would rather avoid adding an extra module system.

I've even seem so projects (like ngbp) manually import each LESS file manually into their `main.less`. But from my experience, this is a big pain for many of the same reasons that loading absolute template paths was a pain, and this workflow is not intuitive for new developers.

However, with just a few simple tweaks to our build process, Grunt will gracefully manage all of these assets for us.

In this example, I tell Grunt to go collect all feature-specific CSS assets, and concatenate them to the end of my `index.css` file.

```language-javascript
concat: {
  css: {
    src: ['app/index.css', 'app/{common,states}/*.css'],
    dest: 'public/bundle.less'
  }
}
```

And just like that, we have a `bundle.css` file which includes all of our CSS assets across our whole project.

Now, if you add a CSS file within any of your feature directories, grunt will load it automatically... no configuration required.

#### With LESS and SASS

Using LESS or SASS? No problem. Just concatenate all your module LESS files to the end of your `main.less` file before you compile them:

```language-javascript
concat: {
  less: {
    src: ['app/index.less', 'app/{common,states}/*.less'],
    dest: 'public/bundle.less'
  }
},
less: {
  dev: {
    files: {
      "public/bundle.css": "<%= concat.less.dest %>"
    }
  }
}
```

The trick is to generate the bundle.less in the *same* location as your index.less, so that any relative `@import`s work correctly when you compile the bundle.less file.

Then, you can do another trick to make it even more modular. Say for instance we want to add some styles specifically to our 'dashboard.productList' state...

```language-css
#dashboard-product-list {
  .class1 {
    border: 1px solid #BADA55;
  }
  .class2 {
    height: 150px;
    width: 150px;
  }
}
```

```
<script type="text/ng-template" id="dashboard.productList.html">
  <div id="dashboard-product-list">
      <div class="class1"></div>
      <div class="class2"></div>
    </div>
</script>
```

We create an id matching the state name to create a CSS "module", then add all the module specific classes within that ID. This ensures that you will avoid CSS naming collisions no matter how large your project gets.

## Putting it all together

That about sums it up. Let's put everything together.

```
app
|-- index.html <= HTML entry point
|-- index.js <= JS entry point
|-- index.less <= LESS entry point
|-- vendor/ <= vendor common files
|   |-- angular/
|   |-- angular-ui/
|   |-- bootstrap/
|-- common/ <= user common files
|   |-- less/
|   |   |-- buttons.less
|   |   |-- buttons.less
|   |-- directives/
|   |   |-- map-directive/
|   |   |   |-- map-directive.js
|   |   |   |-- map-directive.unit.js
|   |   |   |-- map-directive.html
|   |   |   |-- map-directive.less
|   |-- filters/
|   |   |-- capitalize-filter/
|   |   |   |-- capitalize-filter.js
|   |   |   |-- capitalize-filter.unit.js
|   |   |   |-- capitalize-filter.html
|   |   |   |-- capitalize-filter.less
|-- states/
|   |-- state1/
|   |   |-- state1.js
|   |   |-- state1.unit.js
|   |   |-- state1.html
|   |   |-- state1.less
|   |   |-- state1.e2e.js
|   |   |-- state1-edit-modal.js
|   |   |-- state1-edit-modal.less
|   |   |-- state1-edit-modal.html
|   |   |-- state1-edit-modal.unit.js
|   |   |-- child-state/
|   |   |   |-- child-state.js
|   |   |   |-- child-state.unit.js
|   |   |   |-- child-state.html
|   |   |   |-- child-state.less
|   |   |   |-- child-state.e2e.js
|   |-- state2/
|   |   |-- state2.js
|   |   |-- state2.unit.js
|   |   |-- state2.html
|   |   |-- state2.less
|   |   |-- state2-graph-directive.js
|   |   |-- state2-graph-directive.less
|   |   |-- state2-graph-directive.html
|   |   |-- state2-graph-directive.unit.js
|   |   |-- state2.e2e.js
|-- package.json
|-- bower.json
|-- Gruntfile.js
```

This makes an intuitive application structure that is easy to learn, easy to work with, and scales to applications of any size. It combines many of the best parts of other Angular boilerplates, while following most of the recommended practices proposed by the AngularJS team.

I am close to finishing a seed project based on this layout for anybody to use. In the meantime, if you have any suggestions or feedback, please share in the comments below!







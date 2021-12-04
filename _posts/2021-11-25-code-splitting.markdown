---
layout: post
title: "What's this code splitting nonsense?"
date: 2021-12-03 22:40:00 -0500
---

Good morning! You just woke up and you're heading to the kitchen to make yourself a coffee &mdash; 1 cream & 1 sugar, the perfect cup. As you open the carton of cream, you notice a stench... It's expired! Why did this have to happen to me!? You realize it's because you left it out of the refridgerator for far too long when you bought groceries the other day.

# The other day...

You put all your groceries &mdash; including the cream into 1 big bag and drive back home. As you're struggling to take out the bag you notice your neighbour walking back and forth from their car to the door with much smaller bags. By the time you get to your door, they finished bringing in their stuff already. Because you took so long to bring in your groceries, the cream expired... a truely pitiful experience. In real life cream doesn't expire that fast, but on the internet, seconds can make a huge difference between a good experience vs a bad one &mdash; so use small bags.

## Using small bags

The JavaScript applications i'm used to, generally have some sort of bundler installed to package modules into bundles. In order to achieve code splitting, you want your bundler to package your modules into **multiple bundles** instead of one big one to send to the browser. I'm used to **webpack**, so I'm going to use that.

Imagine you have a module that relies on lodash like so:

**src/index.js**

    import _ from 'lodash';

    function component() {
      const element = document.createElement('div');

      element.innerHTML = _.join(['Hello', 'webpack'], ' ');
      return element;
    }

    document.body.appendChild(component());

When you run webpack to bundle your application, you'll get a single output `main.js` file that is around 69.3KiB like so:

    $ npx webpack
    [webpack-cli] Compilation finished
    asset main.js 69.3 KiB [emitted] [minimized] (name: main) 1 related asset
    runtime modules 1000 bytes 5 modules
    cacheable modules 530 KiB
      ./src/index.js 257 bytes [built] [code generated]
      ./node_modules/lodash/lodash.js 530 KiB [built] [code generated]
    webpack 5.4.0 compiled successfully in 1851 ms

You can largely reduce the `main.js` file by incorporating code splitting into your project by utilizing dynamic imports: `import()`

**src/index.js**

    async function getComponent() {
      const element = document.createElement('div');
      const { default: _ } = await import('lodash');

      element.innerHTML = _.join(['Hello', 'webpack'], ' ');
      return element;
    }

    getComponent().then((component) => {
      document.body.appendChild(component());
    });

When you run webpack to bundle your application now, you'll get two outputs: `index.bundle.js` and `vendors-node_modules_lodash_lodash_js.bundle.js`. Notice now that your main entry point has a much smaller size of 13.5KiB.

    [webpack-cli] Compilation finished
    asset vendors-node_modules_lodash_lodash_js.bundle.js 549 KiB [compared for emit] (id hint: vendors)
    asset index.bundle.js 13.5 KiB [compared for emit] (name: index)
    runtime modules 7.37 KiB 11 modules
    cacheable modules 530 KiB
      ./src/index.js 434 bytes [built] [code generated]
      ./node_modules/lodash/lodash.js 530 KiB [built] [code generated]
    webpack 5.4.0 compiled successfully in 268 ms

## Conclusion

People love to bring in their groceries with 1 bag, but sometimes its faster to do with many small bags.

[<img src="https://rokusek.com/wp-content/uploads/2019/12/Man-Carrying-Grocery-Bags.png">](https://rokusek.com)

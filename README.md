[![Dependency Status](https://david-dm.org/iam4x/isomorphic-flux-boilerplate.svg)](https://david-dm.org/iam4x/isomorphic-flux-boilerplate)

# ES6 Isomorphic Flux/ReactJS Boilerplate

> A wonderfull boilerplate for **Flux/ReactJS** applications, running on **Koa**.

## Libraries Included

* [react](https://facebook.github.io/react/)
* [react-router](https://github.com/rackt/react-router)
* [react-hot-loader](https://github.com/gaearon/react-hot-loader)
* [alt](https://github.com/goatslacker/alt)
* [iso](https://github.com/goatslacker/iso)
* [koa](http://koajs.com/)
* [webpack](http://webpack.github.io/)
* [gulpjs](http://gulpjs.com/)
* [babeljs](https://babeljs.io/)

## Why use this boilerplate?

I wanted to create a simple isomorphic reactjs application, but I found [ambidex](https://github.com/appsforartists/ambidex) or [fluxible](https://github.com/yahoo/fluxible) to hard to dive in, too many concepts and it's kinda killing the way of React thinking. Thinking as components, little bricks to assemble.

I needed also an complete stack ready for full web-app development, with nice tools for the front-end. (Gulp and his magic tasks).

With this boilerplate, you can choose easily what components you need and you won't be lost.

## Concepts

**Koa** will be our server for the server side rendering, we use **alt** for our Flux architecture and **react-router** for routing in our app.

With **iso** as helper we can populate **alt** flux stores before the first rendering and have a complete async isomorphic React application.

Run this boilerplate, you will see the server is fetching some fake users and will populate the `UserStore` with this data. **Koa** will render the first markup, serve the JavaScript and then it will entirely run on the client.

## Alt-resolver

Alt-resolver is used to resolve data before React Rendering, it shares the same services with client and server. It's specific to the boilerplate.

Presume you have a route like this:

```
<Route name="users" handler={require('./components/users')} />
```

And you need to fetch users and populate an UserStore before rendering the component, declare an `user` function (that match the route name) in `shared/service.js`.

This function take two arguments from promise `resolve, reject`, you should resolve it with a formatted object for `Iso / Alt` like this:

```
users(resolve, reject) {
  return request
    .get('http://api.randomuser.me/?results=10')
    .end((err, response) => {
      return resolve({UserStore: {users: response.body.results}});
    });
}
```

* On server side, `users` function will be resolved before the first rendering.
* On client-side when switching to the route, it will be resolved before rendering too!

## Installation / How-to

I recommend to use [io.js](https://iojs.org/) to take advantages of `ES6` without `--harmony` flag on `NodeJS`.

It's super easy to do with [nvm](https://github.com/creationix/nvm):

* `$ nvm install iojs`
* `$ nvm use iojs`
* `$ nvm alias default iojs` (to make `node` default to `iojs`)

After that, you will just need to clone the repo and install dependancies:

* `$ git clone https://github.com/iam4x/isomorphic-flux-boilerplate.git`
* `$ cd isomorphic-flux-boilerplate && npm install`
* `$ npm install -g gulp`

### Run the project in development:

* `$ gulp dev`

Open your browser to `http://localhost:8080` and you will see the magic happens! Try to disable JavaScript in your browser, you will still be able to navigate between pages of the application. Enjoy the power of isomorphic applications!


### Run tests

When using `$ gulp dev` tests will be run when you change a file `test/spec/**/*.test.jsx`

To run them once use: `gulp test:build`

### Build project:

Just run `$ gulp build`, it will produce these tasks:

* Run tests from `test/spec/**/*.jsx`
* Concat & minify styles to `/dist/css/styles.css`
* Concat & minify scripts to `/dist/js/app.js`
* Optimize & copy images to `/dist/img/`

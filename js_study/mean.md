
# プロジェクト作成手順

## フロントエンド

### Angular CLIでプロジェクト作成

```
$ ng new mean2-sample --style=scss
$ cd mean2-sample
$ npm install ng2-bootstrap bootstrap jquery --save
```

### angular-cli.json 修正

```javascript
// 省略
"styles": [
  "../node_modules/bootstrap/dist/css/bootstrap.min.css", // 追加行
  "styles.scss"
],
"scripts": [
  "../node_modules/jquery/dist/jquery.min.js",           // 追加行
  "../node_modules/bootstrap/dist/js/bootstrap.min.js"   // 追加行
],
// 省略
```

bootstrap、jqueryのスクリプト、スタイルシートを追加。

### src/app/app.module.ts 修正

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { HttpModule } from '@angular/http';
import { Ng2BootstrapModule } from 'ng2-bootstrap/ng2-bootstrap'; // 追加行

import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule,
    Ng2BootstrapModule // 追加行
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

[ng2-bootstrap](https://valor-software.com/ng2-bootstrap/#/)のモジュールを追加。

## サーバサイド

Angular CLIで作成したプロジェクトのサーバサイド、つまりExpressを追加する。  
ソースディレクトリは ```app/``` 。  
フロントエンドとは異なりトランスパイル不要。  
フロントエンドトランスパイル後の ```dist``` を公開ディレクトリとする。

### Expressの追加

```sh
npm install express@5.0.0-alpha.3 body-parser --save
mkdir app // サーバサイドExpressアプリ用のソースディレクトリ作成
touch app/app.js
mkdir app/routes
touch app/routes/index.js
```

### app/app.js

```
// Get dependencies
const express = require('express');
const path = require('path');
const http = require('http');
const bodyParser = require('body-parser');

// Get our API routes
const index = require('./routes/index');

const app = express();

// Parsers for POST data
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));

// Point static path to dist
app.use(express.static(path.join(__dirname, '../dist')));

// Set our api routes
app.use('/api', index);

// Catch all other routes and return the index file
app.get('*', (req, res) => {
  res.sendFile(path.join(__dirname, 'dist/index.html'));
});

/**
 * Get port from environment and store in Express.
 */
const port = process.env.PORT || '3000';
app.set('port', port);

/**
 * Create HTTP server.
 */
const server = http.createServer(app);

/**
 * Listen on provided port, on all network interfaces.
 */
server.listen(port, () => console.log(`API running on localhost:${port}`));
```

### app/routes/index.js

```
const express = require('express');
const router = express.Router();

/* GET api listing. */
router.get('/', (req, res) => {
  res.send('api works! /api accessed!');
});

module.exports = router;
```


## 起動

```
$ ng build // フロントエンドのビルド
$ node app/app.js  // サーバサイドアプリ起動
```







# 以降メモ

MEANスタックについて。  
ただし、Angular2。
下の構成を目指して ```@angular/cli``` にExpressを足す方針。

```
ng2-mean-webpack/
 │
 ├──app/                            * サーバサイド（Node.js、Express）
 │   ├──models/                     * model definitions for Mongoose
 │   │   └──user.model.js           * a user model for use with PassportJS
 │   ├──routes/                     * store all modular REST API routes for Express here
 │   │   └──authentication          * an Express route for use with PassportJS
 │   │        .router.js
 │   └──routes.js                   * gather all of your Express routes and middleware here
 │
 ├──config/                         * configuration files for environment variables, Mongoose, and PassportJS
 │   ├──config.json/                * allows definition of environment variables
 │   ├──env.conf.js/                * contains utility functions for setting up environment vars
 │   ├──mongoose.conf.js/           * configuration file for Mongoose
 │   └──passport.conf.js/           * configuration file for PassportJS
 │
 ├──sockets/                        * directory for socket.io functionality
 │   └──base.js/                    * a basic socket.io server function
 │
 ├──src/                            * フロントエンド（Angular2）
 │   ├──main.ts                     * our entry file for our browser environment
 │   │
 │   ├──index.html                  * Index.html: where we generate our index page
 │   │
 │   ├──polyfills.ts                * our polyfills file
 │   │
 │   ├──app/                        * WebApp: folder
 │   │   ├──todo/                   * an example component directory
 │   │   │   ├──todo.component.ts   * a simple Angular 2 component
 │   │   │   ├──todo.e2e.ts         *  simple test of components in todo.component.ts
 │   │   │   ├──todo.spec.ts        * a simple end-to-end test for /todo
 │   │   │   ├──todo.html           * template for our component
 │   │   │   └──todo.service.ts     * Angular 2 service linking to our API
 │   │   ├──app.spec.ts             * a simple test of components in app.ts
 │   │   ├──app.e2e.ts              * a simple end-to-end test for /
 │   │   └──app.ts                  * App.ts: a simple version of our App component components
 │   │
 │   ├──assets/                     * static assets are served here
 │   │   ├──icon/                   * our list of icons from www.favicon-generator.org
 │   │   ├──service-worker.js       * ignore this. Web App service worker that's not complete yet
 │   │   ├──robots.txt              * for search engines to crawl your website
 │   │   └──human.txt               * for humans to know who the developers are
 │   │
 │   └──sass/                       * folder for Sass stylesheets
 │       |
 │       ├──base/
 │       │   ├──_animations.scss    * Animation keyframe definitions
 │       │   ├──_reset.scss         * Reset/normalize
 │       │   ├──_typography.scss    * Typography rules
 │       │   ├──_module.scss        * Load all partials from this directory into single partial
 │       │   └── …                  * Etc.
 │       │
 │       ├──components/
 │       │   ├──_buttons.scss       * Buttons
 │       │   ├──_carousel.scss      * Carousel
 │       │   ├──_cover.scss         * Cover
 │       │   ├──_dropdown.scss      * Dropdown
 │       │   ├──_module.scss        * Load all partials from this directory into single partial
 │       │   └── …                  * Etc.
 │       │
 │       ├─ layout/
 │       │   ├──_navigation.scss    * Navigation
 │       │   ├──_grid.scss          * Grid system
 │       │   ├──_header.scss        * Header
 │       │   ├──_footer.scss        * Footer
 │       │   ├──_sidebar.scss       * Sidebar
 │       │   ├──_forms.scss         * Forms
 │       │   ├──_module.scss        * Load all partials from this directory into single partial
 │       │   └── …                  * Etc.
 │       │
 │       ├─ pages/
 │       │   ├──_home.scss          * Home specific styles
 │       │   ├──_contact.scss       * Contact specific styles
 │       │   ├──_module.scss        * Load all partials from this directory into single partial
 │       │   └── …                  * Etc.
 │       │
 │       ├─ themes/
 │       │   ├──_theme.scss         * Default theme
 │       │   ├──_admin.scss         * Admin theme
 │       │   ├──_module.scss        * Load all partials from this directory into single partial
 │       │   └── …                  * Etc.
 │       │
 │       ├─ utils/
 │       │   ├──_variables.scss     * Sass Variables
 │       │   ├──_functions.scss     * Sass Functions
 │       │   ├──_mixins.scss        * Sass Mixins
 │       │   ├──_helpers.scss       * Class & placeholders helpers
 │       │   ├──_module.scss        * Load all partials from this directory into single partial
 │       │   └── …                  * Etc.
 │       ├──vendors/
 │       │   ├──_bootstrap.scss     * Bootstrap
 │       │   ├──_jquery-ui.scss     * jQuery UI
 │       │   ├──_module.scss        * Load all partials from this directory into single partial
 │       │   └── …                  * Etc.
 │       │
 │       │
 │       └──main.scss               * Main sass file to load all partials for this project
 │
 ├──.babelrc                        * configure Babel 6 plugins and ES6/ES7 presets
 │
 ├──server.js                       * ES5 `.js` file importing the server code along with a Babel 6 hook to transpile server ES6/ES7 code on the fly
 ├──server.conf.js                  * configure Express/Socket.io application, connect to database, instantiate Mongoose models, define API and front-end Angular routes, et cetera
 │
 ├──gulpfile.js                     * ES5 `gulpfile` importing the `gulp` workflow code along with a Babel 6 hook to transpile the ES6 code on the fly
 ├──gulpfile.conf.js                * contains all of the workflow management delegated to `gulp`: auto documentation generation; `sass` linting; `nodemon`, et cetera
 │
 ├──spec-bundle.js                  * ignore this magic that sets up our angular 2 testing environment
 ├──karma.config.js                 * karma config for our unit tests
 ├──protractor.config.js            * protractor config for our end-to-end tests
 │
 ├──tsconfig.json                   * config that webpack uses for typescript
 ├──typings.json                    * our typings manager
 ├──package.json                    * what npm uses to manage it's dependencies
 │
 ├──webpack.config.js               * our development webpack config
 ├──webpack.test.config.js          * our testing webpack config
 └──webpack.prod.config.js          * our production webpack config
```

```app``` はトランスパイル無しで動作。  
```src``` はトランスパイルして ```dist```へゲロする。 ```server.js``` から ```dist/index.html``` が読まれる。

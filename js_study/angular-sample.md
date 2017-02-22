```sh
$ mkdir angular-sample
$ cd angular-sample
$ npm init -y

$ npm install --save \
      @angular/{core,common,compiler,forms,http,platform-browser,platform-browser-dynamic,router} \
      rxjs@ zone.js@ core-js
$ npm install --save-dev @types/node angular2-template-loader awesome-typescript-loader extract-text-webpack-plugin typescript webpack webpack-dev-server html-webpack-plugin

$ mkdir src; mkdir src/app; touch src/app/app.component.ts
$ touch src/main.ts; touch src/ventor.ts
$ touch src/index.html
$ touch webpack.config.js
$ touch tsconfig.json
```

## src/app/app.component.ts

```typescript
```

## src/main.ts

```typescript
```
## src/vendor.ts

```typescript
```

## src/index.html

```html
```

## webpack.config.js

```javascritp
```

## tsconfig.json

```javascript
```

# 参考

https://angular.io/docs/ts/latest/guide/webpack.html  
TypeScript+webpack+Angular公式設定系ガイド
↓
うごかない。。。↓がヒントになりそう  
https://github.com/ovrmrw/angular2-5minquickstart-webpack

# 話題

## TypeScriptの型解決について

解決方法にはイカがある。

- tsd
- typings
- @types

この辺参照。（@types/angularjsはAngular2じゃないので注意）  
http://qiita.com/laco0416/items/ed1aadf335f12cd3618d

なんとAngularは型定義が含まれているため、上記３つは不要！

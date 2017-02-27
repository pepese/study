# WebApplicationContext

SpringはDispatcherServletにつき１つWebApplicationContextをもつ。  
WebApplicationContextには以下の特別なbeanが含まれる。

|Bean type|Explanation|
|:---|:---|
|HandlerMapping|Maps incoming requests to handlers and a list of pre- and post-processors (handler interceptors) based on some criteria the details of which vary by HandlerMapping implementation. The most popular implementation supports annotated controllers but other implementations exists as well.|
|HandlerAdapter|Helps the DispatcherServlet to invoke a handler mapped to a request regardless of the handler is actually invoked. For example, invoking an annotated controller requires resolving various annotations. Thus the main purpose of a HandlerAdapter is to shield the DispatcherServlet from such details.|
|HandlerExceptionResolver|Maps exceptions to views also allowing for more complex exception handling code.
|ViewResolver|Resolves logical String-based view names to actual View types.|
|LocaleResolver & LocaleContextResolver|Resolves the locale a client is using and possibly their time zone, in order to be able to offer internationalized views|
|ThemeResolver|Resolves themes your web application can use, for example, to offer personalized layouts|
|MultipartResolver|Parses multi-part requests for example to support processing file uploads from HTML forms.|
|FlashMapManager|Stores and retrieves the "input" and the "output" FlashMap that can be used to pass attributes from one request to another, usually across a redirect.|

[参考](http://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/#mvc-servlet-special-bean-types)

# @RequestMapping

@RequestMappingにHTTPメソッドの意味を付与して以下のようなアノテーションがある。

- @GetMapping
- @PostMapping
- @PutMapping
- @DeleteMapping
- @PatchMapping

# @Controller の AOP Proxying

## リクエストからレスポンスまでの処理の挟まり具合

[これ最高](http://qiita.com/kazuki43zoo/items/757b557c05f548c6c5db)

## リクエスト、値の取得

```
// GET /pets/42;q=11;r=22
@GetMapping("/pets/{petId}")
public void findPet(@PathVariable String petId, @MatrixVariable int q) {
```

- @PathVariable：パスパラメータの取得
- @MatrixVariable：クエリパラメータの取得
- @RequestBody：ボディをオブジェクトで取得
  - Content-Typeでパースされる
- @ModelAttribute
  - スコープ内のブツが取れる？
- @RequestParam
  - クエリパラメータ？
- @RequestBody
  - 文字列で取れる？
- BindingResult
  - アノテーションじゃないけど、これが何か知りたい
- @SessionAttribute
- @RequestAttribute
- @CookieValue
- @RequestHeader

## レスポンス

- @ResponseBody
- @JsonView
- @ResponseStatus

[Jackson JSONP Support](http://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/#mvc-ann-jsonp)

## 例外ハンドリング

- @ExceptionHandler

[参考](http://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/#mvc-ann-rest-spring-mvc-exceptions)

## @ControllerAdvice / @RestControllerAdvice

```@ControllerAdvice``` には、@InitBinder、@ModelAttribute、@ExceptionHandler を設定したメソッドを定義できる。  
これらはそれぞれ、コントローラー内の @RequestMapping が設定されたメソッドの前後に割り込みを行う。  
以下の順番。

1. @ControllerAdviceを付与したクラスの@InitBinderを付与したメソッド
1. @ControllerAdviceを付与したクラスの@ModelAttributeを付与したメソッド
1. @Controllerを付与したクラスの@RequestMappingを付与したメソッド
1. @ControllerAdviceを付与したクラスの@ExceptionHandlerを付与したメソッド

## @InitBinder

WebDataBinderを取得してbindをカスタマイズできる。

[参考](http://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/#mvc-ann-webdatabinder)

registerCustomEditor、addCustomFormatterとかで使えるクラスは以下。（全部かどうかわからん）

|クラス名|内容|
|:---|:---|
|ByteArrayPropertyEditor|ByteArrayへの変換します。|
|ClassEditor|クラスを表す文字列をClassへ変換します。|
|CustomBooleanEditor|Booleanに変換します。|
|CustomCollectionEditor|Collectionへ変換します。|
|CustomDateEditor|java.util.Dateへ変換します。DateFormatを指定できます。デフォルトでは設定されていません。|
|CustomNumberEditor|様々なNumber派生クラス（例：Integer, Long, Float, Double）へ変換します。デフォルトで設定されていますが、書式を変更したい場合は上書きします。|
|FileEditor|java.io.Fileへ変換します。|
|InputStreamEditor|片方の変換しか提供しません。InputStreamに変換しますが、closeしないことに注意。|
|LocaleEditor|Localeに変換します。(書式は [language]_[country]_[variant])|
|PatternEditor|JavaのPatternクラスに変換します。|
|PropertiesEditor|java.lang.Properties に変換します|
|StringTrimmerEditor|文字列をtrimします。空文字はnull に変換されます。デフォルトでは登録されていません。|
|URLEditor|JavaのURLに変換します|

## その他

- HandlerInterceptor

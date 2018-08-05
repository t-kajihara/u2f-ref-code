![翻訳前](./README.org.md)

# U2F仕様のリファレンスコード

このコードは、http://fidoalliance.org/で開発されたFIDO U2F仕様を実装しています。
このコードは、U2Fを探索することに関心のある開発者のためのリファレンスとリソースを意図しています。
コードは、次のコンポーネントで構成されています。

## Java U2F実装

このコードは、U2Fの登録と署名を検証できます。 U2F第2因子を受け入れるように構築されたWebアプリケーションは、
このようなコードベースの上に構築されます。コードベースには簡単なWebアプリケーションが含まれているため、
ユーザーは登録と署名を試すことができます（以下のサンプルWebアプリケーションも参照してください）。

## 仮想（ソフトウェア）U2Fデバイス

これは、U2FデバイスのJava実装です。 それは登録と
署名ステートメントであり、サーバーに対するテスト用です
実装。 物理的なU2Fデバイスは、同様のステートメントを生成します。

## U2Fを使用したサンプルWebアプリケーション

これは、Google App Engine Webプラットフォーム上に構築されたサンプルアプリケーションです。
Webページ内のU2Fとのユーザ対話のための可能なUXを示しています。 ザ
サンプルアプリケーションが配備され、ライブで利用可能
https://crxjs-dot-u2fdemo.appspot.com/。 基礎となるU2F機能は、
Java U2F実装。 開発者はここからコアアイディアを得ることができます。
U2Fを自分の好きなWebアプリケーションプラットフォーム上のWebアプリケーションに統合します。

## Chromeブラウザ用のU2F拡張機能

この拡張機能は、U2F機能をChromeブラウザにもたらします。 Webアプリケーション
この拡張機能によって提供されるU2F APIを使用してUSB U2Fデバイスにアクセスできます。
拡張機能は[Chromeストアから入手可能] [ウェブストア]から直接利用できます。
ソースは `` u2f-chrome-extension``で利用できます。
[拡張README]（u2f-chrome-extension / README.md）を参照してください。

[webstore]: https://chrome.google.com/webstore/detail/fido-u2f-universal-2nd-fa/pfboblefjcgdjicmnffhdgionmgcdmne
* * *

エンドツーエンドのユーザーエクスペリエンスを体験するには、物理的な
仮想デバイス*はこれでUSB層をシミュレートしないため、USBデバイス
時間。 https://goo.gl/z0taoWにアクセスして、FIDO U2F準拠のデバイスを見つけることができます。
販売可能。

## Getting started

u2f-ref-codeは、基本的なWebサーバーを含む自己完結型のJavaプロジェクトです
すべての暗号、ユーティリティなどのパッケージを含みます。実行する必要はありません。
Tomcatのようなコンテナやアプリケーションサーバーで実行されます。 デモサーバーを実行するには、
`` com.google.u2f.tools.httpserver.U2fHttpServer``のメインクラス

Eclipseでサーバーをコンパイルして実行するには、Mavenプロジェクトを
ワークスペース。 ご使用のJDKのバージョンが次の場合は、クラスパスを修正する必要があります。
異なる（これはJava 1.7でテストされています）。 シンプルなデモWebサーバーは
`` com.google.u2f.tools.httpserver.U2fHttpServer.java``にあり、ポート
このクラスを通常のJavaアプリケーションとして実行します（右クリックし、* Runを選択します）。
*と* Javaアプリケーション*）。 U2F拡張機能が必要です
デモアプリケーションがあなたのU2Fトークンと話すためにChromeにインストールされます。

Mavenで直接実行するには、u2f-ref-codeディレクトリから `mvn compile exec：java`を実行してください。

### U2F-GAE-Demo

u2f-gae-demoプロジェクトは、Google App Engineで構築されたサンプルアプリケーションです
U2Fとのユーザーインタラクションの可能性を示すWebプラットフォーム
ウェブページ。

Mavenで開発サーバーを起動するには、 `mvn appengine：devserver`を実行します。 この
`http：// localhost：8888 /`でサーバーをローカルに実行します。

上記のように、EclipseにMavenプロジェクトをインポートする場合、調整する必要があるかもしれません
JDKのバージョン、App Engine SDKのバージョンなどすべてがコンパイルされると、
App Engineサーバーをローカルに配置し、Google Chromeを `http：// localhost：8888 /`にポイントします。

Google ChromeのU2Fのビルトインサポートは、HTTPSサイトでのみ機能します。 テストする
HTTPを使用する `http：// localhost：8888`のアプリケーションでは、
以下：

#### オプション1：Webストアの拡張機能を使用する
* Install the u2f extension [available from the Chrome store][webstore].
* Navigate to `chrome://extensions` and enable `Developer Mode` by clicking a
  checkbox in the top right corner.
* Find the `FIDO U2F (Universal 2nd Factor)` extension.
* Click on "background page". This will open a Developer Tools window, including
  a Console.
* In the console, type:

        HTTP_ORIGINS_ALLOWED = true;
* Now, configure the appspot server to call the U2F extension by setting the
  extension id in
  [u2f-api.js](https://github.com/google/u2f-ref-code/blob/master/u2f-gae-demo/war/js/u2f-api.js)
  to ```kmendfapggjehodndflmmgagdbamhnfd```:
```
  u2f.EXTENSION_ID = 'kmendfapggjehodndflmmgagdbamhnfd';
```
  Remember to reset this value before deploying.
* Then, point your browser at `http://localhost:8888/`.

#### オプション2：内蔵のクロムサポートを使用する
* Quit all instances of Google Chrome.
* Restart Google Chrome with the `--show-component-extension-options`
  command-line flag.
* Navigate to `chrome://extensions` and enable `Developer Mode` by clicking a
  checkbox in the top right corner.
* Find the `CryptoTokenExtension` extension.
* Click on "background page". This will open a Developer Tools window, including
  a Console.

* In the console, type:

        HTTP_ORIGINS_ALLOWED = true;
* Then, point your browser at http://localhost:8888/

You can deploy this App Engine app to your own domain by changing the application
name in `u2f-gae-demo/war/WEB-INF/appengine-web.xml`.


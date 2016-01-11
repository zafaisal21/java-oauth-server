認可サーバー実装 (Java)
=======================

概要
----

[OAuth 2.0][1] と [OpenID Connect][2] をサポートする認可サーバーの Java による実装です。

この実装は JAX-RS 2.0 API と [authlete-java-jaxrs][3] ライブラリを用いて書かれています。
JAX-RS は _The Java API for RESTful Web Services_ です。 JAX-RS 2.0 API は
[JSR 339][4] で標準化され、Java EE 7 に含まれています。 一方、authlete-java-jaxrs
は、認可サーバー実装用のユーティリティークラス群を提供するオープンソースライブラリです。
authlete-java-jaxrs は [authlete-java-common][5] ライブラリを使用しており、こちらは
[Authlete Web API][6] とやりとりするためのオープンソースライブラリです。

この実装は「DB レス」です。 これの意味するところは、認可データ (アクセストークン等)
や認可サーバー自体の設定、クライアントアプリケーション群の設定を保持するためのデータベースを用意する必要がないということです。
これは、[Authlete][7] をバックエンドサービスとして利用することにより実現しています。


ライセンス
----------

  Apache License, Version 2.0


ソースコード
------------

  https://github.com/authlete/java-oauth-server


Authlete について
-----------------

[Authlete][7] (オースリート) は、OAuth 2.0 & OpenID Connect
の実装をクラウドで提供するサービスです ([overview][8])。 Authlete
が提供するデフォルト実装を使うことにより、もしくはこの実装 (java-oauth-server)
でおこなっているように [Authlete Web API][6]
を用いて認可サーバーを自分で実装することにより、OAuth 2.0 と OpenID Connect
の機能を簡単に実現できます。

この認可サーバーの実装を使うには、Authlete から API
クレデンシャルズを取得し、`authlete.properties` に設定する必要があります。
API クレデンシャルズを取得する手順はとても簡単です。
単にアカウントを登録するだけで済みます ([サインアップ][9])。
詳細は [Getting Started][10] を参照してください。


実行方法
--------

1. この認可サーバーの実装をダウンロードします。

        $ git clone https://github.com/authlete/java-oauth-server.git
        $ cd java-oauth-server

2. 設定ファイルを編集して API クレデンシャルズをセットします。

        $ vi authlete.properties

3. [http://localhost:8080][38] で認可サーバーを起動します。

        $ mvn jetty:run &


エンドポイント
--------------

この実装は、[認可エンドポイント][11]と[トークンエンドポイント][12]の二つのエンドポイントを提供します。
パスは下表のとおりです。

| エンドポイント        | パス               |
|:----------------------|:-------------------|
| 認可エンドポイント    | /api/authorization |
| トークンエンドポイント| /api/token         |

認可エンドポイントは、[RFC 6749][1]、[OpenID Connect Core 1.0][13]、及び
[RFC 7636][14] ([PKCE][15]) で説明されているパラメーター群を受け付けます。


認可リクエストの例
------------------

次の例は [Implicit フロー][16]を用いて認可エンドポイントからアクセストークンを取得する例です。
`{クライアントID}` となっているところは、あなたのクライアントアプリケーションの実際のクライアント
ID で置き換えてください。

    http://localhost:8080/api/authorization?client_id={クライアントID}&response_type=token

クライアントアプリケーションについては、[Getting Started][10]
および開発者コンソールの[ドキュメント][17]を参照してください。


カスタマイズ
------------

この実装をカスタマイズする方法については [CUSTOMIZATION.ja.md][39] に記述されています。
Authlete はユーザーアカウントを管理しないので、基本的には「ユーザー認証」に関わる部分についてプログラミングが必要となります。
これは設計によるものです。 ユーザー認証のための仕組みを持っている既存の Web
サービスにもスムーズに OAuth 2.0 と OpenID Connect の機能を組み込めるようにするため、Authlete
のアーキテクチャーは認証と認可を慎重に分離しています。


実装に関する注意
----------------

この実装では、認可ページを実装するために `Viewable` クラスを使用しています。
このクラスは [Jersey][18] (JAX-RS の参照実装) に含まれているものですが、JAX-RS
2.0 API の一部ではありません。


関連仕様
--------

- [RFC 6749][1] - The OAuth 2.0 Authorization Framework
- [RFC 6750][19] - The OAuth 2.0 Authorization Framework: Bearer Token Usage
- [RFC 6819][20] - OAuth 2.0 Threat Model and Security Considerations
- [RFC 7009][21] - OAuth 2.0 Token Revocation
- [RFC 7033][22] - WebFinger
- [RFC 7515][23] - JSON Web Signature (JWS)
- [RFC 7516][24] - JSON Web Encryption (JWE)
- [RFC 7517][25] - JSON Web Key (JWK)
- [RFC 7518][26] - JSON Web Algorithms (JWA)
- [RFC 7519][27] - JSON Web Token (JWT)
- [RFC 7521][28] - Assertion Framework for OAuth 2.0 Client Authentication and Authorization Grants
- [RFC 7522][29] - Security Assertion Markup Language (SAML) 2.0 Profile for OAuth 2.0 Client Authentication and Authorization Grants
- [RFC 7523][30] - JSON Web Token (JWT) Profile for OAuth 2.0 Client Authentication and Authorization Grants
- [RFC 7636][31] - Proof Key for Code Exchange by OAuth Public Clients
- [RFC 7662][32] - OAuth 2.0 Token Introspection
- [OAuth 2.0 Multiple Response Type Encoding Practices][33]
- [OAuth 2.0 Form Post Response Mode][34]
- [OpenID Connect Core 1.0][13]
- [OpenID Connect Discovery 1.0][35]
- [OpenID Connect Dynamic Client Registration 1.0][36]
- [OpenID Connect Session Management 1.0][37]


その他の情報
------------

- [Authlete][7] - Authlete ホームページ
- [authlete-java-common][5] - Java 用 Authlete 共通ライブラリ
- [authlete-java-jaxrs][3] - JAX-RS (Java) 用 Authlete ライブラリ


サポート
--------

[Authlete, Inc.](https://www.authlete.com/)<br/>
support@authlete.com


[1]: http://tools.ietf.org/html/rfc6749
[2]: http://openid.net/connect/
[3]: https://github.com/authlete/authlete-java-jaxrs
[4]: https://jcp.org/en/jsr/detail?id=339
[5]: https://github.com/authlete/authlete-java-common
[6]: https://www.authlete.com/documents/apis
[7]: https://www.authlete.com/
[8]: https://www.authlete.com/documents/overview
[9]: https://so.authlete.com/accounts/signup
[10]: https://www.authlete.com/documents/getting_started
[11]: https://tools.ietf.org/html/rfc6749#section-3.1
[12]: https://tools.ietf.org/html/rfc6749#section-3.2
[13]: http://openid.net/specs/openid-connect-core-1_0.html
[14]: http://tools.ietf.org/html/rfc7636
[15]: https://www.authlete.com/documents/article/pkce
[16]: http://tools.ietf.org/html/rfc6749#section-4.2
[17]: https://www.authlete.com/documents/cd_console
[18]: https://jersey.java.net/
[19]: http://tools.ietf.org/html/rfc6750
[20]: http://tools.ietf.org/html/rfc6819
[21]: http://tools.ietf.org/html/rfc7009
[22]: http://tools.ietf.org/html/rfc7033
[23]: http://tools.ietf.org/html/rfc7515
[24]: http://tools.ietf.org/html/rfc7516
[25]: http://tools.ietf.org/html/rfc7517
[26]: http://tools.ietf.org/html/rfc7518
[27]: http://tools.ietf.org/html/rfc7519
[28]: http://tools.ietf.org/html/rfc7521
[29]: http://tools.ietf.org/html/rfc7522
[30]: http://tools.ietf.org/html/rfc7523
[31]: http://tools.ietf.org/html/rfc7636
[32]: http://tools.ietf.org/html/rfc7662
[33]: http://openid.net/specs/oauth-v2-multiple-response-types-1_0.html
[34]: http://openid.net/specs/oauth-v2-form-post-response-mode-1_0.html
[35]: http://openid.net/specs/openid-connect-discovery-1_0.html
[36]: http://openid.net/specs/openid-connect-registration-1_0.html
[37]: http://openid.net/specs/openid-connect-session-1_0.html
[38]: http://localhost:8080
[39]: CUSTOMIZATION.ja.md
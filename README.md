# Rollup Test

JavaScript のモジュールバンドラ Rollup を試す用のリポジトリ

- [Rollup](https://rollupjs.org/guide/en/)

## Installation

```sh
npm init
npm i --save-dev typescript ts-node rollup @wessberg/rollup-plugin-ts
```

## バンドラの利点

- 転送の最適化
  - HTTP/1.1 接続では 1 リクエスト 1 ファイル。そのため、バンドラを使って複数ファイルを一つにバンドルすれば 1 回の転送で Web コンテンツを転送でき、リクエスト数を最小限にできる。
- モジュールが使える
- （Webpack）JS だけでなく、CSS も画像もバンドルできる

![Webpack](https://ics.media/entry/12140/images/160519_webpack_is.png)

（https://ics.media/entry/12140/ より引用）

- タスクランナーとして働く
  - JS ファイルの圧縮やソースマップに対応していたり、ローカルサーバの起動まで包括的な環境が手に入る。Gulp や npm scripts だけではカバーできない範囲までカバーできる。

## Similar Tools

類似ツールは Webpack などのバンドラ。

- Browserify
- Webpack

### Webpack と Rollup の違い

- ネイティブなモジュールシステム
  - Webpack
    - Webpack 1 では CommonJS しか認識できず、ES6 Module で書かれたものは Babel などで下処理しないと処理不能だったが、Webpack 2 以上では ES6 Module も認識できるようになっている（Babel の ES6 Module 変換は止めておく必要がある）。
  - Rollup
    - Rollup は ES6 Module だけをネイティブに実装していて、CommonJS の処理にはプラグインが必要。
- ES のバージョン
  - Webpack
    - Webpack の場合、Webpack 2 以上では ES6 も通せるはずだが、`babel-loader` で ES5 に変換してから読み込むのが一般的な使用法となっている。
  - Rollup
    - Rollup は JavaScript としても ES6 が前提になっていて、ES5 にしたい場合は出力時に Babel を通す。
  - この違いが目立つのは、`class` や `...` など、ES5 に変換しようとすれば補助関数が必要になるようなコード。モジュールごとに変換をかける Webpack では、同じ関数があちこちに作られてしまうが、Rollup では全体に 1 つで済むため、バンドル後のファイルサイズが小さくなる。
- 名前空間の扱い
  - Webpack
    - Webpack では「ファイルの名前空間を、関数を使って再現する」という方針を取っている
  - Rollup
    - 一方で、Rollup では「名前の衝突はリネームで解決して、フラットな空間に開く」という形になっている。そのため、コードサイズ的には Rollup のほうが有利なのに対して、「ファイル分割」や「動的ロード」といった、モジュール間を疎結合にするような動作については Webpack が有利となる。
- JavaScript 以外の扱い
  - Webpack
    - Webpack は JavaScript 以外の CSS や画像に関しても活用されている
  - Rollup
    - Rollup は基本的に JavaScript 専用のバンドラとして動作する形になっている

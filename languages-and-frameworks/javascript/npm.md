# npm

## npm ci

[公式](https://docs.npmjs.com/cli/v6/commands/npm-ci)

CI環境などで利用されることを目的としていて、高速でクリーンなインストールを行うものらしい。

* `package-lock.json`か`npm-shrinkwrap.json`がその環境にないとダメ
* ↑の内容が`package.json`と異なるとダメ
* `npm install ライブラリ名`みたいな使い方は npm ciには用意されていなくて、プロジェクト全体の依存ライブラリインストールにしか使えない
* プロジェクト内にnode\_modulesが存在する場合、それを消してからインストールをする（＝クリーンインストールしてくれるってことやな）
* `npm install`と違って、`package.json`や`package-lock.json`を更新することはない

という違いがあるらしい。


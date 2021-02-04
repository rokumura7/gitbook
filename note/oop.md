オブジェクト指向ってものに向き合って５，６年経ち、
分かった！と思ったらまた次の壁にぶち当たり、また分かった！と思ったら分からなくなり、
そして遂に僕は**オブジェクト指向が分からないということが分かりました**🍣

先輩諸兄の知識を多分に借りながら、僕なりの現時点のオブジェクト指向の理解を書いてみる。

# オブジェクト指向とは

* 継承
* 多態性
* 隠蔽化

オブジェクト指向の解説には必ずと言ってもいいほどこれらの**三原則**がでてきます。
この３原則を軸に、皆様のスマートフォンを題材に考えてみます。

## 継承

さて、今しがた「スマートフォン」と書きました。皆様の手元にも「スマートフォン」があると思います。
ただ、この「スマートフォン」、人によってiPhoneだったりPixelだったり、もっと細かく言うとiPhoneXだったりPixel4だったり、多種多様だと思います。

この**概念的なまとまりが継承**です。

継承とは、抽象化と具体化です。
抽象的な概念があり、そこからより具体的なものに向かって行くイメージです。
そしてその時、**具体と抽象はis-aの関係**のはずです。
iPhoneXはiPhoneである、ということです。逆はありえませんよね。

もう一つ述べておかなければならないのが振る舞いについてです。
例えば、僕が「手元のスマートフォンでxxx-xxxx-xxxxに電話をしてください」とお願いしたとして、
皆様は手元のスマートフォンを用いて、それがiPhoneだろうがPixelだろうが、電話をかけることができます。
それ以外にも、ネットに接続できて、写真が撮れて・・・と色々できると思います。
これらのできることは、スマートフォンであればみな共通してできることです。

このように、その**抽象的な概念が共通してできる振る舞いを提供（＝定義）する**ことも継承の本質になります。
インターフェースを提供する、というのはこのことです。

当たり前のこと書いていますし、多くの方がこの継承について理解はされていると思いますが、
ソースコードを読んでいると、あたかも「`extends`などを用いて、他のクラスの機能を使えるようにすることが継承である」かのように思える時があります。
何でもできる便利なクラスを作ることが継承ではありません。

## 多態性

先の話の続きになります。
多態性。ポリモフィズムとかって言ったりもします。

継承によって共通の振る舞いがスマートフォンにはあることが分かりましたが、
その振る舞いがスマートフォン毎で実際にどのような処理を行うかだったりその結果がどうなるかはOSや機種によって異なるはずです。

例えば、スマートフォンで写真を撮ってみましょう。
「写真を撮る」という振る舞いですが、
レンズが望遠と広角の２つレンズが搭載されているスマートフォンもありますし、
写真の保存が内部のストレージにされる設定がされているかも知れませんしクラウドに保存されるようになっていることもあるでしょう。

このように、**抽象的な振る舞いに対して、具象それぞれの結果が異なること**を多態性と言います。
抽象的なインターフェースに対して、具体的な処理をプログラムすることと言っても良いかも知れません。

## 隠蔽化

さて、個人的にはここを一番勘違いされている方が多いように思います。
隠蔽化。カプセル化とかモジュール化とかって言ったりもします。
継承や多態性がまず重要、みたいな印象を持たれている気がしますが、隠蔽化も非常に大事です。

まず説明の前に、ゲッターセッターの話をよく見ますが、この話は一旦忘れたほうが良いと思います。
（という話を多くの先輩諸兄もされているにも関わらずこの解説が蔓延るのは何故なのでしょう。）

話をスマートフォンに戻しましょう。
多くの方がスマートフォンの使い方をご存知かと思いますが、その内部はどうでしょうか？
僕は正直全然知らないです。CPUとメモリとバッテリーとカメラがあってー、、知らん。って感じです。
説明書も可能な限り読まずに済ませたいです🍣

電源を入れるという振る舞いを考えた際に、
搭載されているバッテリーから電力をもらい、OSを起動して、必要な情報を収集して、ディスプレイに表示して・・・
と内部では行われていますが（知らんけど）、僕らはそのことを知らなくても電源を入れることができます。

要は、スマートフォンのユーザー側としては、
中のことを知る必要がなくイジる必要もなく、シンプルに使いたいわけです。

スマートフォンの製造者側としても、
中のこととか気にすること使いやすくしたい、という思いで製造されているはずです。
（勝手に変な改造加えられては困る、とも思っているはずです。）

つまり、**必要十分最小に洗練されているもの**をお互い求めています。
隠蔽化とは、**シンプルに提供する**を意味しています。

ここまで読んでいただければ、ゲッターセッターが本質ではないことは分かっていただけるかと思います。
**構造を内部に隠し、必要なインターフェースのみを公開する**、これが隠蔽化です。

# さいごに
つらつら書いたけどまだまだ勉強中。マサカリ大歓迎。

# 参考

* [Qiita：オブジェクト指向タグ](https://qiita.com/tags/%e3%82%aa%e3%83%96%e3%82%b8%e3%82%a7%e3%82%af%e3%83%88%e6%8c%87%e5%90%91)
（色々読み漁ったため。まとめてで恐縮です。。）

* [Wikipedia：カプセル化](https://ja.wikipedia.org/wiki/%E3%82%AB%E3%83%97%E3%82%BB%E3%83%AB%E5%8C%96)
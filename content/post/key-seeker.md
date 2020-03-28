---
title: "i18nのキーをつらつら書くたびに欲しくなってたvimプラグインを作った"
slug: key-seeker
date: 2019-04-23
categories:
- tech
tags:
- vim
thumbnailImage: /img/no-image.jpg
---

他サイトに掲載していた過去記事サルベージ
<!--more-->
以下本文

-------

## 解決したかったこと
例えばこんな感じのi18n用resourceファイルがあったとして、`Login`という文字列を表示したければ、`header.menu.label.login`というキーを書く必要がある。

```json
{
  "header": {
    "menu": {
      "label": {
        "help": "Help",
        "login": "Login",
        "logout": "Logout"
      }
    }
   }
 }
```

手動で。

階層が深くルートまでが画面に収まっていなければ、スクロールして親を辿っていかなければならない。

もちろん手動で。

エディタが賢いとキーを書いているときにそれっぽく補完してくれたりもするが、最終地点の`Login`という場所が分かっているのだから、ちまちま補完などせずに一発でキーを取得したいと長らく願い続けてきた。

## プラグイン作った
いい感じのプラグインがきっとあるだろうと探し回ったりしたものの、どうもそれらしきものは見つからなかった。もしかして誰も困ってないのだろうか、それとも探し方が悪いだけなのだろうかと悩む日々を過ごすうちに、大した処理でもなさそうだし自分で作ったほうが早いのではということで早速作ってみた。
https://github.com/miyanokomiya/key-seeker.nvim

キーが欲しい行にカーソルを置いて`:KeySeekerClip`を実行すると、ルートからその行までのキーを`.`で連結した文字列をヤンクしてくれるだけ。
![Apr-23-2019 18-37-40.gif](/img/post/key-seeker/44_e.gif)

[neovim](https://github.com/neovim/neovim)のrpluginを使ってpythonでプラグインを書ける[pynvim](https://github.com/neovim/pynvim)を使っているので実質neovim用。
vimscriptを完全に理解できていない新参者にもなんとなくプラグインを作らせてくれる最高の仕組みだった。
ちなみになんとなく選んだもののpythonもほぼ触ったことはないので、`import`まわりにとても苦戦した。得意な環境で勝負することをおすすめしたい。

## 妥協点
リポジトリを覗いてもらうと分かるが、全然実装は頑張っていない。ただそれっぽく動いてくれる程度の代物で、対応できるファイルフォーマットもそこそこの制限がある。
ただ今回の動機となったi18nのresourceファイルでは（おそらく他のほとんどのjsonファイルでも）、そんな変な書き方はしないだろうということで妥協しまくっている。
そもそもjsonとしてパースしてすらいない。そのかわりにyamlファイルでもなんとなく使える。

このあたりは実際に問題にぶつかったらまた頑張るかもしれない。それか「このプラグイン使えばいいYO!」という情報が現れるのが先かもしれない。
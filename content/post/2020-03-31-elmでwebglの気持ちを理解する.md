---
title: ElmでWebGL奮闘記
date: '2020-03-31'
slug: elm-webgl-init
categories:
  - tech
  - ElmでWebGL奮闘記
tags:
  - Elm
  - WebGL
thumbnailImage: /img/uploads/スクリーンショット-2020-04-01-0.05.35.png
---
Three.js経由でしかWebGLを使ったことがなかった人間がElmで3D表現するためにWebGLと向かい合った奮闘記。

<!--more-->

## 今までしていた勘違い
まず一番大きな勘違いは、WebGLを「3D世界を表現するためのエンジン」と考えていたこと。この考えは完全にThree.jsを引きずっている。

Three.jsであれば、`World`があって`Engine`が動いて`Camera`と`Light`を設置して`Mesh`を登場させるというまさに「3D世界」を表現するための要素が揃っている。

一方WebGLがやってくれることは一体何か。「与えられたベクトルと行列情報に基づいて平面状に描画する」、この機能実現のために強力な行列計算機能も内包しているが、基本的にやってくれることはこれが全てある。

「3D世界」なる概念はどこにも存在しない。`Camera`も`Light`も`Mesh`も全て分け隔てなくただのベクトルや行列として扱われる。ただのベクトルや行列を巧妙かつ緻密に抽象化して「3D世界」という人間にも理解しやすいインタフェースとして我々に提示してくれているのがThree.jsという存在だった。

ひとたびThree.jsという仲介者が居なくなれば、目の前にはWebGLという、全てが線形代数によって支配される無慈悲な世界が広がっている。

## ElmとWegGL

当然ながらThree.jsなる存在は（探した限り）Elmの世界には存在しない。相当にステートフルなライブラリなのでおそらくElm移植は絶望的ではないだろうか。しかし幸いなことにWebGLそのものをElmで扱うためのパッケージは存在していた。  

https://package.elm-lang.org/packages/elm-explorations/webgl/latest/WebGL

elm-explorations配下にあるのでElmでWebGLはまだまだ発展途上なのかもしれない。パッケージすら存在しなかったのであれば諦めはついたが、使えてしまうのなら仕方ない。Three.jsの仲介なしで、ElmでWebGLを使った3Dゲームを作る旅がこうして始まった。

着々と進んではいるものの、完成までの道のりが長すぎるのでWebGL奮闘記として多分続く。


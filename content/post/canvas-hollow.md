---
title: "長年欲しかったcanvasでくり抜きポリゴンを描画する方法"
slug: canvas-hollow
date: 2019-05-03
categories:
- tech
tags:
- canvas
thumbnailImage: "/img/post/canvas-hollow/33_b.png"
---

他サイトに掲載していた過去記事サルベージ
<!--more-->
以下本文

-------

## やりたいこと
こういう中身がくり抜かれたポリゴンをHTMLのcanvasで描画したい。
![May-03-2019 19.29.39](/img/post/canvas-hollow/33_b.png)

## 真っ先に浮かぶ方法
ブレンドモードを指定してポリゴンの重なり部分を切り取る方法。

```js
ctx.globalCompositeOperation = 'xor'
```
https://www.yoheim.net/blog.php?q=20121206

しかしこの方法は、「今のcanvasに描画されている部分」と「これから描画するポリゴン」の重なり部分が切り取られてしまう。

つまりこのような描画をしておいて、

```js
  ctx.fillStyle = 'red'
  ctx.beginPath()
  ctx.moveTo(10, 40)
  ctx.lineTo(150, 40)
  ctx.lineTo(150, 100)
  ctx.lineTo(10, 100)
  ctx.closePath()
  ctx.fill()

  ctx.fillStyle = 'blue'
  ctx.beginPath()
  ctx.moveTo(50, 10)
  ctx.lineTo(120, 10)
  ctx.lineTo(120, 150)
  ctx.lineTo(50, 150)
  ctx.closePath()
  ctx.fill()
```

![May-03-2019 19.24.43](/img/post/canvas-hollow/36_6.png)

くり抜き描画を追加で行うと、

```js
  ctx.globalCompositeOperation = 'xor'
  ctx.beginPath()
  ctx.moveTo(70, 30)
  ctx.lineTo(100, 30)
  ctx.lineTo(100, 120)
  ctx.lineTo(70, 120)
  ctx.closePath()
  ctx.fill()
```

こうなる。
![May-03-2019 19.26.26](/img/post/canvas-hollow/34_0.png)

違うんだ、こうなって欲しいんだという願いは叶わない。
![May-03-2019 19.29.39](/img/post/canvas-hollow/33_b.png)

## 欲しかった方法
SVGの挙動を調べていたらこういう仕様を見つけた。
> path要素においてはfill-ruleがnonzeroであっても，白抜きとなるケースが発生する．複数のパスから構成されたpash図形であった場合，互いにパスの向きが逆向きであった場合は，内側と外側とが打ち消し合い，fill対象の領域から除外される．

出展サイト：http://defghi1977.html.xdomain.jp/tech/svgMemo/svgMemo_03.htm

そしてこれは、canvasでも成り立つ。
つまりこう書けば、

```js
  ctx.fillStyle = 'red'
  ctx.beginPath()
  ctx.moveTo(10, 40)
  ctx.lineTo(150, 40)
  ctx.lineTo(150, 100)
  ctx.lineTo(10, 100)
  ctx.closePath()
  ctx.fill()

  ctx.fillStyle = 'blue'
  ctx.beginPath()
  ctx.moveTo(50, 10)
  ctx.lineTo(120, 10)
  ctx.lineTo(120, 150)
  ctx.lineTo(50, 150)
  ctx.closePath()

  ctx.moveTo(70, 30)
  ctx.lineTo(70, 120)
  ctx.lineTo(100, 120)
  ctx.lineTo(100, 30)
  ctx.closePath()
  ctx.fill()
```

これが手に入る。
![May-03-2019 19.29.39](/img/post/canvas-hollow/33_b.png)

ポイントはここ。
くり抜かれる側の矩形と、くり抜き範囲となる側の矩形で座標定義の回転方向が逆向きになっている。そして同じ`beginPath()`の中でパスを作ってから一気に`fill()`する。
要するにSVGでの仕様と同じ。SVGというよりは、もっと根底にある描画システム的な共通仕様なのかもしれない。

```js
  ctx.fillStyle = 'blue'
  ctx.beginPath()
  ctx.moveTo(50, 10)
  ctx.lineTo(120, 10)
  ctx.lineTo(120, 150)
  ctx.lineTo(50, 150)
  ctx.closePath()

  ctx.moveTo(70, 30)
  ctx.lineTo(70, 120)
  ctx.lineTo(100, 120)
  ctx.lineTo(100, 30)
  ctx.closePath()
  ctx.fill()
```

回転方向を交互に変えていけばさらにくり抜きをネストしていくこともできてしまう。楽しい。
![May-03-2019 19.44.15](/img/post/canvas-hollow/35_6.png)

もちろん包含関係になくても`xor`な描画として使うことができるので、今まで頑張って座標をポチポチ作っていた場面でもスパッと`xor`を決めることができてしまう。

昔仕事でこういうくり抜きが欲しくて調べまくったのに結局わからず諦めたことがあったが、canvasではなくSVGを調べることで思わぬリベンジを果たすことができた。
これからは思う存分くり抜きまくろう。

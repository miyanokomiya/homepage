---
title: "岡田スネーク"
slug: okada-snake
date: 2020-03-28
categories:
- tech
tags:
- SVG
- elm
- 岡田
thumbnailImage: "/img/post/okada-snake/snap.jpg"
---

数年ぶりにゲームの体裁を整えた新作を作った。
<!--more-->

https://miyanokomiya.github.io/okada-snake/
![okada-snake](/img/post/okada-snake/movie.gif)

## スネークゲーム
見たまんまのスネークゲーム。

ただし自動で進むことはなく、今と未来の盤面を見極めて一手一手進めていく方式。  
最近[Slay the Spire](https://store.steampowered.com/app/646570/Slay_the_Spire/?l=japanese)にドハマりしていて、配られたカードで予告された未来に対処することがゲームに深みをもたらすと学んだ。

ついでに岡田ゲームらしく岡と田を交互に拾い集めていくことが求められる。

モバイル向けの最適化は甘々なので操作性は微妙。PCの十字キーならさくさく移動できる。

## elm
今回の利用言語はelm。

半年くらい前になんとなく触ってみたときはelm本体のバージョンアップにSVG用パッケージが追従されておらず、SVGタグ生成を自力で実装していくという地道な作業だけで終わってしまった過去があったり。

久しぶりにelmをまた触りたくなって試してみたらSVGパッケージもすんなり使えて最高だった。
[elm/svg/1.0.1](https://package.elm-lang.org/packages/elm/svg/1.0.1/)

そして勢いのままにSVG使った何かを作ることに。elm力は皆無で膨らませすぎると投げ出してしまうので、向こう見ずに作り始めてある程度作り込める程度な規模感を求めて行き着いたのは久々の岡田ゲームだった。

岡田フォントを切ったりはしてたけど、ゲームとして最低限の体裁を整えたのは実に3年ぶりくらいかもしれない。待ち望んでくれていた人たちには本当に申し訳なかった。

## 過去作品復刻計画
さくらVPSで運営していたこのホームページの前世はメンテの面倒さから閉鎖してしまい、一部作品をアップしていたjsdo.itも終わってしまったことで過去作品は現在消失してしまっている。

まともにバージョン管理していなかったり、若気の至りで凄まじいコードになっていること間違いなしなので、サルベージよりリメイクした方が早いかもしれない。いずれにせよ何らかの形で復刻したい。
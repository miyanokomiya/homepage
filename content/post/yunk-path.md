---
title: "vimでカレントファイルパスを素早くヤンクしたい"
slug: yunk-path
date: 2019-04-19
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

`<C-g>`で表示はすぐできるのに、ヤンクはすぐできないもどかしさを解消したかった。

```vim
function! ClipText(data)
  let @0=a:data
  let @"=a:data
  let @*=a:data
endfunction
nnoremap <C-g> :call ClipText(expand('%'))<CR><C-g>
```

Thanks: https://gist.github.com/pinzolo/8168337

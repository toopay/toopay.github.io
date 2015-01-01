---
layout: post
title: New Year, Erlang Way...
---

Its not a coincident that this, my first blog post, was written in the very beginning of 2015. Last year i was massively busy, mainly developing several apps for [SaaSVentures](http://saasventures.co), and barely can't have luxury time for writing. 

Since i have two days for new year frenzied, i think its maybe a good time to start rolling this blog as place for me to put things i want to remember.

## Why Erlang?

There is a little girl. She's one year old. And this morning, when she wake up, she saw a thing, which was latter become her favourite amongst other stuff in her naive childhoold life. She never see this item before, even adults around her use it in daily basis without her being noticed. There is a light - so many lights, go around this small item, which projecting many images. And surprisingly, it also generate many different sounds too. She was amazed, and reach it eagerly to investigate. It was her parent smartphone, that apparently her mom forgot to keep it safely last night. Her life is never be the same ever since then.

My first encounter with Erlang was pretty much the same story. As for today, i already wrote roughly 2K LOC of Erlang, building my first OTP application, yet, this Excelent (with big "E") platform still surprised me in the way that none of my previous programming language(s) did! There are several key points i'm about to share, in my very beginning journey of Erlang world.

> **DISCLAIMER** If you are a language fanatic, you might not enjoy to reading further as it may contains my random ramblings upon your worshiped language. Instead, you might go straight to [this entertaining site](http://www.catsthatlooklikehitler.com/cgi-bin/seigmiaow.pl).

## Its an easy to learn Language!

As a person, i'm easy to get bored by tedious things. I prefer simplicity over monolithic stuff, and if i must added here, instead a collection of Mozart symfony, i only have Coldplay on my Soundcloud playlist.

Same thing applied to my programming life. I was porting my OpenCV experiments from C into [Python](https://github.com/toopay/area51) and i'd once wrote [Markdwon editor](https://github.com/toopay/bootstrap-markdown) - a very simple one - in favor of using bloated editor that available in that time.

I'm not interested with [Lisp](http://en.wikipedia.org/wiki/Lisp_%28programming_language%29) and i hate [Java](https://www.facebook.com/frei.denken/posts/10203180194450253). I'm avoid any platform that both requires me to think hard on figuring out the language construct and make it difficult for me to finding right keyword from its grammar specification. 

So first and foremost, we may start with a simple question, how many keywords that Erlang have?

```
after apply attributes call case
catch do end fun in
let letrec module of primop
receive try when
```

There are only 18 keywords! Thats enough for me. As addition for being easy to learn, its also fun. Show me some code you say? A few days ago, i came across [this quiz](http://codegolf.stackexchange.com/questions/22533/weirdest-obfuscated-hello-world?newreg=44a2bf18dcc44f7dbdddc27caf1b4ceb). And since there is no Erlang entry on those answer list, i try to create one. The result :

{% gist ae540adeea470bfb05f5 %}

For those who want to know how it works, its actually very simple. First of all, the straight forward version would be :

{% gist facecceb90f502913de1 %}

Since the quiz do not accept any string, we can exploit Hexadecimal to produce the character with the help of both Erlang [pattern matching] and [list comprehensions]. For example, we can produce `**H**` from `16#48` by bellow operation :

![Hello-1]({{ site.baseurl }}/images/hello-1.png)


## Fault tolerance, Concurency and Hot-Code swapping



## And the journey still goes on...

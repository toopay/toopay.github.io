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

Since the quiz do not accept any string, we can exploit Hexadecimal to produce the character with the help of both Erlang [pattern matching](http://erlang.org/doc/reference_manual/patterns.html) and [list comprehensions](http://www.erlang.org/doc/programming_examples/list_comprehensions.html). 

For example, we can produce `H` string from `16#48` by  : ```[X || X <- [16#48]]```. Which then, could be obsfucated even further using some bit syntax : ``` [X+Y+Z || <<X:8,Y:8,Z:8>> <= <<72:24>>] ```.

The last bit syntax operation is simply assigning the integer representatiof of previous hex value as 24 bit data into three 8 bit variable and concenate them together, as shown on bellow Erlang shell:

![Hello-1]({{ site.baseurl }}/images/hello-1.png)

The rest should be fairly simple, as its just a matter using `erlang:apply` as callback and converting `io` and `format` atoms with same technique described above.

From this point, while above obsfucated "Hello world!" code is obviously useless, but i hope its displaying simple fact that both Erlang list comprehension and pattern matching features, two main features that you'll use alot when you're developing an Erlang app, are dandy for operating many types of data (hex and bit syntax in those example). In some circumtances, it is a very powerfull way to abstracting complex problem down to just several lines of codes.

## Fault tolerance, Concurency and Hot-Code swapping

Ok, i must make a confession : I lie when i saying that i met Erlang by accident, similar to those one year old little girl story - which was real story if you curious. I'm actually select it based by my research in search of the ideal platform for designing real-time application.

When we think about a real-time application, lets say Whats App, the system ideally **always live** ,and, in term of Whats App - it should handle millions active connections in matter of minutes, or even seconds. [While in 2011 they can handle 1 million active TCP connection in a single box, in 2012 they go even further to 2 millions](http://blog.whatsapp.com/196/1-million-is-so-2011). From technical perspective, thats crazy. 

## And the journey still goes on...

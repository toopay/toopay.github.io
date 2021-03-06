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

## "Concurency", Fault tolerance and Hot-Code swapping

Ok, i must make a confession : I lie when i saying that i met Erlang by accident, similar to those one year old little girl story - which was real story if you curious. I'm actually select it based by my research in search of the ideal platform for designing real-time application.

When we think about a real-time application, lets say Whats App, the system ideally **always live**, and, in term of Whats App with its 500 million users - it should handle millions active connections in matter of minutes, or even seconds. [While in 2011 they can handle 1 million active TCP connection in a single box, in 2012 they go even further to 2 millions](http://blog.whatsapp.com/196/1-million-is-so-2011). They haven't release further improvements news for 2013 and 2014 within their blog, but the 2012 data alone was more than enough to stand as testament to their decent system architecture. 

How you can spawning 2 million process within one single box? What happens if one process crashes, does the other 1.999.999 processes remains operated? Also how do you handle the deployment process? Should you display "we're temporarily down" on millions connected devices? From technical perspective, thats crazy and daunting. 

Prior met Erlang, i would be nervous as hell if i must to take above responsibilities. 

> If you want to keep a secret, tell it to a Swede. Born in Stockholm over 20 years ago, Erlang is the most advanced open-source server platform in 
existence, but it seems almost no one knows about it. Erlang can handle 
hundreds of thousands of simultaneous connections; it can spawn 
millions of simultaneous processes in under a second; server code can be 
upgraded, in production, without any interruption of service; and errors 
are handled in such a way that server crashes are extremely uncommon. -- [Evan Miller](http://www.evanmiller.org/), 

Above "promises" seems looks like much how Whats App system operates. But off-course, this should be tested, i said to myself one month ago. It could be just those random marketing gimmick. Today, all of my assertions return true for Erlang **fault tolerance**, **high availability** and **concurency** as its native platform characteristic. 

This is all down to how Erlang's concurrency works. It was based on message passing and the actor model. Think of people communicating with nothing but letters. In this section i want to introduce the three primitives required for concurrency in Erlang: spawning new processes, sending messages, and receiving messages.

So lets start with a simple module that will spawn a new process and listen for any received messages.

{% gist b925e97a24b8099b979b %}

Now we can start sending the message.

![Hello-2]({{ site.baseurl }}/images/hello-2.png)

Whats we just did were basically 2 things. First, we use Erlang `spawn/3` to spawn above module function. Second we passing a message through those ***actor***. 

The result of `spawn/3` above, `<0.41.0>`, is called process identifier, often just written `Pid` by the community. Each thing in Erlang (seriously) is simply a process. Even the erlang shell itself is a process! So when we send `{self(), <args>}` to the `pint/0` function from within Erlang shell, we basically just sent its process identifier (which get echoed back to its mailbox by our `pint/0` function).

![Hello-3]({{ site.baseurl }}/images/hello-3.png)

When we `flush/0` the IO buffer, we can see that previous received messages are correctly processing by our module.

From this very simple demonstration, we can actually grasp the underlying reason of why Erlang can scale better than any other platform : because users would be represented as processes which only reacted upon certain events (i.e.: receiving a call, hanging up, etc.). An ideal system would support processes doing small computations,  and too make it efficient, its plausible for processes to be started very quickly, to be destroyed very quickly and to be able to switch them really fast. How fast?

Lets put more challange to this Swede. Here we try to spawn 10 processes that will echoed any integer argument that passed to it. The argument ranging from 1 to 10, as showed bellow : 

{% gist 7ec64c0d49dca750aca5 %}

And the execution generates interesting output :

![Hello-4]({{ site.baseurl }}/images/hello-4.png)

First, it not showing us the sequential result. The order of the output doesn't make sense, you said. Welcome to parallelism. Because the processes are running at the same time, the ordering of events isn't guaranteed anymore. That's because the Erlang VM uses many tricks to decide when to run a process or another one, making sure each gets a good share of time. Erlang teach me that [lock](http://en.wikipedia.org/wiki/Lock_%28computer_science%29) isn't good for real-time system, and our design should avoid it in any costs.

Secondly it shows us that it took only 45 milliseconds to spawn all of those 10 process. If 10 processes sounds too small for you, i'm also felt the same way. So lets get crazy and try to spawn a million processes! I supress the IO output that echoing the integer values, and heres the result :

![Hello-5]({{ site.baseurl }}/images/hello-5.png)

It was only took roughly 5 seconds for Erlang to both spawning each of those 1 million processes and at the same time, destroying their context each time the function finished its job. Try it on your own, and the result should be roughly same. And if you're monitoring your system via `top` command (on Unix) when you do last code execution, you'll also get another interesting notion :

![Hello-6]({{ site.baseurl }}/images/hello-6.png)

Its show that Erlang uses `222,2%` of our processor. For no-brainer like me, thats a shocking result. How it could supersede `100%` of my available processor? Thats doesn't make sense (i hear you). Well, off-course except that we're apparently completely forgot that we had quad-core processor nowaday. Welcome to the multi-core programming!

I also found some history from this lesson. Many programmers hold the belief that Erlang was ready for multi-core computers years before it actually was. Erlang was only adapted to true [symmetric multiprocessing](http://en.wikipedia.org/wiki/Symmetric_multiprocessing) (SMP) in the mid 2000s. Before that, SMP often had to be disabled to avoid performance losses. To get parallelism on a multicore computer without SMP, you'd start many instances of the VM instead. Erlang was only got most of the implementation right with the R13B release of the language in 2009.  An interesting fact is that because Erlang concurrency is all about isolated processes, it took no conceptual change at the language level to bring true parallelism to the language. All the changes were transparently done in the VM, away from the eyes of the programmers.

Now we know, at its heart, how concurency works on Erlang. But our assertions is not complete yet until we also sure that we can handle any failure within our concurent process, and be able to upgrade our code without any interuption, right?

The first writers of Erlang always kept in mind that failure is common. You can try to prevent bugs all you want, but most of the time some of them will still happen. In the eventuality bugs don't happen, nothing can stop hardware failures all the time. The idea is thus to find good ways to handle errors and problems rather than trying to prevent them all.

Back to our three primitive operations above: spawning, receive and sending a message from within a process, Erlang also allow us to **linking** and **monitoring** each process. [Together](http://www.erlang.org/doc/reference_manual/processes.html), and with **naming processes** register, we could than assemble a process **supervisor**. In higher level perspective, Erlang supervisor is like Pharaoh while our millions processes were his worker, that works hard building the pyramids and mega tombs. When a worker (process) do not behave like he should, Pharaoh will hang it in public (to avoid more missbehave worker), and replace it with similar worker. Thats how cruel Egypt golder empire were once live in mankind history.

We're only have one more question left. Is it possible to introduce patch to running system, without interupting the whole service? An Erlang **release** is a set of OTP **applications** or **libraries** deployed and running together. To become a no-downtime release, we should be able to swap between two releases, the current running release and one prepared and loaded, a f**kin' [Hot Code Swapping](http://en.wikipedia.org/wiki/Hot_swapping). 

For this tasks, Erlang provides both [release_handler](http://www.erlang.org/doc/man/release_handler.html) and [systools](http://www.erlang.org/doc/man/systools.html). They are peculiar, and lil bit hard to use. Fortunately for me (and any other Erlang newcomer), there is [relx](https://github.com/erlware/relx) that does away with all of those tedious work. Performing hot code swapping are a matter of (1) preparing an OTP that listen for upgrade event, (2) bumping up the version and make new release via `relx release relup tar`, then (3) calling `upgrade <new_version>` from our previous release binary. A piece of cake.

I dont have last words to close this section, but there is a video that you probably interesting to watch. Its actually conclude both this section and previous one, all in a nice parody.

<iframe width="500" height="400" src="http://www.youtube.com/embed/rRbY3TMUcgQ?color=white&theme=light"></iframe>

## And the journey still goes on...

Right now, this second, while we take a single breath, many things actually also happens. The Venus is completing 7/8 from its full orbit motion. There is supernova in our near galaxy. There is a new planet, somewhere. Coldplay prepare to release their new album. Some random people just throwed up from drinking bad vodka. And so on. Think about that, i know you probably don't care right now, but any sane people that still have curiosity (well, not limited to but including You : any self-proclaimed software engineer) in their mind, should also questioning the same.

Within a month i realize that i often underestimate how things works, not excluded (especially) in programming. Erlang give me a new insight, an interesting one, about how things works. I even playing lil bit with [some PRNG approach](http://www.slideshare.net/jj1bdx/london-erlang-user-group-talk-slide), and realize how other language that i ever used, was simplify or taking very small consideration on this field, which eventually can lead to serious instability on running system. While i'm started grasp the Erlang concept, i know i'll deal with those [CAP Theorema](http://en.wikipedia.org/wiki/CAP_theorem) when i exploring more about develop distributed system in Erlang.

So for 2015, I will enjoy spending this whole year diving more deep to Erlang. As i digest more Erlang books or articles, I will try to share my experiments in this blog, probably roll out some new oper-source libraries along the way. I'll continue to building my OTP application that utilize awesome community libraries, just to mention a few : [ejabberd](https://github.com/processone/ejabberd), [ranch](https://github.com/ninenines/ranch) and [riak](https://github.com/basho/riak). I know when i get stuck, and [throw a question to the community](http://stackoverflow.com/questions/27229185/riak-data-types-and-search), the community will gladly help. FYI, they are surprisingly very warm and cheerful, also if i must added here : had a good sense of humor. If a community around your language are nothing but either language elitist nor those people who spend most of their time talking about framework/language war, you probably want to try Erlang and mingle with their community. Its not so large, but for sure its the most fine i found so far. And if you do so, you off course will found me, with the smile of a curious little one year old girl bringing her new amazing toy everywhere she go.

***

References :

* http://www.erlang.org/doc/
* http://en.wikipedia.org/
* http://learnyousomeerlang.com/
* http://ninenines.eu/articles/
* http://www.chicagoboss.org/tutorial.pdf

---
title: Idiomatic Code
description: What it is and why it matters.
date: '2020-07-28T00:02:25.283Z'
categories: []
keywords: []
slug: /@drawson/idiomatic-code-a73f17f0f287
---

> You can call it “beautiful code” when the code also makes it look like the language was made for the problem - Ward Cunningham

If you have a sound continuous integration practice, your pull requests will be automatically evaluated for test coverage and compliance with code conventions. But it’s your teammates who will evaluate the degree to which the submitted code is high quality through being _idiomatic_. What does this mean?

![](/Users/davidrawson/AndroidStudioProjects/medium2md/medium/posts/md_1788081470726/img/1____BAzB5eaH2Ugnj7VbzHYmA.jpeg)

#### What does “idiomatic” mean in general?

If we have DuckDuckGo installed, we can pretend our web searches are terminal scripts:

![](/images/0__6vP1WpgxRo1amEL4.jpg)

`!wikt idiomatic` will take us to the Wiktionary page for “idiomatic”, revealing the following definition:

> Pertaining or conforming to the natural mode of expression of a language.

A few links further down is the origin, the Greek “ἴδιος” meaning “private, peculiar, specific, appropriate.”

So, in software engineering “idiomatic” means “conforming to the natural mode of expression in a given context”, where that context can be a programming language, like Kotlin or JS, or even a framework like React or RxJava.

But like many simple definitions, this can be taken to a harmful extreme. So let’s define some boundaries to prevent misuse.

#### “Idiomatic” is not a synonym of “fashionable”

Oscar Wilde may have been talking about artistic fashion when he said:

> And, after all, what is a fashion? From the artistic point of view, it is usually a form of ugliness so intolerable that we have to alter it every six months

But he may as well have been talking about frameworks:

We might arrive at an incorrect conclusion if we assume “the natural mode of expression” in our definition of idiomatic means “the most popular and current form of expression.” We might then go on to say our code is unidiomatic because we are not using the framework-du-jour.

This is problematic — if we must dismiss code that fails to conform to the latest trend as low quality, we introduce waste as we thrash backwards and forwards when the winds of fashion change. To use another metaphor, a codebase where these changes happen frequently can resemble the [lava layers](https://mikehadlow.blogspot.com/2014/12/the-lava-layer-anti-pattern.html) left behind by volcanic eruptions.

While fashion can and should _influence_ our understanding of the idiomatic, it shouldn’t _determine_ it. In other words, “idiomatic” doesn’t mean “being an [edgelord](https://www.merriam-webster.com/words-at-play/what-is-an-edgelord-slang-definition-words-were-watching).”

#### Idiomatic is not bike-shedding

There should be one-- and preferably only one --obvious way to do it.

The [Zen of Python](https://www.python.org/dev/peps/pep-0020/) insists there is one good way to do things, but in some languages the cup runneth over with multiple possible approaches to a particular problem. One might imagine that deeply exploring these possibilities in a pull request is required to achieve idiomatic code.

Of course we want to improve the code, but continually proposing alternative approaches that have no advantages over the original is simply wasteful churn and degenerates into [bike-shedding](https://en.wikipedia.org/wiki/Law_of_triviality) — pointless arguments about trivialities. In other words, continually insisting “_I_ would have done it _this_ way” is not a path to the idiomatic.

![](/images/1__zYhcFTMqGz6HT1pKK__asnA.jpeg)

#### Idiomatic is not just following the docs

One then may think that it is enough to follow closely whatever docs are provided for a language or framework to know what is idiomatic. However, this fails to capture the situation where documentation has diverged from best practice for that context.

This situation is surprisingly common. Often at the time the tools are first written, our understanding is less mature. A community of best practice evolves slowly. For instance, the classes and documentation authored in the early days of Java do not reflect later robust Java practice as reflected in Joshua Bloch’s brilliant [Effective Java](https://read.amazon.com.au/kp/embed?asin=B078H61SCH&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_.Va.EbSR9S306).

![](/images/0__mClOagKQdfeUj4TL.jpg)

Famously, the early `java.util.Date` class contains mutator methods for the year, day, and hour. Thus it contradicts Item 15: Minimize mutability. The later `ZonedDateTime` corrects this mistake. In this case, even though `java.util.Date` is part of the Java API, imitating this class in hopes of obtaining idiomatic Java is wrong.

### What then is idiomatic?

Therefore, the idiomatic lies on a knife-edge: respecting a modern community of practice, but not unduly influenced by its fashions. Respecting the docs, but respecting them enough to discard them when they contradict proven engineering practices. Exploring the possibilities of the language, but not exhausting oneself in debates like: “you can also do it this way in Kotlin…” It is the job of the tech lead to navigate these rocks and shoals.

![](/images/1__bBTYjR9agARof5kEWEbdKQ.png)

### Why?

If idiomatic code is so difficult to achieve then why even bother?

#### Hiring

If your project respects a well-established idiom, you will have an easier task bringing in new friends to help your endeavour. Experienced developers will have done their time in engineering [cargo-cults](https://en.wikipedia.org/wiki/Cargo_cult) and will be vigilantly avoiding these. Similarly, those who appreciate nuance and consistency will want to avoid the endless churn of a latest-framework hype shop. If you can demonstrate in your interactions with prospective employees that you have found a middle way between stale old practices and fashion churn, you will attract talent.

#### Onboarding

A project that conforms to an idiom will be easier for a new hire to understand. They will find the right things in the right places, and their early contributions will fit better.

#### Technical debt

Unidiomatic code begets unidiomatic code. Ignoring the framework idiom, you warp the supplied components into your own unusual structure. Because you’ve departed from the intention of the framework, you will have to add more and more workarounds as time passes. These workarounds will attract bugs and, yes, technical debt.

#### Good citizenship

If you follow the idiom, external dependencies will match your code and will be easy to integrate. Conversely, parts of your own code can be open-sourced with little modification since they match the expectations of a community of practice.

![captionless image](/images/1__KYr19bfhpWG0pvpicRwSlg.jpeg)

### How?

We’ve already established that developer docs are not enough to obtain the idiomatic. And we all know that random Medium articles can be a mixed bunch. What then is needed?

#### Stack Overflow

Stack Overflow can be a great source for the idiomatic when we are learning a new framework. But not in our normal way of use. Just Googling when we get stuck and relying on Stack Overflow solutions is a dangerous path, since we learn the bare minimum to deliver our story without understanding the idiomatic.

A better strategy can be to [follow a Stack Overflow tag](https://stackoverflow.com/help/interesting-topics), and watch how different answers appear and attract votes.

![](/images/0__HlUlKNElO5sRG__Gq.png)

You can even find some framework authors answering questions on their work. Here is an answer to a question about RxJava by library author David Karnok:

![img.png](/images/stackoverflow.png)

https://stackoverflow.com/a/40326875/5241933


When you get more confident, you can attempt your own answers to questions on the tag. In this way, you are submitting your own understanding to review by your peers. This is a great way to check your understanding.

#### Books

Yes, good old-fashioned ink on paper can help here. Unlike so many articles on the internet, books from reputable publishing companies are vetted and edited heavily. If you don’t know where to start, a book can be an excellent guide. It’s often worth shelling out for the book and working through it slowly, rather than rushing in and writing code that does not conform to the idiom.

#### Fulfilling the need for learning

While finding a balance may mean that you don’t use the latest framework, engineers may become frustrated that they are missing out on experience with a technology that will be important for their careers in the future. In this case, it’s important to give an outlet for experimentation. Maintaining 20% time for personal projects is not only good for speed, since 100% utilisation is wasteful, but it’s also a chance for engineers to try the latest toys to keep their skills up-to-date. Alternatively, low-risk, modularised, isolated parts of the codebase may be suitable for experiments with new technologies without departing too far from the idiomatic core.

#### Give it time

Unfortunately we are most likely to write unidiomatic code at the beginning of a project with a new framework. This is the “danger zone”: where we’re a competent enough programmer to deliver features, but not familiar enough with the framework to write idiomatic code. This should be kept in mind when framework switches are considered: not only will there be a gap in delivery as developers learn the new framework, but there will also be a gap in quality as the early work does not conform with the idiomatic. This early code may need to be reworked when we later discover the idiom has been misunderstood. Better sooner than later, before it grows legs and takes the project down a path that no-one can follow…

### Conclusion

One final doubt, “idiomatic” sounds a bit like another word we use for people we don’t like. Yes, it’s cognate with “idiot” from the Greek _idiṓtēs_ meaning “private citizen” or “layman”_._ So be idio-matic in the sense that your code is private, or peculiar, to a community of practice. But don’t be an “idio-t”: don’t write your own peculiar, private code that no-one outside will understand!

Originally published Jul 28, 2020 on medium.com
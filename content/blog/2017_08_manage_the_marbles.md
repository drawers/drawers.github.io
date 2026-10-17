+++
date = '2017-08-08T20:52:15+12:00'
draft = false
title = 'Manage the Marbles with Dagger Producers'
+++

**_TL;DR_** _in composing pipelines of asynchronous tasks we encounter the same kinds of problems as in composing graphs of dependencies._

_The dependency injection framework Dagger 2 can manage both types of composition, leading to increased modularity._

![captionless image](https://miro.medium.com/v2/resize:fit:1214/format:webp/1*SOyy6nf8qIls-4TaLmNI9g.jpeg)

Assume we have an app where the user enters in a player name and the app displays the last 20 games played by that player.

![A simple app](https://miro.medium.com/v2/resize:fit:732/format:webp/1*PlwixgJRhbmMW8QIvf3R3w.png)

The business logic for this involves two requests to our API:

*   a web request to obtain the id of the player from the name
*   a second web request to obtain the games played by that id

![Two requests in the business logic](https://miro.medium.com/v2/resize:fit:278/format:webp/1*nWaVSz_ThVx0JgHgNff80Q.png)

If we were using Dagger 2 to manage the dependencies in our app, the object graph might look something like this:

![captionless image](https://miro.medium.com/v2/resize:fit:756/format:webp/1*ir2hQs6CMZwGu0dgvqC_Tg.png)

Fair enough. Assume we are using MVP. The presenter for the app might contain some code that looked like this if we were using RxJava:

We’ll translate that to Google Guava just for now:

The `transformAsync` here performs the same function as the `flatMap` in the RxJava example — it means “when you’ve finished retrieving the player, retrieve the games using the id of that player”.

Assume we are using MVP and our presenter will show the results of the request to our view. Our presenter has a dependency on both `PlayerRepository` and `GamesRepository`.

{{< gist drawers a48f31cf6071765ae2c4c3ebd90a0c1d >}}

This is no longer ideal since to test the class we would need to mock both `PlayerRepository` and `GamesRepository`.

If we wanted to tidy up the presenter we could extract a `MainRequestHelper` which exposes the final result for both requests, the `ListenableFuture<Games>`:

{{< gist drawers 4b7685032bb8baf17365b14ed9347530 >}}

Now we’ve reduced the number of the dependencies for our presenter at the cost of maintenance of a helper class:

{{< gist drawers becdb76d90232321eea78c5365aaae66 >}}

It feels good to consolidate all the requests into one service for the presenter to call. We’ve also learned that the “graph” of async tasks (retrievePlayer -> retrieveGames) is inextricably related to the object graph; it’s the DI framework that allows us to easily move dependencies from the presenter to the helper class.

At the same time, I’m ambivalent about the helper class — it’s taking up space. Taking up space, perhaps, in the same way a handwritten `Factory` or `ServiceLocator` would were we not using a DI framework.

This naturally leads us to ponder whether we could manage the graph of async tasks (production graph?) in the same way we manage the object graph for dependency injection.

In RxJava marble diagrams, the marble represents a single `Observable`. Often this is simply the single result of an asynchronous task (like a web request) and we are composing these in a very simple manner using `flatMap` or `map`. What if there was a way to manage the marbles in the same way we manage the dependencies for our app?

![The marble diagram for flatMap (courtesy http://reactivex.io/documentation/operators/flatmap.html)](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*HnjrvlaOGvVRmSs1uSUiqA.png)

Enter [Dagger Producers](https://google.github.io/dagger/producers).

Just as we can model an object graph with the `@Binds`and `@Provides`methods in a Dagger 2 module, with Dagger Producers we can model a production graph with a `@ProducerModule`.

In the below example we model the pipeline: a future`Player` is produced from a request to the and a future `Games` is produced from, having already obtained a `Player`, requesting their `Games` from the `GamesRespository` with their id.

We can now write a `Component` (injector) to expose the future `Games` we want:

and call the `Component` inside our presenter:

While this may seem like a lot of boilerplate for achieving very little, imagine a wild requirement change appears and a processed `Statistics` must be shown instead of the list of `Games`.

![captionless image](https://miro.medium.com/v2/resize:fit:304/format:webp/1*nazZ673BSiLiowRfCjdAAQ.png)

If we were updating the old `HelperClass`, we’d need to change the dependencies and the signature:

If instead were using Dagger Producers, we could simply add a new `@Produces` method to our module:

And we can expose the `Future` representing the processed `Statistics` in the `Component`:

Finally, the presenter can access the future `Statistics` through the `Component`:

If you’re excited by this kind of solution, bear in mind that bear in mind that Google Guava is a heavyweight library and if you have method counts or .jar sizes to worry about, you may have to [use ProGuard aggressively](https://github.com/google/guava/wiki/UsingProGuardWithGuava).

For those working on an existing project using Guava and Dagger 2, the new [FluentFuture](https://google.github.io/guava/releases/snapshot/api/docs/com/google/common/util/concurrent/FluentFuture.html) API and Dagger Producers represent an exciting new frontier for code generation and DI.

Originally published on [Medium](https://drawson.medium.com/manage-the-marbles-with-dagger-producers-850e8067d050) on Aug 8, 2017

Milkman
========

*Routing framework*

Milkman manages the application route. Through it, you may navigate the application, and have your application's components respond to route changes. The route may be represented by *URL hash*, *pushstate*, or an internal static global. Milkman is built on top of [troop](https://github.com/danstocker/troop) for OOP, [sntls](https://github.com/danstocker/sntls) for data structures, and [evan](https://github.com/danstocker/evan) for eventing.

[Wiki](https://github.com/danstocker/milkman/wiki)

[Reference](http://danstocker.github.io/milkman/)

[Npm package](https://www.npmjs.com/package/milkman)

Getting started
---------------

### Configuration

Right after milkman has been loaded (required), you may specify whether you want to use hash or pushstate-based routing, by setting the flag `milkman.usePushState`. The flag has no effect under node.

    milkman.usePushState = true;

### Navigation

Navigating to a route is quite simple. Convert a route string to a `Route` instance, which offers an API that includes navigation.

- Synchronous navigation changes the route immediately. By the time execution comes to the next line, the route will have changed. Example: `'user/joe'.toRoute().navigateTo()`.
- Asynchronous navigation lets the current thread finish before the route will change. Async navigation returns a `Q` promise, giving you the opportunity to specify some follow-up. Example: `'user/joe'.toRoute().navigateToAsync()`.
- Debounced navigation allows you to skip routes would they be overridden by subsequent navigation commands within a specified time frame (100ms by default). This is useful when re-routing due to some condition not being met by the application's state. Example: `'user/joe'.toRoute().navigateToDebounced()`.

Alternatively, if you don't want the route to be reflected in the URL (hash or state), silent navigation is also possible.

    'user/joe'.toRoute().navigateToSilent();

### Responding to routing events

Milkman triggers routing events in a special event space, whenever the application route changes, either through the hash, or pushstate. To capture such events, eg. for all 'user' routes, use the following expression known from *evan*.

    'user'.toRoute().subscribeTo(milkman.Router.EVENT_ROUTE_CHANGE, function (event) {
        console.log(event.beforeRoute.toString());
        console.log(event.afterRoute.toString());
    });

To listen to all routing events, you'll need to subscribe to en empty route.

    [].toRoute().subscribeTo(...);

Sample code
-----------

To see milkman in action, check out [Hills](https://github.com/danstocker/hills).

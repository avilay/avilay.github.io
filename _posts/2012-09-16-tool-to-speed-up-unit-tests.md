---
layout: post
title: Tool to Speed Up Unit Tests
date: '2012-09-16 03:11:00'
---

### Why I don't write unit tests

Unit tests have saved me countless number of times. I know the benefits of TDD. Yet there are times when I do not write unit tests because _ **writing unit tests takes time** _. And my good senses have a tendency to desert me whenever I am racing against a deadline. So I cut corners, leave out unit tests, and of course, pay for it later in production. This got me thinking - which parts of authoring UTs are the most time consuming? And can I do something about it? Turns out _ **writing mock objects is one of the most time consuming parts** _ of authoring UTs. And yes, there is something I (and you) can do about it.

### Common unit test pattern

Lets say you have a simple 3-tier architecture that is most common for web applications.

![figure1](/assets/imgs/tool-to-speed-up-unit-tests/figure1.jpg)

Now, I want to write a unit test for the SnackController. I would end up writing 2 pieces of code - the actual unit tests for the SnackController and a mock CookieService.

<figure class="kg-card kg-image-card"><img src="/content/images/2020/03/figure3.jpg" class="kg-image"></figure>
![figure3](/assets/imgs/tool-to-speed-up-unit-tests/figure3.jpg)

### Why mocks?

1. I want to test how SnackController behaves when CookieService throws unexpected errors and faults. In other words, I want to inject faults in CookieService.
2. If CookieService is a remote process, usually running on a different server, I don't want my SnackController unit tests to take a dependency on network I/O or RPC. I want my unit tests to be "self contained" and fast.
3. I don’t want bugs in CookieService to affect the testing of SnackController.

Reason #1 is the strongest reason why I write mock objects. For reason #2 - I am ok with making calls over the wire in my unit tests. Sure, they will fail occasionally due to network flakiness or other reasons outside the control of my test environment. But if it can happen in a test environment, it will surely happen in production! So I better have the right recovery actions in place in SnackController. And as long as I run CookieService unit tests before running SnackController unit tests, I am reasonably assured that reason #3 will not be an issue.

### Proxy instead of mock

Now, instead of writing a mock service for the purpose of injecting faults, wouldn’t it be cool if I were to write some sort of a proxy object with delegates (function pointers for the old schoolers amongst us :-)) that are wired up to methods in the real CookieService? That way I could re-wire any delegate in the proxy to a faulty method at runtime and inject fault in the SnackController calls.

![figure22](/assets/imgs/tool-to-speed-up-unit-tests/figure22.jpg)

### Auto-generated proxy

Sounds cool. Now I can switch between faulty and actual behavior at runtime. But it does not solve my original problem of saving time. I ended up writing a proxy service instead of a mock service - spending just as much time as before. What I really need is a proxy factory that can create - at runtime - the above proxy object, with the delegates and all. Turns out with a bit of IL magic, I can do just that. And I did :-) With two lines of code, I can create a proxy object for any given object, implementing any given interface.

`ProxyFactory pf = new ProxyFactory(false);`  
`ICookieService proxy = pf.Create(realCookieSvc);`  
  
By default, the proxy object implements ICookieService and all its methods are wired up to the corresponding methods in the realCookieSvc. ProxyFactory is the class that I wrote that has all the IL magic in it.

ProxyFactory has a ChangeBehavior method that can rewire any method in the proxy to a user supplied method. So in this case, I would implement a faulty method ask ProxyFactory to rewire one of the good methods in the proxy to the new faulty method. All of this happens at runtime.

You can find the code and this example (in much more detail) implemented in the [dynamic\_proxy project on github](https://github.com/avilay/dynamic_proxy). This is under the [creative commons license](http://creativecommons.org/licenses/by/3.0/), so do as you please with it :-).


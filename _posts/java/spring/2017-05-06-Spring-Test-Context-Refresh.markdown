---
layout: post
title:  "Spring Refresh Test Context"
date:   2017-05-06 00:00:00 +0200
categories:  [spring, java, test]
---

When creating tests. Unit tests should be completely independent of each other.
But what to do when you step one step up in the pyramid of testing you come to integration tests.
Those of course can depend on each other. But should only depend on each other in one use case so each use case should be isolated.

At the point when data comes into the game there are aDi few pitfalls. When using Spring Framework there seems to be a solution. There can be new Spring context created/reset at method level or class level. 

Therefor there is only one Annotation necessary:

{% highlight java %}
@DirtiesContext(classmode=BeforeClass)
{% endhighlight %}

There are the following configuration options available:
Supported Test Phases
- Classlevel Annotation at Test class
  - Before current test class: when declared at the class level with class mode set to `BEFORE_CLASS` 
  - Before each test method in current test class: when declared at the class level with class mode set to `BEFORE_EACH_TEST_METHOD`
  - After each test method in current test class: when declared at the class level with class mode set to `AFTER_EACH_TEST_METHOD`
  - After current test class: when declared at the class level with class mode set to `AFTER_CLASS`
- Methodlevel Annotation at Test case
  - Before current test method: when declared at the method level with method mode set to `BEFORE_METHOD`
  - After current test method: when declared at the method level with method mode set to `AFTER_METHOD`
[extracted from Spring documentation](http://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/test/annotation/DirtiesContext.html)

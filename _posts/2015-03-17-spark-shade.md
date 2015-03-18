---
layout: post
title: "Shading in spark"
description: ""
category: 
tags: []
---

Understanding maven shading functionality.

Apache maven shade plugin is used to include/execlude dependencies in order to make a shaded executable jar by renaming packages of classes and resolving conflicts.

Sometimes what happens that we have mutliple versions of different artifacts transitively in project build and while packaging the application in a single distribution jar we realize difficulties. Hence maven shade plugin help us to rename the packages of artifacts and packaging it.

Below is a scenario where we can use it.

A Foo library, which depends on a specific version (e.g. 1.0) of Bar library. Assuming I cannot make use of other version of Bar lib (because API change, or other technical issues, etc). If I simply declare Bar:1.0 as Foo s dependency in Maven, it is possible to fall into a problem: A Qux project is depending on Foo, and also Bar:2.0 (and it cannot use Bar:1.0 because Qux needs to use new feature in Bar:2.0). Then it is a dilemma: should I use Bar:1.0 (which Qux s code will not work) or Bar:2.0 (which Foo s code will not work).

In order to solve this problem, developer of Foo can choose to use shade plugin to rename its usage of Bar, so that all classes in Bar:1.0 jar is now embedded in Foo jar, and the package of the embedded Bar classes is changed from com.bar to com.foo.bar. By doing so, Qux can safely depends on Bar:2.0 because now Foo is no longer depending on Bar, and it is using is own copy of "altered" Bar located in another package.

There are many remarkable places where shading is used in spark.

<b>>First of all Anything that is shaded must be promoted to "compile"<b>

<b>[SPARK-4809] Rework Guava library shading.</b>

Spark-core uses guava library internally. Thus any code which depends on spark-core does not see any transitive dependencies on which spark-core classes depends on.

Thus user who is using a different version of guava and spark-core classes will face conflicts while packaging application in a single distribution jar.

"Guava is shaded so in spark-core that it's applied uniformly across the Spark build. This means Guava is shaded inside spark-core itself, so that the dependency issues above are solved. Aside from that, all Spark sub-modules have their Guava references relocated, so that they refer to the relocated classes now packaged inside spark-core. Before, this was only done by the time the assembly was built, so project that did not end up inside the assembly (such as streaming backends) could still reference the original location of Guava classes).

This relocation does not apply to the sub-modules under network/, though. For those cases, we want to keep the Guava dependency alive, since we want to use the same Guava as the rest of the Yarn NM when deploying the auxiliary shuffle service. For this reason, also,
the network/ dependencies are shaded into the spark-core artifact too, so that the raw Guava dependency doesn't leak."

This makes all Spark-generated artifacts use the Guava defined in
the build file at runtime too. The leaked classes ("Optional" and
friends) are now exposed in a few places where they weren't before,
but hopefully that won't cause issues.

<b> [SPARK-3996]Shade Jetty in Spark deliverables </b>

If somebody wants to use Spark in a Jetty 9 server, then it's causing a version conflict. Given that Spark's dependency on Jetty is light, it'd be a good idea to shade this dependency.





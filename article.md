#

The purpose of this article is to provide guidance to developers using Spring on the effective use of Spring profiles,
and on how to conveniently manage local Spring configuration while observing 12 factor principles.
We will assume that the reader has an understanding of how and why to externalize configuration in Spring.
If you would like some additional guidance on that topic,
[this](https://spring.io/blog/2015/01/13/configuring-it-all-out-or-12-factor-app-style-configuration-with-spring)
is a high quality source.

While it's great to have configuration externalized,
I've found that it's not always obvious how to gracefully set up local configuration when using this strategy.
Many people default to using an `application-local.yml` file or something similar,
but this pattern is problematic.
Superficially, it violsates the concept of externalized configuration principles by bundling a config file directly into the jar file.
I'd like to dive briefly into why this is a real problem, rather than merely a philosophical one.
I'll assume that we can all agree that configuration strategies should match across dev, prod, and any other environments.
This helps minimize risk when promoting an application between environments.
Local configuration strategy should match as closely as possible with how the app is configured elsewhere,
to minimize the likelihood of 'it worked on my machine' situations.
It's impractical to bundle configuration files with an application.
Doing so means that any configuration change is actually a build and deployment of the application,
since the configuration must first be updated in the source,
followed by the creation and deployment of a new binary with the updated configuration values.
In addition to making it potentially difficult or slow executte configuration updates,
this muddies the water between what a 'code change' or 'configuration change' really are.
Finally, this necessitates baking secrets into the binary, which is very risky.
The secrets could be encrypted, which would be better than storing them in plain test,
but then a key must somehow be dynamically provided to the application to allow for decryption.
At that point, something already has to be externalized.
Why not just make things easier on everyone and fully externalize all environment configuration?

Assuming you're convinced that externalizing config is a good idea,
and that matching local strategy with deployed strategy is valueble, lets talk about how to get there.
There are three primary paths towards clean variable externalization in Spring.
The first is to use Java properties, the second is to use environment variables,
and the third is use a Spring configuration server.
We will try to cover setting up local configuration for each.

When using Java properties, it's fairly simple to use  the same strategy locally.
I typically include a .properties or .yml file appropriate for testing or running locally in my command.

When using environment variables to configure the application in production,
I like to assign environment variables using gradle or maven.

When using a Spring Configuration server,
I believe that is best to use such a server locally as well.



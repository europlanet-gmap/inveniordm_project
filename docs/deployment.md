# How to deploy InvenioRDM (into production)?

The more general question is: how to deploy an web-app (into production)?

The question comes from the fact that, when an (web) app goes into production,
we have to care about
(i) logging;
(ii) security,
Fundamentally.
Depending on the app -- InvenioRDM, for instance -- we must also care about
(iii) data persistence
in the long run.
And very important for the success of your app and the maintainers peace of mind are
(iv) stability,
(v) monitoring.

> **Deployment environment**:
> There are different options of environments to deploy a software:
> (Docker) containers, (virtual) machines and all the management frameworks
> orchestrating the components add a lot of noise to the discussion.
> My primary choice here is to have the app (InvenioRDM and dependencies)
> running from Docker containers; But I should also discuss how it goes inside
> a good-old GNU/Linux (OS, virtual) machine.

**Logging**
To be able to inspect what is actually happening to your app, especially in the
case of error, is fundamental to any fix or improvement we may have/want to do.

**Security**
I'm no expert on security whatsoever therefore I am not going to enumerate all
the cornerstones of the topic, but what matters to me: SSL and user authentication.
And it is not that _want_ but rather that I _need_ to have SSL enabled in the
apps realm for we are using OAuth2 protocol to authenticate our users.

And if somewhere in the tubes there is some non-SSL traffic, OAuth will complain:
```
oauthlib.oauth2.rfc6749.errors.InsecureTransportError:
(insecure_transport) OAuth 2 MUST utilize https.
```

**Data persistence**
When we go into production the data being managed by our app should exist
independent of the app's status. Although that seems obvious, it is an
aspect that changes from a testing to a production environment. And it is
certainly an important item of the environment setup when (docker) containers
are being used as those are ephemerous by definition.

**Stability**
We assume here that the app software app is stable enough to go into production;
so we are not talking about the obvious stability discussion during dev/alpha/beta
phases of features development.
Our _stability_ concern here is on the matters of system resources, tolerance
for requests overload, and, in the case the app/service goes down, some sort
of auto-recovery or complete shutdown.
Bottom-line is: we don't want a mal-functioning service (app), we either want it
working fine or not working at all.

**Monitoring**
To be able to monitor a resource is at least comfortable; Mandatory in high-demand
services, optional in small-scale scenarios.
Monitoring tools allow us to check health-status of our app, as well as other
resources necessary to the app's well functioning (e.g, disk space, cpu load).

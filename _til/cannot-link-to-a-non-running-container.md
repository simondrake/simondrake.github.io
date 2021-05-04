---
layout: post
title: Cannot link to a non running container
subtitle: How to fix this docker error
date: 2021-05-04
tags: [DevOps, docker]
---

When starting/restarting a docker container, you may get an error similar to the below:

```
ERROR: for <service>  Cannot link to a non running container: /b2f21b869ccc_<dependency>_1 AS /<service>_1/<dependency>_1
```

This usually happens when a dependency container has a different ID than the one given (`b2f21b869ccc` in the above example). To resolve this issue, force recreate the service, dependency and fix the link to the correct docker ID.

<pre class="command-line"><code class="language-bash">
docker-compose up -d --force-recreate {service}
</code></pre>

To force recreate all containers, omit the service:

<pre class="command-line"><code class="language-bash">
docker-compose up -d --force-recreate
</code></pre>

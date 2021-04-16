---
layout: post
title: Setting a timeout in Jenkins
subtitle: Use groovy to set a timeout in a pipeline
date: 2021-04-15
tags: [jenkins, groovy]
---

In the `pipeline` step, choose `Pipeline script` and use the following to set a timeout:


{:.line-numbers}
```groovy
#!/usr/bin/env groovy

stage ("Fake timeout") {
  echo 'Waiting 10 minutes for deployment to complete'
  sleep 1 // seconds
}
```

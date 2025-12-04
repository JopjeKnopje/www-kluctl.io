---
title: "Hooks and jobs"
linkTitle: "hooks and jobs"
weight: 5
description: >
    Basic example on running jobs using hooks.
---

## Introduction

Jobs only have some [mutable fields](https://kubernetes.io/docs/concepts/workloads/controllers/job/#mutable-scheduling-directives) making them tricky to patch/update this problem can be solved by using [hooks]({{% ref "docs/kluctl/deployments/hooks.md" %}}).


In the following [example](https://kubernetes.io/docs/concepts/workloads/controllers/job/) we've marked a `Job` to be ran after the deployment, we've also specified kluctl not to wait for its completion.

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: pi
  annotations:
    kluctl.io/hook: post-deploy
    kluctl.io/hook-wait: false
spec:
  template:
    spec:
      containers:
      - name: pi
        image: perl:5.34.0
        command: ["perl",  "-Mbignum=bpi", "-wle", "print bpi(2000)"]
      restartPolicy: Never
  backoffLimit: 4
```

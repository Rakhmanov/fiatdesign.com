+++
title = "Deploying Job and CronJob with YAML Anchors and List"
date = "2025-04-08"
author = "Denis"
description = ""
tags = ["Kubernetes"]
categories = ["Development"]
+++

# DRY Kubernetes: Deploying Job and CronJob with YAML Anchors and List

Managing both a one-time `Job` and a recurring `CronJob` in Kubernetes often leads to duplicated specs. In most cases, they share the same container image, environment variables, volumes, and execution logic — so why repeat yourself?

This article shows how to eliminate duplication by using:
- YAML anchors (`&` and `<<:`) for reusable specs
- Kubernetes `List` kind to group related resources into a single manifest

---

## Why This Pattern?

You may want both:
- A `Job` you can trigger manually (e.g., ad hoc sync or debug run)
- A `CronJob` that runs on a schedule (e.g., nightly mirror)

But you don’t want to maintain the full `spec.template` twice.

---

## The YAML Pattern

📄 base/job-cron.yaml
```yaml
apiVersion: v1
kind: List
items:
  - apiVersion: batch/v1
    kind: Job
    metadata:
      name: custom-job
    spec: &jobSpec
      backoffLimit: 1
      template:
        spec:
          restartPolicy: Never
          containers:
            - name: runner
              image: alpine
              command: ["sh", "-c", "echo Running job"]
  - apiVersion: batch/v1
    kind: CronJob
    metadata:
      name: custom-cron
    spec:
      schedule: "0 0 * * *"
      jobTemplate:
        spec:
          <<: *jobSpec
```

## How It Works

- `&jobSpec` defines an anchor at the `Job.spec` level.
- `<<: *jobSpec` uses the anchor to inject that structure into the `CronJob`'s `jobTemplate.spec`.
- `kind: List` bundles the two resources into a single file you can `kubectl apply -f`.


## Using Kustomize to control Cronjob and Job

📄 base/kustomization.yaml

```yaml
resources:
  - job-cron.yaml
```

Example of deletion of Job to keep CronJob
📄 overlays/prod/kustomization.yaml

```yaml
resources:
  - ../../base

patches:
  - target:
      kind: Job
      name: custom-job
    patch: |-
      apiVersion: batch/v1
      kind: Job
      metadata:
        name: custom-job
      $patch: delete
```

You folder structure will look like this.
```
📁 custom-job-kustomize/
├── 📁 base/
│   ├── job-cron.yaml
│   └── kustomization.yaml
├── 📁 overlays/
│   └── 📁 prod/
│       └── kustomization.yaml
```


---

## Why Use This?

- Avoids drift between `Job` and `CronJob` definitions
- Improves maintainability and readability
- Works well in GitOps and Kustomize bases

---

## Gotchas

- YAML anchors don’t work across separate files (unless you use a YAML preprocessor).
- The syntax is pure YAML — not a Kubernetes-specific feature — so it's resolved *before* reaching the API server.
- Editors and linters may complain unless configured properly.

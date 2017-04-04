# S2I Scripts for NodeBB on OpenShift

This repository contains the S2I scripts for building and running
[NodeBB](https://github.com/NodeBB/NodeBB) on OpenShift. It basically is a
NodeJS application which builds perfectly with the official
[NodeJS S2I builder](https://github.com/NodeBB/NodeBB). The only thing
modified in the scripts in this repo is the call of `./nodebb build` before
starting NodeBB in the `run` script.

## BuildConfig

Just specify where the custom S2I scripts are located:

Snippet:

```
[...]
  strategy:
    sourceStrategy:
      env:
      - name: NODE_ENV
        value: production
      from:
        kind: ImageStreamTag
        name: nodejs:4
        namespace: openshift
      scripts: https://raw.githubusercontent.com/appuio/nodebb-s2i/master
    type: Source
[...]
```

## DeploymentConfig

Add all needed configuration for NodeBB as environment variable. So you don't
need a configuration file.

Snippet:

```
[...]
    spec:
      containers:
      - env:
        - name: url
          value: http://localhost:4567
        - name: secret
          value: mysecret
        - name: database
          value: redis
        - name: port
          value: "4567"
        - name: redis__host
          value: "redis"
[...]
```


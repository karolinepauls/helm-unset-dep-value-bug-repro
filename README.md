# Inability to unset a subchart value - repro


```
helm dependency update master
```

As expected:
```
$ helm template dependency
---
# Source: dependency/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: config
data:
  key: set by the dependency
```

As expected - master renders its dependency:
```
$ helm template master
---
# Source: master/charts/dependency/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: config
data:
  key: set by the dependency
```

As expected - can unset nested values by setting them to null.
```
$ helm template dependency --set map.key=null
---
# Source: dependency/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: config
data:
  key:
```

Not expected. Unsetting dependency keys doesn't work.
```
$ helm template master --set dependency.map.key=null
---
# Source: master/charts/dependency/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: config
data:
  key: set by the dependency
```

However, setting them to something else does.
```
$ helm template master --set dependency.map.key=SOMETHING_ELSE
---
# Source: master/charts/dependency/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: config
data:
  key: SOMETHING_ELSE
```

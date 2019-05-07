# ktl
A tiny wrapper for kubectl. Have a comfortable k8s cli life with ktl!

**NOTE: ktl is NOT stable. e.g., several options like** `-n {namespace}` **will fail.**


## Features
### Overview
* Resource tree view
* Get parents/children/siblings of specified resources
* Switch contexts and namespaces easily (like kubectx)
* Resource name forward match
* Shorter commands


Suppose we have the following resources:
* service/nginx1
  * deployment/nginx1
    * replicaset/nginx1-rs
      * pod/nginx1-1
      * pod/nginx1-2
* service/nginx2
  * deployment/nginx2
    * replicaset/nginx2-rs
      * pod/nginx2-1


### Additional subcommands
#### Resource tree view (`tree`)
```
$ ktl tree svc nginx1
service/nginx1
  deployment/nginx1
    replicaset/nginx1-rs
      pod/nginx1-1
      pod/nginx1-2

$ ktl tree rs nginx2-rs
replicaset/nginx2-rs
  pod/nginx2-1
```

#### Search parents (`parents`, `parent`, `par`)
```
$ ktl parent rs nginx1-rs
NAME     DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
nginx1   2         2         2            2           4d
```

#### Search children (`children`, `child`, `chi`)
```
$ ktl child rs nginx1-rs
NAME       READY   STATUS    RESTARTS   AGE
nginx1-1   1/1     Running   1          4d
nginx1-2   1/1     Running   1          4d
```

#### Search siblings (`siblings`, `sibling`, `sib`)
```
$ ktl sibling po nginx1-1
NAME       READY   STATUS    RESTARTS   AGE
nginx1-1   1/1     Running   1          4d
nginx1-2   1/1     Running   1          4d
```

#### Switch contexts (`get/use ctx`)
```
$ ktl get ctx
CURRENT   NAME       CLUSTER    AUTHINFO   NAMESPACE
*         minikube   minikube   minikube   default
```

```
$ ktl use ctx minikube
```

If you have `fzf`, can choose a context interactively
```
$ ktl use ctx
```

#### Switch namespaces (`get/use ns`)
```
$ ktl get ns
CURRENT   NAME          STATUS   AGE
*         default       Active   4d
          kube-public   Active   4d
          kube-system   Active   4d
```

```
$ ktl use ns kube-system
Context "minikube" modified.
Active namespace is "kube-system".
```

If you have `fzf`, can choose a namespace interactively
```
$ ktl use ns
```


### Misc.
#### Resource name forward match (`^{resource name prefix}`)
Currently, supporting only `get`
```
$ ktl get po ^nginx1
NAME       READY   STATUS    RESTARTS   AGE
nginx1-1   1/1     Running   1          4d
nginx1-2   1/1     Running   1          4d

$ ktl get po ^nginx2
NAME       READY   STATUS    RESTARTS   AGE
nginx2-1   1/1     Running   1          4d
```

#### Fuzzy resource name matching
**NOT IMPLEMENTED**
**TODO**

#### Shorter commands
##### Command
* `kubectl` -> `ktl`

#### Subcommands
* `describe` -> `desc`
* `delete` -> `del`
* `config` -> `conf`
* `cluster-info` -> `info`

##### [OPT] Extremely short subcommands
**NOT IMPLEMENTED**

You can enable these subcommands by `KTL_EXTREMELY_SHORT_COMMAND=1`
* `get` -> `g`
* `describe` -> `d`
* `create` -> `c`
* `apply` -> `a`
* `replace` -> `r`
* `delete` -> `d`
* `logs` -> `l`
* `port-forward` -> `p`

#### Resource type
* `deployment` -> `dep`



## Usage
### Use a single wrapper script
```
mkdir -p /opt/ktl
wget https://raw.githubusercontent.com/amaya382/ktl/master/ktl -O /opt/ktl/ktl
chmod +x /opt/ktl/ktl
sudo ln -s /opt/ktl/ktl /usr/local/bin/ktl
```
Need only a single script `ktl` (and bash)

### As a plugin of kubectl
**NOT IMPLEMENTED**

Require `kubectl 1.13+`



## Acknowledgements
* [kubectx](https://github.com/ahmetb/kubectx)
  * Using a ton of them logic and codes, thanks a lot!


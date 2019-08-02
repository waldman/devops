# cycle

Reload an application unity on target environment with step by step introspection.

Currently it support Kubernetes (kube module) and ecs (ecs module).

This script was made to be used with fixed tag Docker images hosting/orchestration systems (where a simple cycle of the containers is enough to deploy a new revision/version).

## Usage
```
cycle - Reload an application unity on target environment.

Usage:
  cycle <module> <settings> <env_selector> <app_selector>

Available modules:
  kube
  ecs
```

## Settings files

The settings file should be on the `~/.cycle` folder and follow the format bellow:

`<module>-<setting_indentifier>.conf`

For example:
- `kube-<my_k8s_cluster_name>.conf`
- `ecs-<my_aws_account_name>.conf`

#### Expected settings - kube

```
kubectl="/path/to/your/kubectl/binary"
kube_context="your_cluster_context"
```

#### Expected settings - ecs

```
aws_profile="your_aws_profile"
aws_region="your_ecs_clusters_region"
```

## Instrospection

Every parameter of the command, when the last, will return a list with available options for the next step except for the <app_selector> that will actually run the cycle routine.

For example:
```
bash-3.2$ cycle

cycle - Reload an application unity on target environment.

Usage:
  cycle <module> <settings> <env_selector> <app_selector>

Available modules:
  kube
  ecs
```

```
bash-3.2$ cycle kube

cycle - Reload an application unity on target environment.

Usage:
  cycle kube <settings> <namespace_name> <deployment_name>

Available settings:
  setting-1
  setting-2
  setting-n
```

```
bash-3.2$ cycle kube setting-1

cycle - Reload an application unity on target environment.

Usage:
  cycle kube setting-1 <namespace_name> <deployment_name>

Available namespaces:
  namespace-1
  namespace-2
  namespace-N
```

```
bash-3.2$ cycle kube setting-1 namespace-1

cycle - Reload an application unity on target environment.

Usage:
  cycle kube kube-setting-1 namespace-1 <deployment_name>

Available deployments:
  deployment-1
  deployment-2
  deployment-N
```

```
bash-3.2$ cycle kube setting-1 namespace-1 deployment-1
deployment.extensions/deployment-1 env updated
```

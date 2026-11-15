# Kubernetes Cluster Workload Distribution

Cloud Computing Lab Evaluation 2 - Problem 08

## Objective
Deploy multiple replicas on a multi-node Kubernetes cluster and examine how pods are distributed across worker nodes.

## Setup
- minikube, 3 nodes (`minikube start --nodes 3 -p lab8`)
- Application: nginx:1.25, 6 replicas

## Manifests
- `deploy-default.yaml` - default scheduling
- `deploy-spread.yaml` - topology spread constraints (maxSkew 1)
- `deploy-antiaffinity.yaml` - pod anti-affinity

## Results
| Scenario | lab8 | lab8-m02 | lab8-m03 | Pending |
|---|---|---|---|---|
| Default (6) | 2 | 2 | 2 | 0 |
| Topology spread (6) | 2 | 2 | 2 | 0 |
| Topology spread scaled (8) | 2 | 3 | 3 | 0 |
| Node lab8-m03 drained (8) | 2 | 3 | 0 | 3 |
| After uncordon (8) | 2 | 3 | 3 | 0 |
| Anti-affinity (6) | 1 | 1 | 1 | 3 |

## Conclusion
Default scheduling distributed pods evenly but without a guarantee. Topology spread kept the difference between nodes at 1 pod. Anti-affinity gave strict separation but left pods Pending when replicas exceeded nodes. The application stayed available during a node failure.

Screenshots are in the `screenshots` folder.

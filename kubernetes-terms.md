# Kubectl Terms: Quick Reference

## Cluster

A set of worker machines, called nodes, that run containerized applications. Every cluster has at least one worker node.[-]
The worker node(s) host the Pods that are the components of the application workload. The control plane manages the worker nodes and the Pods in the cluster. In production environments, the control plane usually runs across multiple computers and a cluster usually runs multiple nodes, providing fault-tolerance and high availability.

```kubectl-cli
# Display endpoint information about the master and services in a cluster
kubectl cluster-info

# Display the Kubernetes version running on the client and the server
kubectl version

# Get the configuration of the cluster
kubectl config view

# List the API resources that are available
kubectl api-resources

# List the API versions that are available
kubectl api-versions

# List everything
kubectl get all --all-namespaces
```

## Daemonsets

A DaemonSet ensures that all (or some) Nodes run a copy of a Pod. As nodes are added to the cluster, Pods are added to them. As nodes are removed from the cluster, those Pods are garbage collected. Deleting a DaemonSet will clean up the Pods it created.

Some typical uses of a DaemonSet are:

+ running a cluster storage daemon on every node
+ running a logs collection daemon on every node
+ running a node monitoring daemon on every node

In a simple case, one DaemonSet, covering all nodes, would be used for each type of daemon. A more complex setup might use multiple DaemonSets for a single type of daemon, but with different flags and/or different memory and cpu requests for different hardware types.

A DaemonSet ensures that all eligible nodes run a copy of a Pod. Normally, the node that a Pod runs on is selected by the Kubernetes scheduler. However, DaemonSet pods are created and scheduled by the DaemonSet controller instead. That introduces the following issues:

+ Inconsistent Pod behavior: Normal Pods waiting to be scheduled are created and in Pending state, but DaemonSet pods are not created in Pending state. This is confusing to the user.
+ Pod preemption is handled by default scheduler. When preemption is enabled, the DaemonSet controller will make scheduling decisions without considering pod priority and preemption.

```kubectl-cli
# List one of more daemonsets
kubectl get deamonset

# Edit and update the definition of one or more daemons
kubectl edit daemonset [daemonset name]

# Delete a daemonset
kubectl delete daemonset [daemonset name]

# Create a new daemonset
kubectl create daemonset [daemonset name]

# Manage the rollout of a daemonset
kubectl rollout daemonset

# Display the detailed state of daemonsets within a namespace
kubectl describe daemonset [daemonset name] -n [namespace name]
```

## Deployments

A Deployment provides declarative updates for Pods and ReplicaSets.

You describe a desired state in a Deployment, and the Deployment Controller changes the actual state to the desired state at a controlled rate. You can define Deployments to create new ReplicaSets, or to remove existing Deployments and adopt all their resources with new Deployments.

```kubectl-cli
# List one or more deployments
kubectl get deployment

# Display the detailed state of one or more deployments
kubectl describe deployment [deployment name]

# Edit and update the definition of one or more deployments on the server
kubectl edit deployment [deployment name]

# Create a new deployment
kubectl create deployment [deployment name]

# Delete a deployment
kubectl delete deployment [deployment name]

# Display the rollout status of a deployment
kubectl rollout status deployment [deployment name]
```

## Events

Event is a report of an event somewhere in the cluster. It generally denotes some state change in the system. Events have a limited retention time and triggers and messages may evolve with time. Event consumers should not rely on the timing of an event with a given Reason reflecting a consistent underlying trigger, or the continued existence of events with that Reason. Events should be treated as informative, best-effort, supplemental data.

```kubectl-cli
# List recent events for all resources in the system
kubectl get events

# List warnings only
kubectl get events --field-selector type=Warning

# List events but exclude Pod events
kubectl get events --field-selector involvedObject.kind!=Pod

# Pull events for a single node with a specific name
kubectl get events --field-selector involvedObject.kind=Node, involvedObject.name=[name node]

# Filter out normal events from a list of events
kubectl get events --field-selector type!=Normal
```

## Logs

Application logs can help you understand what is happening inside your application. The logs are particularly useful for debugging problems and monitoring cluster activity. Most modern applications have some kind of logging mechanism. Likewise, container engines are designed to support logging. The easiest and most adopted logging method for containerized applications is writing to standard output and standard error streams.

```kubectl-cli
# Print the logs for a pod
kubectl logs [pod name]

# Print the logs for the last hour for a specific pod
kubectl logs --since=1h [pod name] 

# Display the most recent 20 lines of a log
kubectl logs --tail=20 [pod name]

# Display the logs from a service and optionally select which container
kubectl logs -f [service name]  -c [container]

# Display the logs for a pod and follow new logs
kubectl logs -f [pod name]

# Display the logs for a container in a pod
#kubectl logs -c [container name] [pod name]

# Output the logs for a pod into a file named pod.log
kubectl logs [pod name] pod.log

# Display the logs for a previously failed pod
kubectl logs --previous [pod name]
```

## Manifest Files

Specification of a Kubernetes API object in JSON or YAML format.[-]
A manifest specifies the desired state of an object that Kubernetes will maintain when you apply the manifest. Each configuration file can contain multiple manifests.

```kubectl-cli
# Apply a configuration to an object by filename or stdin.  Overrides the existing configuration
kubectl apply -f manifest_file.yml

# Create objects
kubectl create -f manifest_file.yml

# Create objects in all manifest files in a directory
kubectl create -f ./dir

# Create objects from a URL
kubectl creaet -f 'url'

# Delete an object
kubectl delete -f manifest_file.yml
```

## Namespaces

In Kubernetes, namespaces provides a mechanism for isolating groups of resources within a single cluster. Names of resources need to be unique within a namespace, but not across namespaces. Namespace-based scoping is applicable only for namespaced objects (e.g. Deployments, Services, etc) and not for cluster-wide objects (e.g. StorageClass, Nodes, PersistentVolumes, etc).

Namespaces are intended for use in environments with many users spread across multiple teams, or projects. For clusters with a few to tens of users, you should not need to create or think about namespaces at all. Start using namespaces when you need the features they provide.

```kubectl-cli
# Create namespace
kubectl create namespace [namespace name]

# List one or more namespaces
kubectl get namespaces [namespace name]

# Display the detailed state of one or more namespaces
kubectl describe namespace [namespace name]

# Delete a namespace
kubectl delete namespace [namespace name]

# Edit and Update the definition of a namespace
kubectl edit namespace [namespace name]

# Display resource usage for a namespace
kubectl top namespace [namespace name]
```

## Nodes

Kubernetes runs your workload by placing containers into Pods to run on Nodes. A node may be a virtual or physical machine, depending on the cluster. Each node is managed by the control plane and contains the services necessary to run Pods.

Typically you have several nodes in a cluster; in a learning or resource-limited environment, you might have only one node.

The components on a node include the kubelet, a container runtime, and the kube-proxy.

The name identifies a Node. Two Nodes cannot have the same name at the same time.

```kubectl-cli
# Update the taints on one or more nodes
kubectl taint node [node name]

# List one or more nodes
kubectl get node

# Delete a node or multiple nodes
kubectl delete node [node name]

# Display resource usage for all nodes
kubectl top node

# Display resource allocation per node
kubectl describe node | grep Allocated -A 5

# Display pods running on a specific node
kubectl get pods -o wide | grep [node name]

# Annotate a node
kubectl annotate node [node name]

# Mark a node as unschedulable
kubectl cordon node [node name]

# Mark a node as schedulable
kubectl uncordon node [node name]

# Drain a node
kubectl drain node [node name]

# Add or Update the labels of one or more nodes
kubectl label node
```

## Pods

Pods are the smallest deployable units of computing that you can create and manage in Kubernetes.

A Pod (as in a pod of whales or pea pod) is a group of one or more containers, with shared storage and network resources, and a specification for how to run the containers. A Pod's contents are always co-located and co-scheduled, and run in a shared context. A Pod models an application-specific "logical host": it contains one or more application containers which are relatively tightly coupled. In non-cloud contexts, applications executed on the same physical or virtual machine are analogous to cloud applications executed on the same logical host.

A Pod is similar to a set of containers with shared namespaces and shared filesystem volumes.

Pods are generally not created directly and are created using workload resources.

```kubectl-cli
# List one or more pods
kubectl get pod

# Delete a pod
kubectl delete pod [pod name]

# Display the details of a pod
kubectl describe pod [pod name]

# Create a pod
kubectl create pod [pod name]

# Execute a command inside a pod
kubectl exec [pod name] -c [container name] <command>

# Get interactive shell on a pod
kubectl exec -it [pod name] --bash

# Display resource usage for pods
kubectl top pod

# Add or Update the annotations of a pod
kubectl annotate pod [pod name] [annotation]

# Add or Update the label of a pod
kubectl label pod [pode name]
```

## Replication Controllers

**Note: A Deployment that configures a ReplicaSet is now the recommended way to set up replication.**

A ReplicationController ensures that a specified number of pod replicas are running at any one time. In other words, a ReplicationController makes sure that a pod or a homogeneous set of pods is always up and available.

```kubectl-cli
# Display a list the replication controllers
kubectl get rc

# Displaya list of the replication controllers by namespace
kubectl get rc --namespace="[namespace name]"
```

## ReplicaSets

A ReplicaSet is defined with fields, including a selector that specifies how to identify Pods it can acquire, a number of replicas indicating how many Pods it should be maintaining, and a pod template specifying the data of new Pods it should create to meet the number of replicas criteria. A ReplicaSet then fulfills its purpose by creating and deleting Pods as needed to reach the desired number. When a ReplicaSet needs to create new Pods, it uses its Pod template.

A ReplicaSet's purpose is to maintain a stable set of replica Pods running at any given time. As such, it is often used to guarantee the availability of a specified number of identical Pods.

```kubectl-cli
# Display a list of ReplicaSets
kubectl get replicasets

# Display the details of one or more ReplicaSets
kubectl describe replicasets [replicaset name]

# Scale a ReplicaSet
kubectl scale --replicas=[number of replicas]
```

## Secrets

A Secret is an object that contains a small amount of sensitive data such as a password, a token, or a key. Such information might otherwise be put in a Pod specification or in a container image. Using a Secret means that you don't need to include confidential data in your application code.

```kubectl-cli
# Create a secret
kubectl create secret

# Display a list of secrets
kubectl get secrets

# Display list of details about secrets
kubectl describe secrets

# Delete a secret
kubectl delete secret [secret name]
```

## Services

An abstract way to expose an application running on a set of Pods as a network service. The set of Pods targeted by a Service is usually determined by a selector

```kubectl-cli
# Display a list of services
kubectl get services

# Display the details of the services
kubectl describe services

# Expose a replication controller, service, deployment or pod as a new Kubernetes service
kubectl expose deployment [deployment name]

# Edit and Update a service definition
kubectl edit services
```

## Service Accounts

A service account provides an identity for a pod

```kubectl-cli
# List service accounts
kubectl get serviceaccounts

# Display the details of one or all service accounts
kubectl describe serviceaccounts

# Replace a service account
kubectl replace serviceaccount

# Delete a service account
kubectl delete serviceaccount [service account name]
```

## Stateful Set

StatefulSet is the workload API object used to manage stateful applications
Manages the deployment and scaling of pods. Provides guarantees about the ordering and uniqueness of the deployed pods.

```kubectl-cli
# List StatefulSet
kubectl get statefulset

# Delete StatefulSet only (does not delete pod)
kubectl delete statefulset/[stateful set name] --cascade=false
```

# Kubectl Commands: Quick Reference

## RUN

Create and run a particular image in a pod.

```kubectl-cli
# Start a nginx pod
kubectl run nginx --image=nginx
```

## GET

Display one or many resources.

Prints a table of the most important information about the specified resources. You can filter the list using a label selector and the --selector flag. If the desired resource type is namespaced you will only see results in your current namespace unless you pass --all-namespaces.

```kubectl-cli
# Display a list of all namespaces
kubeclt get namespaces

# Display a list of all pods
kubectl get pods

# Display a detailed list of all pods
kubectl get pods -o wide

# Display a list of all running pods on a particular node server
kubectl get pods --field-selector=spec.nodeName=[server name]

# Display a list a specific replication controller
kubectl get replicationcontroller [replication-controller-name]

# Display a list of all replication controllers and services
kubectl get replicationcontroller,services

# Display a list of all daemon sets
kubectl get deamonset
```

## CREATE

Create a resource from a file or from stdin.

JSON and YAML formats are accepted.

```kubectl-cli
# Create a new namespace
kubectl create namespace [namespace name]

# Create a resource from a file (json or yml)
kubectl create -f filename

# Create a pod using the data in pod.json
kubectl create -f ./pod.json

# Create a pod based on the JSON passed into stdin
cat pod.json | kubectl create -f -

# Edit the data in registry.yaml in JSON then create the resource using the edited data
kubectl create -f registry.yaml --edit -o json
```

## EXPOSE

Expose a resource as a new Kubernetes service.

Looks up a deployment, service, replica set, replication controller or pod by name and uses the selector for that resource as the selector for a new service on the specified port. A deployment or replica set will be exposed as a service only if its selector is convertible to a selector that service supports, i.e. when the selector contains only the matchLabels component. Note that if no port is specified via --port and the exposed resource has multiple ports, all will be re-used by the new service. Also if no labels are specified, the new service will re-use the labels from the resource it exposes.

```kutectl-cli
# Create a service for a replicated nginx, which serves on port 80 and connects to the containers on port 8000
kubectl expose rc nginx --port=80 --target-port=8000

# Create a service for a replicated nginx using replica set, which serves on port 80 and connects to the containers on port 8000
kubectl expose rs nginx --port=80 --target-port=8000

#Create a service for an nginx deployment, which serves on port 80 and connects to the containers on port 8000
kubectl expose deployment nginx --port=80 --target-port=8000
```

## APPLY

Applying and Updating a Resource

Apply a configuration to a resource by file name or stdin. The resource name must be specified. This resource will be created if it doesn't exist yet. To use 'apply', always create the resource initially with either 'apply' or 'create --save-config'.

JSON and YAML formats are accepted.

```kubectl-cli
# Create a new service with the definition contained in a yml file
kubectl apply -f [service-name].yml

# Create a new replication container from a yml file
kubectl apply -f [controller-name].yml

# Create the objects defined in any .yml file stored in a directory
kubectl apply -f [directory name]
```

## DESCRIBE

Show details of a specific resource or group of resources.

Print a detailed description of the selected resources, including related resources such as events or controllers. You may select a single object by name, all objects of that type, provide a name prefix, or label selector.

```kubectl-cli
# View details regarding a particular node
kubectl describe nodes [node name]

# View details about all pods
kubectl describe pods

# Display details about a pod from a json file
kubectl describe -f pod.json

# Display details about all pods managed by specific replication controller
kubectl describe pods [replication controller name]

# Describe pods by label name=[my Label]
kubectl describe po -l name=[my Label]

```

## DELETE

Delete a Resource

Delete resources by file names, stdin, resources and names, or by resources and label selector.

JSON and YAML formats are accepted. Only one type of argument may be specified: file names, resources and names, or resources and label selector.

Some resources, such as pods, support graceful deletion. These resources define a default period before they are forcibly terminated (the grace period) but you may override that value with the --grace-period flag, or pass --now to set a grace-period of 1.

```kubectl-cli
# Remove a pod using pod.yml file
kubectl delete -f pod.yml

# Remove all pods and services with a specific label
kubectl delete pods,services -l [label key]=[label value]

# Remove all pods
kubectl delete pods --all

# Delete a pod with minimal delay
kubectl delete pod [pod name] --now

# Force delete a pod on a dead node
kubectl delete pod [pod name] --force
```

## EXEC

Execute a command in a container.

```kubectl-clt
# Recieve output form a command run on the first container in a pod
kubectl exec [pod name] [--command]

# Display output from a command run on a specific container in a pod
kubectl exec [pod name] -c [container name] [--command]

# Run bin/bash
kubectl exec -ti [pod name] --/bin/bash

# Switch to raw terminal mode; sends stdin to 'bash' in a container from a pod and sends stdout/stderr from 'bash' back to the client
kubectl exec [pod name] -c [container name] -it -- bash
```

## CONFIG

Modify kubeconfig files using subcommands like "kubectl config set current-context my-context"

```kubectl-clt
# Display current context
kubectl config current-context

# Set a cluster entry in kubeconfig
kubectl config set-cluster [cluster name] -server=[server name]

# Set individual value in a kubeconfig file
kubectl config set [property name] [property value]

# Unset an individual value in a kubeconfig file.
kubectl config unset [property name]
```

## ANNOTATE

All Kubernetes objects support the ability to store additional data with the object as annotations. Annotations are key/value pairs that can be larger than labels and include arbitrary string values such as structured JSON. Tools and system extensions may use annotations to store their own data.

```kubectl-clt
# Update pod with annotation key / value pair
kubectl annotate pods [pod name] [key]='[value]'

# Update a pod indentified by type and name in "pod.json"
kubectl annotate -f pod.json [key]='[value]'
```

____

## Short Names

| **Short Name** | **Long Name**              |
|----------------|----------------------------|
| csr            | certificatesigningrequests |
| cs             | componentstatuses          |
| cm             | configmaps                 |
| ds             | daemonsets                 |
| deploy         | deployments                |
| ep             | endpoints                  |
| ev             | events                     |
| hpa            | horizontalpodautoscalers   |
| ing            | ingresses                  |
| limits         | limitranges                |
| ns             | namespaces                 |
| no             | nodes                      |
| pvc            | persistentvolumeclaims     |
| pv             | persistentvolumes          |
| po             | pods                       |
| pdb            | poddisruptionbudgets       |
| psp            | podsecuritypolicies        |
| rs             | replicasets                |
| rc             | replicationcontrollers     |
| quota          | resourcequotas             |
| sa             | serviceaccounts            |
| svc            | services                   |

____

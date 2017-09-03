# Openshift CLI CheatSheet

## Nodes

### View all nodes
`oc get nodes`

### View nodes labels
`oc get nodes --show-labels`

### Add label to node
`oc label nodes <nodes name> <key=value>`

### Add label to all nodes
`oc label nodes --all <key=value>`

### Overwrite label
`oc label nodes <node name> <key=value> --overwrite`

### Remove label from node
`oc label nodes <node name> <key>-`

### Get nodes information
`oc describe node <node name>`

---

```
The parameter -n <project name> is optional in all commands below although it is recommended in order to avoid any problem or confusion between projects when using Openshift CLI
```

## Pods

### View pods for a specific project
`oc get po -n <project name>`

### View nodes each pod is running
`oc get po -o wide`

### Watch for changes on pods
`oc get po -w` or `watch oc get po`

### View all pods in all projects
`oc get po --all-namespace`

### View all pods and nodes in all projects
`oc get po --all-namespace -o wide`

### View pods logs
`oc logs <pods name>`

### Follow pods logs
`oc logs -f <pods name>`

### View logs from a pod with more than 1 container
`oc logs <pods name> -c <containers name>`

### View logs from the last 30 seconds
`oc logs <pods name> --since=30s`

### View the last 60 lines of logs
`oc logs <pods name> --tail=60`

### Scale your pod to 5 replicas
`oc scale dc <dc name> --replicas=5`

### Scale your pods when you don't have a dc
`oc scale rc <rc name> --replicas=5`

### Remote access to a POD
`oc rsh <pod name>`

### Rsync to a POD
`oc rsync <local dir> <pod name>:<remote dir>`

---

## Projects

### View projects
`oc get projects`

### Edit project
`oc edit namespace <projects name>`

### Delete project
`oc delete project <projects name>`

### Create project
`oc new-project <projects name> --display-name="<projects display name> --description=<projects description>"`

*The params display-name and description are optionals*

### Create project with another admin user
`oc adm new-project --admin=<admins name>`

### Create project with specific node selector
`oc adm new-project --node-selector=<node selector>`

### Create cluster default project
`oc adm create-bootstrap-project-template -o yaml > my-template.yaml`

Edit it as you want

`vi my-template.yaml`

Import to the cluster

`oc create -f my-template.yaml -n default`

Edit master-config.yaml
```
...
  projectRequestTemplate: "default/project-request"
```

### Add admin role to another user in your project
`oc adm policy add-role-to-user admin <user> -n <project>`

### Label project
`oc label project <projects name> key=value`

### Allow any container to run as root in your project
`oc adm add-scc-to-user anyuid -z default -n <project>`

---

## User

### View users
`oc get users`

### Add cluster-admin role to user
`oc adm policy add-cluster-role-to-user cluster-admin <user>`

### Add user permission for create project
`oc adm policy add-cluster-role-to-user self-provisioner <user>`

### Remove user permission for create projects
`oc adm policy remove-cluster-role-from-user self-provisioner <user>`

### Remove permission for all users to create projects
`oc adm policy remove-cluster-role-from-user self-provisioner system:authenticated`

`oc adm policy remove-cluster-role-from-user self-provisioner system:authenticated:oauth`

### Delete user from Openshift cache
`oc delete user <user>`

`oc delete identity <identity:user>`

---

## Service Account

### Create service account
`oc create serviceaccount <service account name`

### Get service account
`oc get sa`

### Delete service account
`oc delete sa <service account>`

### Edit service account
`oc edit sa <service account>`

### Get token from ServiceAccount
`oc serviceaccounts get-token <service account>`

### Add role/permission to a service account
`oc adm police add-role-to-user <role> -z <service account> -n <project>`

---

## ConfigMap

### Create configmap

## Clean up
`oc adm prune images --confirm`

`oc adm prune builds --confirm`

`oc adm prune deploments --confirm`

`oc adm top images`

`docker rm $(docker ps -a -q)`


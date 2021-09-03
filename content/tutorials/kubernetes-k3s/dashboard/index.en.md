---
title: "Install and access the K8s Web UI Dashboard"
date: 2021-08-03T17:01:01+01:00
draft: false
series: ["kubernetes_k3s"]
categories: [ubuntu-snapcraft, kubernetes]
---

While I don't find the dashboard very useful for configuring anything in the cluster, it can be helpful to find a resource you've lost track of or discover resources you didn't know were there.

Before following this guide, you should have an installed kubernetes cluster. If you don't, check out the guide how to [Install K3s](../1.a.k3s)<!-- or [Install Microk8s](../1.b.microk8s) -->.

## Installing the dashboard

To install the dashboard we need to run the following one command on the primary cluster node (in my example, this is `k8s-1`). K3s installations require the command be prefixed with `sudo`:

1. - K3s:
      ```bash
      sudo kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.3.1/aio/deploy/recommended.yaml
      ```
   <!-- - Microk8s:
      ```bash
      kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.3.1/aio/deploy/recommended.yaml
      ``` -->

This URL includes the version number, so you will want to double check that it is up-to-date when following the instruction. The latest version will show up at the [K8s dashboard latest release page](https://github.com/kubernetes/dashboard/releases/latest).

![Screenshot of terminal output from installing the K8s Web UI Dashboard](installation.png)

## Creating the admin user

Accessing the dashboard requires that we supply a token to authorise ourselves. This account is not created by the command above.

1. On the primary cluster node create a file called `dashboard.admin-user.yml` with the following content:
    ```yaml
    apiVersion: v1
    kind: ServiceAccount
    metadata:
      name: admin-user
      namespace: kubernetes-dashboard
    ```

    I recommend using `nano` to create the file:

    ```bash
    nano -w dashboard.admin-user.yml
    ```

    To save the file and exit `nano` once you've copied the contents into the terminal type <kbd>ctrl</kbd>+<kbd>x</kbd> followed by <kbd>y</kbd> to confirm that you want to save the file and finally <kbd>enter</kbd>.

    When applied to the cluster this will create our user account called `admin-user` and store it into the `kubernetes-dashboard` namespace, which was created by the installation step above.

    - The `apiVersion` must be set to `v1` so that K8s knows which fields are acceptible.
    - The `kind` must be set to `ServiceAccount` to tell K8s that we're attempting to create a user.

1. Also on the primary cluster node create another file called `dashboard.admin-user-role.yml` with the following content:
   ```yaml
   apiVersion: rbac.authorization.k8s.io/v1
   kind: ClusterRoleBinding
   metadata:
     name: admin-user
   roleRef:
     apiGroup: rbac.authorization.k8s.io
     kind: ClusterRole
     name: cluster-admin
   subjects:
   - kind: ServiceAccount
     name: admin-user
     namespace: kubernetes-dashboard
   ```

   When applied to the cluster this will create a "role binding" to bind our user account to the cluster admin role.

   - The `apiVersion` must be set to `rbac.authorization.k8s.io/v1` to tell K8s that we're using the RBAC API version 1. A different `apiVersion` will likely change the fields available for use below, so will break our attempt to apply to the cluster.
   - The `kind` field must be set to `ClusterRoleBinding` to tell K8s that we're attempting to create a Cluster Role Binding that binds an account to a cluster role.
   - The `metadata.name` entry is an arbitrary name for the role binding but it makes sense to name this the same as the user account that we're binding.
   - `subjects` list must include one or more items. Each item in the list must have a `kind`, `name`, and `namespace` field.
      - The `kind` field must be set to `ServiceAccount` since that is what we used to create our `admin-user`.
      - The `name` should be set to the same as that we used in the `dashboard.admin-user.yml` file, i.e. `admin-user`.
      - The `namespace` should be set to the same name as that we used in the `dashboard-admin-user.yml` file so that K8s can find the correct service account, i.e. `kubernetes-dashboard`.
   - The `roleRef` configuration specifies the role that each user in the `subjects` list is assigned to.
      - The `apiGroup` field must be set to `rbac.authorization.k8s.io` to tell K8s that we're referencing a role-based access control role.
      - The `kind` field should be set to that of the accounts in the `subjects` list are granted the role of `cluster-admin` - This is a default role in K8s.

1. Apply these configurations to the cluster with the following command:
   - K3s:
      ```bash
      sudo kubectl create -f dashboard.admin-user.yml -f dashboard.admin-user-role.yml
      ```
   <!--- Microk8s:
      ```bash
      kubectl create -f dashboard.admin-user.yml -f dashboard.admin-user-role.yml
      ```-->

## Accessing the dashboard

Accessing the dashboard is tricky because you need to both access over HTTPS, or via `localhost` (i.e. from the same machine). You also need to get a token to authorise your access with. I will show how to access remotely over HTTPS (**do not** do this if your cluster is on a public network!):

1. On the primary cluster node edit the `kubernetes-dashboard` service:
    - K3s
       ```bash
       sudo env EDITOR=nano kubectl edit service kubernetes-dashboard -n kubernetes-dashboard
       ```
    <!--- Microk8s
       ```bash
       env EDITOR=nano kubectl edit service kubernetes-dashboard -n kubernetes-dashboard
       ```-->

    1. Move the cursor down to the line that includes `type: ClusterIP` and change it to `type: NodePort` with the same indentation. (This is the configuration path `.spec.type`)

    1. Save and exit the editor with <kbd>ctrl</kbd>+<kbd>x</kbd> followed by <kbd>y</kbd> to indicate that we want to save the file and finally <kbd>enter</kbd> to confirm.

1. On the primary cluster node run the following to determine the port number to connect to the Web UI:
   ```bash
   sudo kubectl get service kubernetes-dashboard -n kubernetes-dashboard
   ```

   In my installation this prints:

   ```plain
   NAME                   TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)         AGE
   kubernetes-dashboard   NodePort   10.43.128.69   <none>        443:31235/TCP   126m
   ```

   The important part is the `PORT` column which has the value `443:31235` here. The second number, after the colon (here it is `31235`), is the port we will use to connect. The hostname is the primary node address. In my case this will mean an address of `https://k8s-1:31235/`. Load this address in your web browser and accept the warning about a self-signed security certificate to load the UI.

1. On the primary cluster node run the following to get your token:
   - K3s:
      ```bash
      echo $(sudo kubectl -n kubernetes-dashboard get secret $(sudo kubectl -n kubernetes-dashboard get sa/admin-user -o jsonpath="{.secrets[0].name}") -o go-template="{{.data.token | base64decode}}")
      ```
   <!--- Microk8s:
      ```bash
      echo $(kubectl -n kubernetes-dashboard get secret $(kubectl -n kubernetes-dashboard get sa/admin-user -o jsonpath="{.secrets[0].name}") -o go-template="{{.data.token | base64decode}}")
      ```-->

    Copy and paste the output into the field on the Web UI login screen.

You should now be able to navigate to the `nodes` item in the left-hand menu of the UI to see your nodes:

![Screenshot of the Web UI Dashboard showing the nodes view](webui-nodes.png)

{{< ads circuit=false >}}

## Other chapters in this series

{{< series "kubernetes_k3s" >}}

## Other series

{{< series "kubernetes" >}}

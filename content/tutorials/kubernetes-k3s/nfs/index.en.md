---
title: "Kubernetes with NFS Persistent Volumes"
date: 2021-08-03T17:02:01+01:00
draft: false
series: ["kubernetes_k3s"]
categories: [ubuntu-snapcraft, kubernetes]
---

Using NFS persistent volumes is a relatively easy, for kubernetes, on-ramp to using the kubernetes storage infrastructure.

Before following this guide, you should have an installed kubernetes cluster. If you don't, check out the guide how to [Install K3s](../k3s)<!-- or [Install Microk8s](microk8s) -->.

## Setting up the NFS share

We will share a directory on the primary cluster node for all the other nodes to access. I will assume that you want to share a filesystem mounted at `/mnt/storage-disk`.

1. Install the NFS service:
   
   ```bash
   sudo apt update && sudo apt install -y nfs-kernel-server
   ```

1. Edit the file `/etc/exports`:
   
   ```bash
   sudo nano -w /etc/exports
   ```

1. Add a new line at the bottom with the following content:

   ```plain
   /mnt/storage-disk *(rw,sync,no_subtree_check,no_root_squash,anonuid=65534,anongid=65534)
   ```

1. Save and exit the editor with <kbd>ctrl</kbd>+<kbd>x</kbd> followed by <kbd>y</kbd> to indicate that we want to save the file and finally <kbd>enter</kbd> to confirm.

1. Load the configuration we just wrote into the NFS server:

   ```bash
   sudo exportfs -a
   ```

## Install nfs client onto each of the other nodes

On each of the other nodes, we need the nfs client to be installed or pods will fail to schedule and start. On `k8s-2` and `k8s-3` run:

```bash
sudo apt update && sudo apt install -y nfs-common
```

## Configuring the NFS provisioner

We could create a Persistent Volume and Persistent Volume Claim manually, but there's an automated method using a Provisioner. This is a special configuration that takes our NFS share and automatically slices it up into Persistent Volumes whenever a new Persistent Volume Claim is created up till the provisioned storage space is exhausted.

To install the provisioner we'll first get `helm` because it simplifies the installation inordinately!

1. Install `helm` on the primary cluster node:

   ```bash
   curl -fsSL https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
   ```

1. Configure `helm` to access the provisioner repository:

   ```bash
   sudo helm repo add nfs-subdir-external-provisioner https://kubernetes-sigs.github.io/nfs-subdir-external-provisioner/
   ```

1. Using `helm` install the provisioner:

   ```bash
   sudo env KUBECONFIG=/etc/rancher/k3s/k3s.yaml \
   helm install nfs-subdir-external-provisioner nfs-subdir-external-provisioner/nfs-subdir-external-provisioner \
     --set nfs.server=k8s-1 \
     --set nfs.path=/mnt/storage-disk
   ```

   There are many options that may be set with the `--set` flag. Each option must be supplied with their own `--set option.name=value` parameter pair. For the full list of parameters see [the configuration section of the helm chart readme for the NFS provisioner](https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner/blob/master/charts/nfs-subdir-external-provisioner/README.md#configuration).

## Creating a test deployment to verify the provisioner

Now we will test that the provisioner is working by creating a test claim and a test deployment that uses the claim.

1. Create and edit a file called `test-claim.yaml`:

   ```bash
   nano -w test-claim.yaml
   ```

1. Paste the following content into the editor:

   ```yaml
   apiVersion: v1
   kind: PersistentVolumeClaim
   metadata:
     name: test-claim
   spec:
     storageClassName: nfs-client
     accessModes:
       - ReadWriteMany
     resources:
       requests:
         storage: 1Mi
   ```

   - The `apiVersion` must be set to `v1` so that K8s knows which fields are acceptible.

   - The `kind` must be `PersistendVolumeClaim` to inform K8s that we are creating a PVC.

   - The `metadata.name` entry is an arbitrary name for the PVC but we will use it to tie the PVC to the pod in the following steps.

   - The `spec.storageClassName` must be `nfs-client` because this is what the default installation using `helm` sets up.

   - The `spec.accessModes` should be a list with a single item of value `ReadWriteMany`.
     
     The available access modes are:

     - ReadWriteOnce -- the volume can be mounted as read-write by a single node
     - ReadOnlyMany -- the volume can be mounted read-only by many nodes
     - ReadWriteMany -- the volume can be mounted as read-write by many nodes

   - The `spec.resources.requests.storage` should be a number suffixed with `Mi`, `Gi`, or `Ti` indicating Mebibytes (`1024*1024` bytes), Gibibytes (`1024*1024*1024` bytes), or Tebibytes (`1024*1024*1024*1024` bytes). E.g. `5Gi` would equal five Gibibytes or `5*1024*1024*1024` bytes. If there is insufficient space in the NFS share then this PVC will stay in a pending state.

1. Save and exit the editor with <kbd>ctrl</kbd>+<kbd>x</kbd> followed by <kbd>y</kbd> to indicate that we want to save the file and finally <kbd>enter</kbd> to confirm.

1. Create and edit a file called `test-pod.yaml`:

   ```bash
   nano -w test-pod.yaml
   ```

1. Paste the following content into the editor:

   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: test-pod
   spec:
     containers:
     - name: test-pod
       image: gcr.io/google_containers/busybox:1.24
       command:
         - "/bin/sh"
       args:
         - "-c"
         - "touch /mnt/SUCCESS && exit 0 || exit 1"
       volumeMounts:
         - name: nfs-pvc
           mountPath: "/mnt"
     restartPolicy: "Never"
     volumes:
       - name: nfs-pvc
         persistentVolumeClaim:
           claimName: test-claim
   ```

   - The `apiVersion` must be set to `v1` so that K8s knows which fields are acceptible.

   - The `kind` must be `Pod` to inform K8s that we are creating a pod.

   - The `metadata.name` entry is an arbitrary name for the pod.

   - The `spec.containers` field takes a list of container definitions:

     - The `name` field is an arbitrary name for the container.
     - The `image` field is the docker-compatible container image name. Here we are using a container hosted by google called `busybox` with version `1.24`.
     - The `command` field is the command to run inside the container when it is launched.
     - The `args` field is a list of arguments to pass to the command specified above.
     - The `volumeMounts` field is a list of mount definitions for inside the container.
       - The `name` field in the mount definition is the name of the volume as configured in `.spec.volumes`.
       - The `mountPath` field tells K8s where to mount the filesystem from the volume into the container.
   
   - The `volumes` field is a list of volumes to make available for containers within this pod.
     - The volume `name` field is an arbitrary name that we use to reference in the `.spec.containers.volumeMounts.name` field.
     - The `persistentVolumeClaim.claimName` field is the name of a PVC. We configured this in our `test-claim.yaml` file's `.metadata.name` field, i.e. `test-claim`.

1. Save and exit the editor with <kbd>ctrl</kbd>+<kbd>x</kbd> followed by <kbd>y</kbd> to indicate that we want to save the file and finally <kbd>enter</kbd> to confirm.

1. Apply the claim and the pod definitions to the cluster:

   ```bash
   sudo kubectl apply -f test-claim.yaml -f test-pod.yaml
   ```

The container should now download its image, run and exit. Left behind will be a new folder called `/mnt/storage-disk/default-test-claim-pvc-*` where the `*` is a UUID. Inside this folder should be a single file called `SUCCESS` indicating that the PVC successfully provisioned, mounted into the container, and the container was able to write a file.

## Other chapters in this series

{{< series "kubernetes_k3s" >}}

## Other series

{{< series "kubernetes" >}}

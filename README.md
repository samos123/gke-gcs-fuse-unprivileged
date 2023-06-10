# Mount GCS Fuse without privileges in K8s/GKE
You can now use GCS fuse for GKE which doesn't require privileged pods: https://cloud.google.com/kubernetes-engine/docs/how-to/persistent-volumes/cloud-storage-fuse-csi-driver





OLD ARCHICED! Only use for non GCS use cases below:
------------------------

All credit should go to @pre: https://github.com/kubernetes/kubernetes/issues/7890#issuecomment-766088805


### Creating a Device Manager that allows sharing `/dev/fuse` to containers

Create the ConfigMap for the DeviceManager:
```
kubectl apply -f device-manager-cm.yaml
```

Create the device manager DaemonSet:
```
kubectl apply -f device-manager-ds.yaml
```

### Configure AppArmor to allow mounting fuse

Create a namespace for the AppArmor loader:
```
kubectl apply -f apparmor-ns.yaml
```

Create the ConfigMap that contains our AppArmor policy:
```
kubectl apply -f apparmor-cm.yaml
```

Create DaemonSet that runs AppArmor loader on each node:
```
kubectl apply -f apparmor-ds.yaml
```

### Mount fuse from a pod
Create a deployment that specifies our custom AppArmor policy
```
kubectl apply -f app-that-mounts-fuse.yaml
```

Now test by running in the fuser-mounter pod
```
kubectl exec -ti deployment/fuse-mounter -- bash
gcsfuse your-gcs-bucket3 /mnt
```


Disclaimer: Use at your own risk and customize as nessecary

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)





Longohorn Setup
---------------
https://github.com/balchua/do-microk8s/blob/master/docs/longhorn.md

```
$ microk8s helm3 install longhorn longhorn/longhorn \
  --namespace longhorn-system \
  --set defaultSettings.defaultDataPath="/longhorn" \
  --set csi.kubeletRootDir="/var/snap/microk8s/common/var/lib/kubelet" \
  --version 1.4.2
```

Longohorn Dashboard
-------------------

https://dashboard.k8s

username: longhorn
password: k8s

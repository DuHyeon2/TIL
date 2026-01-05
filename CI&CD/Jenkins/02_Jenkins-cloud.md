```
helm install nfs-subdir-userdata-provisioner nfs-subdir-external-provisioner/nfs-subdir-external-provisioner --namespace egis-cloud 
--set nfs.server=10.0.1.56 
--set nfs.path=/nfs-userdata 
--set storageClass.name=nfs-egis-userdata 
--set storageClass.reclaimPolicy=Retain 
--set storageClass.allowVolumeExpansion=true 
--set storageClass.mountOptions[0]=nfsvers=4.1 
--set storageClass.mountOptions[1]=rsize=1048576 
--set storageClass.mountOptions[2]=wsize=1048576 
--set storageClass.mountOptions[3]=hard 
--set storageClass.mountOptions[4]=timeo=600 
--set storageClass.mountOptions[5]=retrans=2 
--set storageClass.mountOptions[6]=noatime 
--set storageClass.mountOptions[7]=nodiratime 
--set storageClass.mountOptions[8]=sync
```
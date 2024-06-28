## Grafana : node(s) had volume node affinity conflict
### First we need to check the zone where the pv and the node by describing 
  ```
   kubectl describe nodes | egrep '^Name:|topology.kubernetes.io/zone'
  ```
  ```
   kubectl describe pv pvc-27a7e014-9f90-4baa-b1b7-e7de3b8d034d pvc-bab1e1d6-2625-4b51-85ac-6bbab89632b7 | egrep -A4 '^Name:|topology.kubernetes.io/zone|Node Affinity:'
  ```
  
### we need to take the backup of pv and create a snapshot
### Then create a disk from that snapshot
### Then create a pv 
 ```
 apiVersion: v1 
kind: PersistentVolume 
metadata: 
  name: grafana-pv 
spec: 
  capacity: 
    storage: 10Gi 
  accessModes: 
    - ReadWriteOnce 
  persistentVolumeReclaimPolicy: Retain 
  azureDisk: 
    kind: Managed 
    diskName: grafana-test 
    diskURI: <DiskURL> 
  storageClassName: default 
```

### Now create a pvc  in particular namespace

```
apiVersion: v1 
kind: PersistentVolumeClaim 
metadata: 
  name: grafana-pvc 
spec: 
  accessModes: 
    - ReadWriteOnce 
  resources: 
    requests: 
      storage: 10Gi 
  storageClassName: "default" 
  volumeName: grafana-pv
```
### Delete the old pvc
### Now check the pods
 



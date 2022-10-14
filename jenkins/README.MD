Add bitnami repo:
```sh
helm repo add bitnami https://charts.bitnami.com/bitnami
```

create namespacke jenkins:
'''sh 
kubectl create namespace jenkins
```
Install jenkins
```sh
helm install <name> bitnami/jenkins -n jenkins
```  

Create Storage Class:
```sh
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: local-storage
provisioner: kubernetes.io/no-provisioner
volumeBindingMode: WaitForFirstConsumer
 ```
 apply storageclass
 ```sh
 kubectl apply -f sc.yml
 ```
 
 Create PV: 
 ```sh
 apiVersion: v1
kind: PersistentVolume
metadata:
  name: jenkins-pv-volume
  labels:
    type: local
spec:
  storageClassName: local-storage
  claimRef:
    name: jenkins-pv-claim
    namespace: jenkins
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  local:
    path: /mnt
  nodeAffinity:
    required:
      nodeSelectorTerms:
      - matchExpressions:
        - key: kubernetes.io/hostname
          operator: In
          values:
          - node01
          
 ```
  apply pv
 ```sh
 kubectl apply -f pv.yml
 ```
 
 Create PVC:
 ```sh
 apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: jenkins-pv-claim
  namespace: jenkins
spec:
  storageClassName: local-storage
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
      
      
 ```
 
  apply pvc
 ```sh
 kubectl apply -f pvc.yml
 ```

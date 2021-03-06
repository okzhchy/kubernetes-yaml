# kubernetes-yaml
https://lms.quickstart.com/custom/858487/LFS258-Labs_V_2018-02-15.pdf  
### Deployment  
kubectl run nginx --image=nginx  
kubectl rollout history deployment nginx-deployment  
kubectl rollout undo deployment nginx-deployment --to-revision=1  
kubectl scale deployment nginx-deployment --replicas=2  
### Service
kubectl expose deployment nginx --type=NodePort --port=80 --target-port=80  
kubectl get ep nginx
### Label
kubectl label nodes <node-name> <label-key>=<label-value>  
kubectl get nodes k8s.node1 --show-labels  
kubectl get pod -l app=myapp  
kubectl get pod --show-labels |grep app=myapp |awk '{print $1}'  
### Taint
kubectl taint nodes node1 key=value:NoSchedule  
kubectl taint nodes node1 key:NoSchedule-  
kubectl describe node  |grep -i taint  
### Cluster
kubectl get cs 
kubectl cluster-info  
kubectl get event  
### ETCD
ETCDCTL_API=3 etcdctl \  
  --endpoints=https://172.30.105.3:2379  \  
  --cacert=/etc/kubernetes/ssl/ca.pem \  
  --cert=/etc/etcd/ssl/etcd.pem \  
  --key=/etc/etcd/ssl/etcd-key.pem \  
  snapshot save snapshotdb  
ETCDCTL_API=3 etcdctl --write-out=table snapshot status snapshotdb
### kubelet
kubectl create clusterrolebinding kubelet-bootstrap --clusterrole=system:node-bootstrapper --user=kubelet-bootstrap  
  --pod-manifest-path=/etc/kubernetes/manifests \  
  ExecStartPost=/usr/bin/touch /test.txt  
systemctl start kubelet  
systemctl enable kubelet  
kubectl get csr  
kubectl certificate approve csr-2b308  
kubectl get nodes  
### API
kubectl config view  
kubectl cluster-info  
curl --cert ./admin.pem --key ./admin-key.pem  --cacert ./ca.pem https://10.1.131.41:6443/  
### CongfigMap
kubectl create -f test-cfg.yaml
kubectl create configmap test-config --from-file=./configs  
kubectl create configmap test-config2 --from-file=./configs/db.conf --from-file=./configs/cache.conf  
kubectl create configmap test-config3 --from-literal=db.host=10.5.10.116 --from-listeral=db.port='3306'  
kubectl get configmap test-config -o yaml  
### Proxy
kubectl proxy --address='0.0.0.0' --port=8001 --accept-hosts='^\*$'  
curl http://127.0.0.1:8001/api  
### Kubectl
kubectl config get-contexts  
kubectl config set-context kubernetes \  
  --cluster=kubernetes \  
  --user=admin  
kubectl config use-context kubernetes  
### Secret
echo -n "admin" | base64  
echo "YWRtaW4=" | base64 --decode  
### ServiceAccount (ca.crt+namespace+token)
spec:  
  serviceAccountName: build-robot  
### Log
journalctl -xe -u kubelet  
find / -name "\*apiserver\*log"  
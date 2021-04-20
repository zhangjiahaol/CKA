**CKA考题**

命令补全：source <(kubectl completion bash)

**第一题------------------------------**

![img](C:/Users/jiazhang/AppData/Local/YNote/data/m18942285430@163.com/93b2b00b950748d2ae29a15eb319961b/clipboard.png)

kubectl config use-context k8s

kubectl create namespace app-team1

kubectl create clusterrole deployment-clusterrole --verb=create --resource=deployments,statefulsets,daemonsets

kubectl create sa cicd-token -n app-team1

kubectl -n app-team1 create rolebinding deployment-rolebinding --clusterrole deployment-clusterrole --serviceaccount app-team1:cicd-token

**第二题------------------------------**

![img](C:/Users/jiazhang/AppData/Local/YNote/data/m18942285430@163.com/b34fb924d332419f977ed598ecc04a3e/clipboard.png)

kubectl config use-context ek8s

kubectl cordon ek8s-node1

kubectl drain --delete-local-data=true --ignoredaemonsets=true ek8s-node1

**第三题------------------------------**

![img](C:/Users/jiazhang/AppData/Local/YNote/data/m18942285430@163.com/d383d20ef8ce403cacf2f6cfab9d71b5/clipboard.png)

配置环境

kubectl config use-context mk8s

开始操作

kubectl get nodes

ssh k8s-master1

sudo -i

kubectl drain k8s-master1 --ignoredaemonsets=true

apt update

apt install kubeadm=1.19.0-00 kubelet=1.19.0-00 kubectl=1.19.0-00

kubeadm upgrade apply 1.19.0 --etcd-upgrade=false

systemctl restart kubelet

kubectl uncordon k8s-master1

kubectl get nodes

**第四题------------------------------**

![img](C:/Users/jiazhang/AppData/Local/YNote/data/m18942285430@163.com/3831ff11dfd34dd3bde845763b7640ce/clipboard.png)

export ETCDCTL_API=3

docker ps -a |grep etcd

docker cp e99ee398d40c:/usr/local/bin/etcdctl /usr/bin/

\#备份

mkdir -p /data/backup/

etcdctl --endpoints="https://127.0.0.1:2379" --cacert="/etc/kubernetes/pki/etcd/ca.crt" --cert="/etc/kubernetes/pki/etcd/server.crt" --key="/etc/kubernetes/pki/etcd/server.key" snapshot save /data/backup/etcd-snapshot.db

\#还原

etcdctl snapshot restore /data/backup/etcd-snapshot.db

**第五题------------------------------**

![img](C:/Users/jiazhang/AppData/Local/YNote/data/m18942285430@163.com/1f4362e998784512aa1cc9b48d42d2b7/clipboard.png)

k8s.io官网搜索NetworkPolicy

vi NetworkPolicy.yaml

apiVersion: networking.k8s.io/v1

kind: NetworkPolicy

metadata:

  name: allow-port-from-namespace

  namespace: internal

spec:

  podSelector: {}

  policyTypes:

  \- Ingress

  ingress:

  \- from:

​    \- namespaceSelector:

​        matchLabels:

​          project: internal

​    \- podSelector: {}

​    ports:

​    \- protocol: TCP

​      port: 8080

kubectl apply -f NetworkPolicy.yaml

**第六题------------------------------**

![img](C:/Users/jiazhang/AppData/Local/YNote/data/m18942285430@163.com/19ed49b53b954cde84d262e61874f7b2/clipboard.png)

kubectl create deployment front-end --image=nginx:alpine

kubectl edit deployment front-end

...

  spec:

​    containers:

​    \- image: nginx:alpine

​      imagePullPolicy: IfNotPresent

​      name: nginx

​      ports:

​      \- containerPort: 80

​        name: http

​        protocol: TCP

​      resources: {}

...

kubectl get deployments.apps front-end --show-labels --no-headers

kubectl -n test expose deployment front-end --name=frontend-svc --type=NodePort 

kubectl get svc -n test

ssh master

curl -I 127.0.0.1:30971

**第七题------------------------------**

![img](C:/Users/jiazhang/AppData/Local/YNote/data/m18942285430@163.com/3978a055b1d2475f80680fc5b1aca74c/clipboard.png)

kubectl config use-context k8s

kubectl create ns ing-internal      #是否有namespace没有的话要创建

kubectl -n ing-internal create deployment hello --image=nginx:alpine-hi    #查看是否有svc没有的话要创建

在k8s官网搜索ingress

apiVersion: networking.k8s.io/v1 kind: Ingress metadata:  name: pong  namespace: ing-internal  annotations:    nginx.ingress.kubernetes.io/rewrite-target: / spec:  rules:  - http:      paths:      - path: /hello        pathType: Prefix        backend:          service:            name: hello            port:              number: 5678

kubectl get ingress -n ing-internal

kubectl edit ingress -n ing-internal pong    #看下IP是多少

curl -kL <INTERNAL_IP>/hello

**第八题------------------------------**

![img](C:/Users/jiazhang/AppData/Local/YNote/data/m18942285430@163.com/e48ebf93cf914086991f2489403702a1/clipboard.png)

kubectl config use-context k8s

kubectl get deployment

kubectl scale --replicas=4 deployment web-server

kubectl get deployment

**第九题------------------------------**

![img](C:/Users/jiazhang/AppData/Local/YNote/data/m18942285430@163.com/40d7b522f4794fa7906d4e7014a58d91/clipboard.png)

kubectl config use-context k8s

kubectl get nodes --show-labels

\# 有可能node已经打好了标签就不需要打了

kubectl label nodes k8s-node1 disk=spinning

\# k8s.io官网搜索nodeselector 复制第二个yaml

vi pod.yaml

apiVersion: v1 kind: Pod metadata:  name: nginx-kusc00401  labels:    app: nginx-kusc00401 spec:  containers:  - name: nginx    image: nginx    imagePullPolicy: IfNotPresent  nodeSelector:    disk: ssd

kubectl apply -f pod.yaml

kubectl get po -o wide --show-labels

**第十题------------------------------**

![img](C:/Users/jiazhang/AppData/Local/YNote/data/m18942285430@163.com/4d87453b0c47462299896648ba833383/clipboard.png)

kubectl config use-context k8s

kubectl get node

kubectl describe node | grep -i taint

mkdir -p /opt/KUSC00402

echo 2 > /opt/KUSC00402/kusc00402.txt

**第十一题------------------------------**

![img](C:/Users/jiazhang/AppData/Local/YNote/data/m18942285430@163.com/33b2d62e47bf47d9a096fc9bf29135c0/clipboard.png)

kubectl config use-context k8s

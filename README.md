# minnikube 啟動
    minikube start
    
# minikube Dashboard
    # 啟動dashboard
    minikube dashboard
    
    # 查詢dashboard url
    minikube dashboard --url

# 使用local registry
  [參考]<https://minikube.sigs.k8s.io/docs/handbook/registry/>

# 建立namespace
    kubectl create namespace=kotlindemo
  
# 建立第一個pod
  ##### 1. 先將image push 至local registry
    docker push localhost:5000/ron/kotlin/demo:v1
  ##### 2. pod.yaml
    apiVersion: v1
    kind: Pod
    metadata:
    namespace: kotlindemo
    name: test-pod 
    labels:
        app: kotlindemo
    spec:
    containers:
      - name: pod-demo
        image: localhost:5000/ron/kotlin/demo:v1
        ports: 
        - containerPort: 8080
  ##### 3. 在minikube建立pod
      kubectl create -f pod.yaml --namespace=kotlindemo
  ##### 4. 查詢執行中pods
      kubectl get pods --namesapce=kotlindemo
  
# 使用depolyment部署
  ##### 1. depolyment.yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: test-depolyment
      namespace: kotlindemo
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: kotlindemo-depolyment
      template:
        metadata:
          labels:
            app: kotlindemo-depolyment
        spec:
          containers:
          - name: pod-demo
            image: localhost:5000/ron/kotlin/demo:v1
            ports:
            - containerPort: 8080
  ##### 2. 在minikube建立depolyment
      kubectl create -f depolyment.yaml --namespace=kotlindemo 
  ##### 3. 可以使用get depolyments來查看已建立的depolyments資訊
      kubectl get depolyments --namespace=kotlindemo

# 使用service(NodePort)存取pod
  ##### 1. service.yaml
    apiVersion: v1
    kind: Service
    metadata:
      name: test-service
      namespace: kotlindemo
    spec:
      type: NodePort
      ports:
      - port: 8080
        protocol: TCP
        targetPort: 8080
      selector:
        app: kotlindemo-depolyment
  ##### 2. 在minikube建立service
      kubectl create -f service.yaml --namespace=kotlindemo
  ##### 3. 可以使用get svc來查看已建立的service資訊
      kubectl get svc --namespace=kotlindemo
  ##### 4. 可以使用minkube service來取的連接到該service的url(由於我使用driver=docker的模式)，minikube會幫我建立tunnel來連接。
      minkube service test-service --namespace=kotlindemo --url
  ##### 5. 可以使用下列指令取的local對應到的docker中執行的minikube建立的service的port號。
      ps -ef | grep docker@127.0.0.1

# 使用secret儲存ssl certificate
  ##### 1. secret.yaml
    apiVersion: v1
    data: 
      tls.crt: { "base64 encoded cert" }
      tls.key: { "base64 encoded key" } 
    kind: Secret
    metadata: 
      name: test-tls
      namespace: kotlindemo
    type: kubernetes.io/tls
  ##### 2. 在minikube建立secret
      kubectl create  -f secret.yaml --namespace=kotlindemo
  ##### 3. 可以使用get secret來查看已建立的service資訊
      kubectl get secret --namespace=kotlindemo

# 啟用minikube ingress controller
  ##### 1. 啟用ingress controller
    minikube addons enable ingress
  ##### 2. 驗證nginx ingress controller運行狀況
    kubectl get pods -n ingress-nginx


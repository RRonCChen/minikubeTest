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
    docker push localhost:50000/ron/kotlin/demo:v1
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
      
  
一、下载Istio【1.8.1】

    链接: https://pan.baidu.com/s/1OJD-5_Eq2zO0EK1hgKArHQ  密码: qwab
    
二、解压完成切换到当前目录下设置环境变量
     
    cd ./istio-1.8.1
    
    export PATH=$PWD/bin:$PATH
   
三、安装Istio

    istioctl install --set profile=demo -y
    
四、设置空间名称

    kubectl label namespace default istio-injection=enabled
    
五、部署样本服务实例

    kubectl apply -f ./samples/bookinfo/platform/kube/bookinfo.yaml
    
六、查看Service和Pods是否创建完成

    kubectl get services
    
    kubectl get pods
    
七、验证页面标题是否正常

    kubectl exec "$(kubectl get pod -l app=ratings -o jsonpath='{.items[0].metadata.name}')" -c ratings -- curl -s productpage:9080/productpage | grep -o "<title>.*</title>"
    
八、创建网关验证服务配置

    kubectl apply -f ./samples/bookinfo/networking/bookinfo-gateway.yaml
    
    istioctl analyze
    
    kubectl get svc istio-ingressgateway -n istio-system
    
九、创建IP和端口
 
    export INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
    
    export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')
    
    export SECURE_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].port}')
    
    本地主机名需要重新设置：export INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].hostname}')
    
十、设置网关地址

    export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT
    
    echo "$GATEWAY_URL"
    
    样本服务实例访问页面地址：echo "http://$GATEWAY_URL/productpage"

    
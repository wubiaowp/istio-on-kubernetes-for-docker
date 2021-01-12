一、执行监控安装

    kubectl apply -f ./samples/addons
    
二、查看安装状态
 
    kubectl rollout status deployment/kiali -n istio-system
    
三、启动控制面板
 
    istioctl dashboard kiali
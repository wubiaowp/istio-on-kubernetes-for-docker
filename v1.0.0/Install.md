一、下载地址

    https://github.com/istio/istio/releases
    
二、配置环境变量验证安装

    istioctl version
    
三、切换到解压目录下

    kubectl apply -f ./install/kubernetes/helm/istio/templates/crds.yaml
    
    kubectl apply -f ./install/kubernetes/istio-demo.yaml【镜像地址要替换成远程能下载的地址】
    
四、查看安装是否完成

    kubectl get svc -n istio-system
    
    kubectl get pod -n istio-system
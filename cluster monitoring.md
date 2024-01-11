# docs

  https://kubernetes.github.io/ingress-nginx/deploy/
  
  
  https://kubernetes.github.io/ingress-nginx/examples/auth/basic/
  
  
  https://k3s.io/
  
  
  https://helm.sh/docs/intro/install/
  
   ## Grafana installation using helm 

   
    helm repo add grafana https://grafana.github.io/helm-charts
    
    
    helm repo update
    
    
    helm install grafana grafana/grafana


   ## prometheus installation using helm
    
    
    helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
    
    
    helm repo update
    
    
    helm install prometheus prometheus-community/prometheus
    
    
    helm repo add grafana https://grafana.github.io/helm-charts
    
    
    helm repo update


  ## loki setup

    
    
    helm upgrade --install loki grafana/loki-stack
    
    
    helm repo add bitnami https://charts.bitnami.com/bitnami
    
    
    helm install thanos bitnami/thanos (edited) 


  ##  To get the password of grafana

     kubectl get secret grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo

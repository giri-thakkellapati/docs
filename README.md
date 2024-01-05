# docs

  https://kubernetes.github.io/ingress-nginx/deploy/
  
  
  https://kubernetes.github.io/ingress-nginx/examples/auth/basic/
  
  
  https://k3s.io/
  
  
  https://helm.sh/docs/intro/install/
  
    
    helm repo add grafana https://grafana.github.io/helm-charts
    
    
    helm repo update
    
    
    helm install grafana grafana/grafana
    
    
    helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
    
    
    helm repo update
    
    
    helm install prometheus prometheus-community/prometheus
    
    
    helm repo add grafana https://grafana.github.io/helm-charts
    
    
    helm repo update
    
    
    helm upgrade --install loki grafana/loki-stack
    
    
    helm repo add bitnami https://charts.bitnami.com/bitnami
    
    
    helm install thanos bitnami/thanos (edited) 

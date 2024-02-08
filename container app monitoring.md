   ## container application monitoring
   
   
   ### First we need to create pvc
### pvc.yaml

    
 ```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: beatsapi-logs
  namespace: sy-beatscoreapi  
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```
	      
### Then we need to create configmap file for loki
### Add the loki endpoint in configmap.
### add the logpath and file name as *.log

    
    
### configmap.yaml

```     
apiVersion: v1
kind: ConfigMap
metadata:
  name: promtail-sidecar-config-map
data:
  promtail.yaml: |
      server:
	http_listen_port: 9080
	grpc_listen_port: 0
	log_level: "debug"
      positions:
	filename: /tmp/positions.yaml
      clients: # Specify target
	- url: http://${var.username}:${var.password}@loki.${var.ingressIP}.nip.io/loki/api/v1/push
      scrape_configs:
	- job_name: node-syslog
	  static_configs:
	  - targets:
	    - localhost
	    labels:
	      job: hello-observability
	      __path__: /tmp/hello-observability.log
	  - targets:
	    - localhost
	    labels:
	      job: tomcat-access
	      __path__: /tmp/access_log.log    
```
 
		      
		      
	      
### we need to edit the deployment file of application
### Added the promtail deployment in the same application deployment.
### Then created the volume mounts

    
    
    
    
### deployment.yaml
    
```        
apiVersion: apps/v1
kind: Deployment
metadata:
  name: demo-app-deployment
  labels:
    app: demo-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: demo-app
  template:
    metadata:
      labels:
	app: demo-app
    spec:
      containers:
	- name: demo-app-container
	  image: harish0717/oltpspringboot:v1
	  ports:
	    - containerPort: 8080
	  env:
	    - name: JAVA_TOOL_OPTIONS
	      value: -javaagent:./opentelemetry-javaagent.jar
	    - name: OTEL_EXPORTER_OTLP_TRACES_ENDPOINT
	      value: http://tempo.monitoring.svc.cluster.local:4317
	    - name: OTEL_SERVICE_NAME
	      value: hello-observability
	  volumeMounts:
	    - name: shared-logs # shared space
	      mountPath: /tmp

	- name: promtail
	  image: grafana/promtail:latest
	  args:
	  - "--config.file=/etc/promtail/promtail.yaml"
	  ports:
	  - containerPort: 9080
	  volumeMounts:
	    - name: config
	      mountPath: /etc/promtail
	    - name: shared-logs # shared space
	      mountPath: /tmp
      volumes:
      - name: config
	configMap:
	  name: promtail-sidecar-config-map
      - name: shared-logs
	persistentVolumeClaim:
	  claimName: beatsapi-logs




---


apiVersion: v1
kind: Service
metadata:
  name: demo-app-service
spec:
  selector:
    app: demo-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: ClusterIP

---

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: demo-app-ingress
spec:
  ingressClassName: nginx
  rules:
    - host:  52.140.84.170.nip.io
      http:
	paths:
	  - path: /
	    pathType: Prefix
	    backend:
	      service:
		name: demo-app-service
		port:
		  number: 80
```	
	
		
		
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
	    
      

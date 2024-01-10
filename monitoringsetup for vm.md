# set-up prometheus in local vm
### installing prometheus

     sudo apt update -y
     
     curl -LO https://github.com/prometheus/prometheus/releases/download/v2.38.0/prometheus-2.38.0.linux-amd64.tar.gz
     
     tar xvfz prometheus-*.tar.gz
     
     tar -xvf prometheus-2.38.0.linux-amd64.tar.gz
     
     mv prometheus-2.38.0.linux-amd64 prometheus-files
     
     sudo useradd --no-create-home --shell /bin/false prometheus
     
     sudo mkdir /etc/prometheus
     
     cd
     
     sudo mkdir /var/lib/prometheus
     
     sudo chown prometheus:prometheus /etc/prometheus
     
     sudo chown prometheus:prometheus /var/lib/prometheus
     
  ### Copy Prometheus and promtool binary from Prometheus-files folder to /usr/local/bin and change the ownership to Prometheus user

         sudo cp prometheus-files/prometheus /usr/local/bin/
        
         sudo cp prometheus-files/promtool /usr/local/bin/
        
         sudo chown prometheus:prometheus /usr/local/bin/prometheus
        
         sudo chown prometheus:prometheus /usr/local/bin/promtool 

 ### Move the consoles and console_libraries directories from Prometheus-files to /etc/Prometheus folder and change the ownership to Prometheus user.

       sudo cp -r prometheus-files/consoles /etc/prometheus
      
       sudo cp -r prometheus-files/console_libraries /etc/prometheus
      
       sudo chown -R prometheus:prometheus /etc/prometheus/consoles
      
       sudo chown -R prometheus:prometheus /etc/prometheus/console_libraries
      


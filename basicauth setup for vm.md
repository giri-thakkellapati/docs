# First install nginx in vm

    cd /etc/nginx

# on ubuntu
    sudo apt install apache2-utils

# on centos
    sudo yum install httpd-tools

    htpasswd -c /etc/nginx/.htpasswd admin

# we need to add the ip of prometheus or loki

    
        sudo nano /etc/nginx/siteserver {
        ...
    
        #additional authentication properties
        auth_basic  "Protected Area";
        auth_basic_user_file /etc/nginx/.htpasswd;
    
        location / {
            proxy_pass           http://localhost:9090/;
        }
    
        ...
    }s-enabled/prometheus

# Save and test the new configuration

    nginx -t

# Restart Nginx

      sudo service nginx restart
      
      sudo service nginx status

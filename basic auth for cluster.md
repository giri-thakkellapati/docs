# basic auth

    htpasswd -c auth foo
    New password: <bar>
    New password:
    Re-type new password:
    Adding password for user foo

# kubectl create secret generic basic-auth --from-file=auth



# kubectl get secret basic-auth -o yaml

# we need to change annotations in ingress


      apiVersion: networking.k8s.io/v1
      kind: Ingress
      metadata:
        name: ingress-with-auth
        annotations:
          # type of authentication
          nginx.ingress.kubernetes.io/auth-type: basic
          # name of the secret that contains the user/password definitions
          nginx.ingress.kubernetes.io/auth-secret: basic-auth
          # message to display with an appropriate context why the authentication is required
          nginx.ingress.kubernetes.io/auth-realm: 'Authentication Required - foo'
      spec:
        ingressClassName: nginx
        rules:
        - host: foo.bar.com
          http:
            paths:
            - path: /
              pathType: Prefix
              backend:
                service: 
                  name: http-svc
                  port: 
                    number: 80

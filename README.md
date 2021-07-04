# pitomi
Launch your own private hitomi la mirror

## components
- backend: https://github.com/project-pitomi/pitomi-backend
- frontend: https://github.com/project-pitomi/pitomi-frontend
- batch: https://github.com/project-pitomi/pitomi-batch

## requirement
- kubernetes
- azure storage account(or maybe [azurite](https://github.com/Azure/Azurite))
- mongodb

## Images
### mobile
![mobile512 02](https://user-images.githubusercontent.com/5654978/124376198-c15fd400-dce0-11eb-8973-705e4d4cc23e.png)
![mobile512 01](https://user-images.githubusercontent.com/5654978/124376203-c45ac480-dce0-11eb-875f-ac8305ade5f4.png)


### desktop
![desktop1024 01](https://user-images.githubusercontent.com/5654978/124376049-03d4e100-dce0-11eb-9f4e-eb056017f84d.png)


## k8s ingress example
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: pitomi
  namespace: hitomi
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/use-regex: "true"
    nginx.ingress.kubernetes.io/rewrite-target: /$1
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
spec:
  tls:
  - hosts:
    - <<host name>>
    secretName: <<tls secret>>
  rules:
  - host: <<host name>>
    http:
      paths:
      - path: /(graphql$)
        pathType: Prefix
        backend:
          service:
            name: pitomi-backend
            port:
              number: 4000
      - path: /(.*)
        pathType: Prefix
        backend:
          service:
            name: pitomi-frontend
            port:
              number: 3000
```

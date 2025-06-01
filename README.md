# README

helm install kube-support .
helm upgrade kube-support .

# TO BE USED
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: d4display-fedon
spec:
  selector:
    matchLabels:
      app: d4display-fedon
  replicas: 1
  template:
    metadata:
      labels:
        app: d4display-fedon
    spec:
      containers:
        - name: d4display-fedon
          image: europe-west2-docker.pkg.dev/d4dist-prd/d4dist-prd-gar-repo/d4display-fedon:latest
          ports:
            - containerPort: 80
      imagePullSecrets:
        - name: gcp-artifact-registry
---
apiVersion: v1
kind: Service
metadata:
  name: d4display-fedon
spec:
  type: ClusterIP
  ports:
    - port: 3000
      targetPort: 3000
  selector:
    app: d4display-fedon
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: d4display-fedon
spec:
  rules:
    - http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: d4display-fedon
                port:
                  number: 3000
```




apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: argocd
spec:
  rules:
    - http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: argocd
                port:
                  number: 80











helm repo add grafana https://grafana.github.io/helm-charts
helm install grafana

helm install my-grafana grafana/grafana --namespace monitoring




kubectl port-forward traefik-97b44b794-bsvjn -n kube-system 9000:9000



###############3
   kubectl -n fedon create secret docker-registry gcp-artifact-registry \
       --docker-server https://europe-west2-docker.pkg.dev \
       --docker-username _json_key \
       --docker-password "$(cat key.json)" \
       --docker-email d4dist-prd-sa-fedon@d4dist-prd.iam.gserviceaccount.com





apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: NodePort
  selector:
    app: web
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
      nodePort: 30007

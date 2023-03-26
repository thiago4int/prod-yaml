# Criar ip
gcloud compute addresses create sgeap-frontend-homol --global
gcloud compute addresses create sgeap-frontend-prod --global

# Files
backend-config.yaml
deployment.yaml
front-config.yaml
ingress.yaml
service.yaml

kubectl apply -f deployment.yaml -n default
kubectl apply -f backend-config.yaml -n default
kubectl apply -f service.yaml -n default
kubectl apply -f front-config.yaml -n default
kubectl apply -f ingress.yaml -n default
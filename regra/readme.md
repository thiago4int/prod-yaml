# Comandos para subir
kubectl create namespace homol (Se ainda não tiver)

kubectl create serviceaccount regra-service-account \
    --namespace homol

gcloud iam service-accounts create regra-service-account

gcloud iam service-accounts add-iam-policy-binding regra-service-account@cearaprev-sgeap-prod.iam.gserviceaccount.com \
    --role roles/iam.workloadIdentityUser \
    --member "serviceAccount:cearaprev-sgeap-prod.svc.id.goog[homol/regra-service-account]"

kubectl annotate serviceaccount regra-service-account \
--namespace homol \
iam.gke.io/gcp-service-account=regra-service-account@cearaprev-sgeap-prod.iam.gserviceaccount.com

# Definir cluster
gcloud container clusters get-credentials cearaprev-sgeap-cluster-prod --region southamerica-east1 --project cearaprev-sgeap-prod

# Criar ip
gcloud compute addresses create sgeap-regra-prod --global

# Files
backend-config.yaml
deployment.yaml
front-config.yaml
ingress.yaml
service.yaml

kubectl apply -f deployment.yaml (Linhas 5 e 6 comentadas)
kubectl apply -f backend-config.yaml
kubectl apply -f service.yaml
kubectl apply -f front-config.yaml
kubectl apply -f ingress.yaml
# Comandos para subir
kubectl create namespace homol (Se ainda não tiver)

kubectl create serviceaccount assinatura-digital-homol \
    --namespace homol

gcloud iam service-accounts create assinatura-digital-homol

gcloud iam service-accounts add-iam-policy-binding assinatura-digital-homol@cearaprev-sgeap-prod.iam.gserviceaccount.com \
    --role roles/iam.workloadIdentityUser \
    --member "serviceAccount:cearaprev-sgeap-prod.svc.id.goog[homol/assinatura-digital-homol]"

kubectl annotate serviceaccount assinatura-digital-homol \
--namespace homol \
iam.gke.io/gcp-service-account=assinatura-digital-homol@cearaprev-sgeap-prod.iam.gserviceaccount.com

# Definir cluster
gcloud container clusters get-credentials cearaprev-sgeap-cluster-prod --zone southamerica-east1-c --project cearaprev-sgeap-prod

# Criar ip
gcloud compute addresses create sgeap-assinatura-digital-homol --global

# Files
backend-config.yaml
deployment.yaml
front-config.yaml
ingress.yaml
service.yaml

kubectl apply -f deployment.yaml -n homol
kubectl apply -f backend-config.yaml -n homol
kubectl apply -f service.yaml -n homol
kubectl apply -f front-config.yaml -n homol
kubectl apply -f ingress.yaml -n homol
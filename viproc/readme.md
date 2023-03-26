# Comandos para subir
kubectl create namespace homol (Se ainda não tiver)

kubectl create serviceaccount viproc-homol \
    --namespace homol

gcloud iam service-accounts create viproc-homol

gcloud iam service-accounts add-iam-policy-binding viproc-homol@cearaprev-sgeap-prod.iam.gserviceaccount.com \
    --role roles/iam.workloadIdentityUser \
    --member "serviceAccount:cearaprev-sgeap-prod.svc.id.goog[homol/viproc-homol]"

kubectl annotate serviceaccount viproc-homol \
--namespace homol \
iam.gke.io/gcp-service-account=viproc-homol@cearaprev-sgeap-prod.iam.gserviceaccount.com

# Definir cluster
gcloud container clusters get-credentials cearaprev-sgeap-cluster-prod --zone southamerica-east1-c --project cearaprev-sgeap-prod

# Criar ip
gcloud compute addresses create sgeap-viproc-prod --global

# Files
backend-config.yaml
deployment.yaml
front-config.yaml
ingress.yaml
service.yaml

kubectl apply -f deployment.yaml
kubectl apply -f backend-config.yaml
kubectl apply -f service.yaml
kubectl apply -f front-config.yaml
kubectl apply -f ingress.yaml
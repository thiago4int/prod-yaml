# Comandos para subir
kubectl create namespace homol (Se ainda não tiver)

kubectl create serviceaccount cadastro-homol \
    --namespace homol

kubectl create secret generic cadastro \
  --from-literal=username=postgres \
  --from-literal=password="CkEAI4jli13qeqHI" \
  --from-literal=database=sgeap_homol

gcloud iam service-accounts create cadastro-homol

gcloud iam service-accounts add-iam-policy-binding cadastro-homol@cearaprev-sgeap-prod.iam.gserviceaccount.com \
    --role roles/iam.workloadIdentityUser \
    --member "serviceAccount:cearaprev-sgeap-prod.svc.id.goog[homol/cadastro-homol]"

kubectl annotate serviceaccount cadastro-homol \
--namespace homol \
iam.gke.io/gcp-service-account=cadastro-homol@cearaprev-sgeap-prod.iam.gserviceaccount.com

Definir 'Cloud SQL Client' para conta cadastro-homol@cearaprev-sgeap-prod.iam.gserviceaccount.com em 'IAM & Admin -> Service Account'

# Definir cluster
gcloud container clusters get-credentials cearaprev-sgeap-cluster-prod --zone southamerica-east1-c --project cearaprev-sgeap-prod


# Criar ip
gcloud compute addresses create sgeap-cadastro-homol --global
gcloud compute addresses create sgeap-cadastro-prod --global


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
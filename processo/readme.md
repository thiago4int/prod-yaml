# Comandos para subir
kubectl create namespace homol (Se ainda não tiver)

kubectl create serviceaccount processo-homol \
    --namespace homol

kubectl create secret generic processo \
  --from-literal=username=postgres \
  --from-literal=password="CkEAI4jli13qeqHI" \
  --from-literal=database=sgeap_homol

gcloud iam service-accounts create processo-homol

gcloud iam service-accounts add-iam-policy-binding processo-homol@cearaprev-sgeap-prod.iam.gserviceaccount.com \
    --role roles/iam.workloadIdentityUser \
    --member "serviceAccount:cearaprev-sgeap-prod.svc.id.goog[homol/processo-homol]"

kubectl annotate serviceaccount processo-homol \
--namespace homol \
iam.gke.io/gcp-service-account=processo-homol@cearaprev-sgeap-prod.iam.gserviceaccount.com

Definir 'Cloud SQL Client' para conta processo-homol@cearaprev-sgeap-prod.iam.gserviceaccount.com em 'IAM & Admin -> Service Account'

# Definir cluster
gcloud container clusters get-credentials cearaprev-sgeap-cluster-prod --zone southamerica-east1-c --project cearaprev-sgeap-prod


# Criar ip
gcloud compute addresses create sgeap-processo-prod --global

# Files
backend-config.yaml
deployment.yaml
front-config.yaml
ingress.yaml
service.yaml

kubectl apply -f deployment.yaml -n homol 		(Linhas 5 e 6 comentadas)
kubectl apply -f backend-config.yaml -n homol
kubectl apply -f service.yaml -n homol 			(Linhas 5 e 6 comentadas)
kubectl apply -f front-config.yaml -n homol
kubectl apply -f ingress.yaml -n homol
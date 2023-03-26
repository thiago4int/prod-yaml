# Comandos para subir
kubectl create namespace homol (Se ainda não tiver)

kubectl create serviceaccount documento-homol \
    --namespace homol

kubectl create secret generic documento \
  --from-literal=username=postgres \
  --from-literal=password="CkEAI4jli13qeqHI" \
  --from-literal=database=sgeap_homol

gcloud iam service-accounts create documento-homol

gcloud iam service-accounts add-iam-policy-binding documento-homol@cearaprev-sgeap-prod.iam.gserviceaccount.com \
    --role roles/iam.workloadIdentityUser \
    --member "serviceAccount:cearaprev-sgeap-prod.svc.id.goog[homol/documento-homol]"

kubectl annotate serviceaccount documento-homol \
--namespace homol \
iam.gke.io/gcp-service-account=documento-homol@cearaprev-sgeap-prod.iam.gserviceaccount.com

Definir 'Cloud SQL Client' para conta documento-homol@cearaprev-sgeap-prod.iam.gserviceaccount.com em 'IAM & Admin -> Service Account'

# Definir cluster
gcloud container clusters get-credentials cearaprev-sgeap-cluster-prod --region southamerica-east1 --project cearaprev-sgeap-prod


# Criar ip
gcloud compute addresses create sgeap-documento-homol --global
gcloud compute addresses create sgeap-documento-prod --global

# Files
kubectl apply -f deployment.yaml
kubectl apply -f backend-config.yaml
kubectl apply -f front-config.yaml
kubectl apply -f service.yaml
kubectl apply -f ingress.yaml

# Deploy
Fazer deploy do Service e Deployment:
```bash
kubectl apply -f deployment.yaml -n homol 	
```

Fazer deploy do backend:
```bash
kubectl apply -f backend-config.yaml -n homol
```

Deploy do service com configuração do backend:
```bash
kubectl apply -f service.yaml -n homol 	
```
	
Deploy da configuração do frontend:
```bash	
kubectl apply -f front-config.yaml -n homol 	
```
	
Deploy do ingress:
```bash	
kubectl apply -f ingress.yaml -n homol	
```

Após o comando o ingress, esperar receber o ip e depois ir no load balance do serviço, editar com configurações de HTTPS e IP que foi criado pelo comando de criação de IP.
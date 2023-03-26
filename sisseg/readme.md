<<<<<<< HEAD
kubectl create secret generic sisseg \
  --from-literal=username=postgres \
  --from-literal=password="CkEAI4jli13qeqHI" \
  --from-literal=database=sgeap_homol



gcloud compute addresses create sgeap-sisseg-homol --global
gcloud compute addresses create sgeap-sisseg-prod --global
# Learn GKE
Content mainly from https://learn.hashicorp.com/tutorials/terraform/gke?in=terraform/kubernetes
## Start GKE cluster
```
cat > terraform.tfvars << ENDOFFILE
project_id = "<PROJECT_ID>"
region     = "<REGION>"
ENDOFFILE 
export GOOGLE_CREDENTIALS=$(pwd)/terraform.json
terraform apply

gcloud container clusters get-credentials $(terraform output kubernetes_cluster_name) --region $(terraform output region)
```
## Deploy hello-world
kubectl create deployment hello-app --image=gcr.io/google-samples/hello-app:1.0 --port=8080

## Deply kubernetes dashboard
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta8/aio/deploy/recommended.yaml
kubectl proxy
http://127.0.0.1:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/

### Get token
kubectl apply -f https://raw.githubusercontent.com/hashicorp/learn-terraform-provision-gke-cluster/master/kubernetes-dashboard-admin.rbac.yaml
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep service-controller-token | awk '{print $1}')
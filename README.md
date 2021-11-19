# aks-azure-functions
```
cd aksfunc
func kubernetes deploy --name aksfunc --registry $REGISTRYNAME.azurecr.io
```
```
secret/aksfunc unchanged
secret/func-keys-kube-secret-aksfunc configured
serviceaccount/aksfunc-function-keys-identity-svc-act created
role.rbac.authorization.k8s.io/functions-keys-manager-role created
rolebinding.rbac.authorization.k8s.io/aksfunc-function-keys-identity-svc-act-functions-keys-manager-rolebinding created
service/aksfunc-http created
deployment.apps/aksfunc-http created
Waiting for deployment "aksfunc-http" rollout to finish: 0 of 1 updated replicas are available...
```
```
export IP=$(kubectl get services aksfunc-http -o json | jq -r '.status.loadBalancer.ingress[0].ip')
export KEY=$(kubectl get secrets func-keys-kube-secret-aksfunc -o json | jq -r '.data."functions.httptrigger.default"' | base64 -d)
curl http://$IP/api/httptrigger?code=$KEY
```

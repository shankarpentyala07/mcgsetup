1. Create a service Account
Example: `oc create serviceaccount astra-sa-login -n default`

2. Assign cluster-admin role: 
Example:
```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: astracontrol-admin
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: astra-sa-login
  namespace: default`
```
create the above file: `oc apply -f <crb>`
3. Create Service Account token secret:

```
apiVersion: v1
kind: Secret
metadata:
  name: secret-sa-sample
  annotations:
    kubernetes.io/service-account.name: "astra-sa-login" 
type: kubernetes.io/service-account-token 
```
create the above file: `oc apply -f <secret>`

4. After creating a new ServiceAccount token secret, you can edit the ServiceAccount and append it to the list of mountable secrets. 

Example:

oc edit sa astra-sa-login

```
kind: ServiceAccount
apiVersion: v1
metadata:
  name: sa-sample
  namespace: sample
secrets:
  - name: secret-sa-sample
  - name: sa-sample-dockercfg-bw2bq
imagePullSecrets:
  - name: sa-sample-dockercfg-bw2bq
```

5. Download the ServiceAccount kubeconfig file by clicking on Download kubeconfig file via OpenShift Web Console in users -> Service Account

Reference: https://access.redhat.com/solutions/6992422

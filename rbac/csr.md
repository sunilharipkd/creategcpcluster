**1. Create CSR**

```

# Create the key for the user using openssl
openssl genrsa -out ram.key 2048 

# Create a CSR from the key:
openssl req -new -key ram.key -out ram.csr

```
**CSR Sample file**

Add the base64 encoded value of the csr into the yaml and create a request.

```
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: ram
spec:
  groups:
  - system:authenticated
  request: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJQ21EQ0NBWUFDQVFBd1V6RUxNQWtHQTFVRUJoTUNRVlV4RXpBUkJnTlZCQWdNQ2xOdmJXVXRVM1JoZEdVeApJVEFmQmdOVkJBb01HRWx1ZEdWeWJtVjBJRmRwWkdkcGRITWdVSFI1SUV4MFpERU1NQW9HQTFVRUF3d0RjbUZ0Ck1JSUJJakFOQmdrcWhraUc5dzBCQVFFRkFBT0NBUThBTUlJQkNnS0NBUUVBMzlyWkhSc252SVJSU2VydXJ0ZisKRXVCTUc2WGJZSUVCSmk4OHI2QkgvR3hjUnU2cXRSdVo0dW9OdlNuWHZaYjFIYmkyakw0Nmw3SEIxYUtNOU5vVQp0S1lKRm9hRFEzY2ljamVnQ2tkWTg4ZHpSRWkyek83MHJySnNnNlJ6b1hja3lUdDNUZzMvQWwvdy9NU1p3UGVBCnVEaEd6MU1uNUhTeHAva2JTU1lIaDB1NHdtVklXem9zVE4rbDE5bi83dVdLU0pGNHF3WGF6Zm5WRHJ2cDh0N2sKY2hyYnVnSjAzc1AwUU9yelZra0srcTRIeHpOVTNWMWxGOS9XbEhDMkZTUE1xdTE4ZzlwY0lLMUlxUkVZMFFJNApUeGV1Y0xaNWZrQzhNK1ZQci83MHp0Z3Zpdk9SU0h4UTgzcGRCZ1ZkNVF1eTNPa29RSzBIWWlGWVc1T3MwdzVFCkJ3SURBUUFCb0FBd0RRWUpLb1pJaHZjTkFRRUxCUUFEZ2dFQkFFVUxoU3JReCtJN2Mvc0FFNE9VdStxSXlVMjYKaDdmRE1sWmlScE9Fc2lFVzNZS0ZMaFVOZ2oyclFNQ3YyUHNMNCtxWVVCbm93SU9aRHQ3aldSVzVBOUxCcTV2ZQpYQzAwU2ZKeFVHQStPS0Iwb20vZ2dwWFNVNnNqVnNVTVBqN1FaeDhQdDl4KzNCNHFOMFVtcTY2eFhONS9xczVyClVzdkl2dmdNM21DNWE2N1hXbEFyYWt5TENNeVhYNkJxeGpRTW1GUEJJeTB3YWNPVGFmMytNMUR2TDJRTEpKd3QKS3VkbCsrL21GSnpLREluTXdmbG0vU3hlRXRzL2psSkZSZ0RoNXRmN1pCbC9GQmVBYWNRRnJYVXhBdHJFeVoxTApQRXZoMEtxWUppYmV4YlZaK0EzR0NoakxIT213ZXFjY2JBOXNaRXdLU1ZiMFJtaCtpcklKQnhPM3J3OD0KLS0tLS1FTkQgQ0VSVElGSUNBVEUgUkVRVUVTVC0tLS0tCg==
  signerName: kubernetes.io/kube-apiserver-client
  expirationSeconds: 86400  # one day
  usages:
  - client auth
```
Check the csr and it would be pending for signing.
```
root@cks-master:~/csr# kubectl get csr
NAME   AGE   SIGNERNAME                            REQUESTOR          REQUESTEDDURATION   CONDITION
ram    20s   kubernetes.io/kube-apiserver-client   kubernetes-admin   24h                 Pending
```


**2. Sign the CSR**

```
kubectl certificate approve ram
certificatesigningrequest.certificates.k8s.io/ram approved

kubectl get csr
NAME   AGE    SIGNERNAME                            REQUESTOR          REQUESTEDDURATION   CONDITION
ram    107s   kubernetes.io/kube-apiserver-client   kubernetes-admin   24h                 Approved,Issued

```

`kubectl get csr -o yaml` This would help you to identify the signed certificate in the status section. 

![image](https://user-images.githubusercontent.com/111420932/236674158-6b3161f6-b4b3-4b1f-a296-85b6072c089b.png)

`echo <certificate> | base64 -d `

Create certificate file using the vim editor. 

**3. Using the certificate and key to connect to the K8s API**

1. Set credentials 

```
 k config set-credentials ram --client-key=ram.key --client-certificate=ram.crt
 
 ```
 
 Validate kubeconfig view 
 
 ```
 k config view 
 ```
 
 
 ![image](https://user-images.githubusercontent.com/111420932/236675301-732098d5-a798-4e8d-8b0f-99f275e18c5a.png)

Here the certificate and keys are referred to as files, if we want them to be added to the kubeconfig file and mask them, then use the --embed-certs option.

```
k config set-credentials ram --client-key=ram.key --client-certificate=ram.crt --embed-certs
```

Output would look like:

![image](https://user-images.githubusercontent.com/111420932/236675440-f4fa35f6-8e06-4de5-99d9-a27a712b82a0.png)

Set a new context to the user .
```
root@cks-master:~/csr# k config set-context ram --user=ram --cluster=kubernetes
Context "ram" created.


root@cks-master:~/csr# k config get-contexts
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin
          ram                           kubernetes   ram
```
Use the context ram to check the privilege.

```
root@cks-master:~/csr# k config use-context ram
Switched to context "ram".
root@cks-master:~/csr# k get ns
Error from server (Forbidden): namespaces is forbidden: User "ram" cannot list resource "namespaces" in API group "" at the cluster scope
root@cks-master:~/csr# k get pods
Error from server (Forbidden): pods is forbidden: User "ram" cannot list resource "pods" in API group "" in the namespace "default"
root@cks-master:~/csr# k get secrets
Error from server (Forbidden): secrets is forbidden: User "ram" cannot list resource "secrets" in API group "" in the namespace "default"
root@cks-master:~/csr# k get secrets -n india
No resources found in india namespace.
root@cks-master:~/csr#
```


## TAKEAWAY: Created the below resources.
1. Created users ram,dave 
2. Created roles,clusterroles,rolebinding to make the least privilege applied.
3. Created a key for ram, created a CSR for ram and send it to API, signed CSR, created a crt.
4. Using the crt and the key set the credentials for the kubeconfig. 
5. Set-the context for the user ram and used that context.
6. Lastly, tried to access resources in the cluster within the context to validate the RBAC.

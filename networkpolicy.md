Implemented by the network plugins CNI *Calico/Weave*
Namespace Level
Restrict the ingress/egress for a group of pods based on certain rules and conditions
PODs can communicate with any PODs without any network policy. 
Possible restrictions:
  - IP Block
  - Namespace Selector
  - Pod Selector

Code Samples:

1. This networkpolicy is applied to all pods that has a label "tier: frontend" and in the namespace "blue".

```
kind: networkPolicy
metadata:
  name: policy-pod1
  namespace: blue
spec:
  podSelector:
    matchLabels:
      tier: frontend

```
2. The below one denies all outgoing traffic from pods with label "tier: frontend"


```
kind: networkPolicy
metadata:
  name: policy-pod1
  namespace: blue
spec:
  podSelector:
    matchLabels:
      tier: frontend
  policyTypes:
  - Egress
```

3. Allow outgoing Traffic
   - to allow traffic to pods within namespace red and port 6379
   - to allow traffic to pods with label "tier: backend" within same namespace ie) blue.

```
kind: networkPolicy
metadata:
  name: policy-pod1
  namespace: blue
spec:
  podSelector:
    matchLabels:
      tier: frontend
  policyTypes:
  - Egress
  egress:
  - to: 
    - namespaceSelector:
        matchLabels:
          name: red
    ports:
    - protocol: TCP
      port: 6379
  - to:
    - podSelector:
        matchLabels:
          tier: backend
```

4. Default deny traffic for all pods 

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny
  namespace: default
spec:
  podSelector: {}
  policyTypes:
    - Ingress
    - Egress
```

5. Allow traffic from frontend to backend pods considering they are in the same namespace.

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: frontend-egress
  namespace: default
spec:
  podSelector:
    matchLabels:
      run: frontend
  policyTypes:
    - Egress
  egress:
    - to:
        - podSelector:
            matchLabels:
              run: backend
      ports:
        - protocol: TCP
          port: 80
```

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: backend-ingress
  namespace: default
spec:
  podSelector:
    matchLabels:
      run: backend
  policyTypes:
    - Ingress
  ingress:
    - from:
        - podSelector:
            matchLabels:
              run: frontend
      ports:
        - protocol: TCP
          port: 80
```

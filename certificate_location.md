## Certificate locations in Kubernetes

# 1. CA 
`/etc/kubernetes/pki/ca.crt`

# 2. API 
`/etc/kubernetes/pki/apiserver.crt`

# 3. ETCD 
/etc/kubernetes/pki/etcd/server.crt

# 4. API -> ETCD
/etc/kubernetes/pki/apiserver-etcd-client.crt

# 5. API -> Kubelet
/etc/kubernetes/pki/apiserver-kubelet-client.crt

# 6. Scheduler -> API  

scheduler config file : /etc/kubernetes/scheduler.conf
In this config file the certficates exists for client-certs


# 7. ControllerManager -> API
controllermanager config file : /etc/kubernetes/controller-manager.conf
In this config file the certficates exists for client-certs

# 8. Kubelet -> API
kubelet config file : /etc/kubernetes/kubelet.conf
<img width="942" alt="image" src="https://user-images.githubusercontent.com/111420932/236202590-4562c2c9-b9a9-44d7-aa70-a251637261e8.png">

# 9. Kubelet server certificate
`
Cert path: /var/lib/kubelet/pki 
File Name: kubelet.crt
`



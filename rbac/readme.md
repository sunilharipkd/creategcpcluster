# What is RBAC ? 
Regulating access to compute or network resources based on the roles of the individual users within the organization.

In kube-api server configuration, the key '--authorization-mode=RBAC' lets you control the cluster access using the RBAC .In Kubeadm it is enabled by default. 

Note:
1. There is no deny rule in the RBAC, We whitelist what needs to be allowed. 
2. It uses roles and rolebindings to assign the permissions to the service accounts or users
3. Based on the authorization binded to the users and service accounts, the actions can be performed. 

Principle of least privilege must be kept in mind while permissions are given to the users/service accounts.

Role: What are the privileges given to the namespaced resource for a specific namespace.
ClusterRole: What are the privileges given to the resources for the whole cluster/for non-namespaced resources.

Rolebinding: Where are the permissions applied in a namespace.
Clusterrolebinding: Where are the permissions applied in the cluster in all namespaces/non-namespaced. 

`kubectl api-resources --namespaced=false`  -- List all non-namespaced kubernetes resources

`kubectl api-resources --namespaced=true`   -- List all namespaced kubernetes resources


**Important** - The ClusterRole permissions are assigned to the **current** and **future** namespaces. In case if the role needs to be restricted to only current namespaces,
then clusterrole must be binded to **rolebinding**. 

But you cannot bind a role and clusterrolebinding, as role is applicable only for one namespace.




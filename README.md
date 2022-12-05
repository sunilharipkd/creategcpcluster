# PRE-REQUISITE:

1. You must have a GCP account.

To connect the gke cluster from your local machine, install the gcloud utility into your machine and authorize to your gcp account. 
This is the best way to run the kube commands doing ssh to your master server. 

# Install gcloud sdk from
https://cloud.google.com/sdk/auth_success

# Run locally
```
gcloud auth login
gcloud projects list
gcloud config set project YOUR-PROJECT-ID
gcloud compute instances list 
```
#Create 2 VM instances

Master: Ubuntu 20.04 (must)
        Disk 50GB 
        Machine min 2CPU 4GB

Worker: Ubuntu 20.04 (must)
        Disk 50GB 
        Machine min 2CPU 4GB

Once the VM instances are created, use the command `gcloud compute ssh <instance name>` to log into the machines. 
This depends on how you need to log into the virtual machines, based on that your networking and other parameters can be adjusted

# Creation of cluster
Once logged in to the master :

```
sudo -i
bash <(curl -s https://raw.githubusercontent.com/sunilharipkd/creategkecluster/main/install_master.sh)
```

log in to the worker :

```
sudo -i
bash <(curl -s https://raw.githubusercontent.com/sunilharipkd/creategkecluster/main/install_worker.sh)
```

IMPORTANT: Once both the commands are executed, then run the printed kubeadm-join-command from the master (last line of the output) on the worker.
This will make the worker node join the kubernetes master and form the cluster. 

Validation:

1. Logout from the master
2. Log in back to the master and wait for some time to validate the cluster 
`
kubectl get nodes
`

#Open the ports 30000-40000 to access the cluster. 
Run this in the local terminal: 
`gcloud compute firewall-rules create nodeports --allow tcp:30000-40000`




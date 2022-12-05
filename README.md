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

Slave: Ubuntu 20.04 (must)
        Disk 50GB 
        Machine min 2CPU 4GB

Once the VM instances are created, use the command `gcloud compute ssh <instance name>` to log into the machines. 
This depends on how you need to log into the virtual machines, based on that your networking and other parameters can be adjusted

Once logged in to the master :

```
sudo -i
bash <(curl -s https://raw.githubusercontent.com/killer-sh/cks-course-environment/master/cluster-setup/latest/install_master.sh)




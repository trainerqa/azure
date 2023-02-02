# Create Linux VM

az vm create -n VM2 -g IaaSResources --image CentOS --admin-username baciadadmin --ssh-key-value ~/.ssh/id_rsa.pub --nsg VM2-NSG --subnet Production --vnet-name BACIAD-VNET --size Standard_D1_v2 --public-ip-address VM2-PIP 

az vm list-ip-addresses -n VM2 -g IaaSResources -o table

az vm open-port -g IaaSResources -n VM2 --port 80 --priority 100



sudo yum install httpd  git



git clone http://github.com/justindavies/baciad

cd baciad

cp * /var/www/html



service httpd start

sudo systemctl enable httpd 





# ACI

az container create -g dan -n dan-baciad --image azuredan/docrepo:baciad --ip-address=Public





# AKS

az group create -n dans-aks -l eastus

az aks create -n dans-aks -g dans-aks

az aks get-credentials -n dans-aks -g dans-aks


kubectl run baciad --image=azuredan/docrepo:baciad --replicas=1 --port=80

kubectl get po

kubectl get svc

kubectl expose deployment baciad --port=80 --type=LoadBalancer

kubectl scale  --replicas=3 deployment/baciad



# AD Join

sudo yum install realmd sssd krb5-workstation krb5-libs samba-common-tools

sudo realm discover BACIADLIVEOUTLOOK.ONMICROSOFT.COM



kinit azuread@baciadliveoutlook.onmicrosoft.com

sudo realm join --verbose BACIADLIVEOUTLOOK.ONMICROSOFT.COM -U 'azuread@ BACIADLIVEOUTLOOK.ONMICROSOFT.COM



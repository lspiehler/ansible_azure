# Example Ansible Azure Build
This playbook creates CentOS 8 VMs in Azure behind a load balancer with a public IP. It also includes a playbook to remove all created components.
## Install Prerequisites
```
yum install python3-pip
pip3 install 'ansible==2.9.15'
pip3 install 'ansible[azure]'
```

## Azure Authentication

### Method 1:
```
cat << EOF > ~/.azure/credentials
[default]
ad_user=azureuser@domain.com
password=azurepassword
subscription_id=00000000-1111-2222-3333-444444444444
EOF
```
### Method 2:
```
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc

echo -e "[azure-cli]
name=Azure CLI
baseurl=https://packages.microsoft.com/yumrepos/azure-cli
enabled=1
gpgcheck=1
gpgkey=https://packages.microsoft.com/keys/microsoft.asc" | sudo tee /etc/yum.repos.d/azure-cli.repo

sudo yum -y install azure-cli

az login
# ^ follow directions from stdout
```

```
ansible-playbook -e @vars.yml environment_create.yml -e 'ansible_python_interpreter=/usr/bin/python3'
```
# Ansible-Lab

- this is the terraform code to set up a test environment with 3 servers, 
- one of the server is the ansible master and the other 2 are nodes with environment tags dev and qa.







1- Set up the Test environment
1-a Write the infrastructure code

Here we will set up the test environment using Terraform

open git bash and move to the home directory
cd ~

Now clone the repository from GitHub 
git clone https://github.com/ityourway/ansible-using-terraform-aws.git

now cd into Ansible-Lab
cd Ansible-Lab

Now let's go ahead and launch the infrastructure.
1-b Launch the Infrastructure

To launch the infrastructure, run the commands below
terraform init
terraform plan 
terraform apply  --auto-approve







You can see the SSH command to connect to each instance

Now that we are done, let's SSH into our master server and configure Ansible, in my case I used the command highlighted in the image above.
2- Configure Ansible

Once in your master server, let's configure our Ansible working Directory

Create a directory named ansible-dev in the home directory and navigate into it.
cd ~
mkdir ansible-dev
cd ansible-dev

Create a file named inventory.yml in the home directory.
vi inventory.yml

Paste the content below into the inventory.yml file and edit the content to have your various servers' private IP addresses, note that the IP addresses were displayed in the terraform output.
[webservers]
172.31.0.230
[databaseservers]
172.31.10.184

[webservers:vars]
ansible_ssh_private_key_file=~/ansible-key.pem
ansible_user= ec2-user

[databaseservers:vars]
ansible_ssh_private_key_file=~/ansible-key.pem
ansible_user= ubuntu

    Create a file named ansible.cfg in the ansible-dev directory.

vi ansible.cfg

    Paste the content below into ansible.cfg file.

[defaults]
host_key_checking = False
inventory=inventory.yml
interpreter_python=auto_silent
localhost_warning=false
deprecation_warnings = False

Validate and check the inventory.
ansible-inventory --graph

Run the Ansible commands to access the servers in the 2 groups:
ansible all -m ping 

You should get the output below if everything is configured right.

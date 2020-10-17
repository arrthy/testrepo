# Mediawiki

This repository contains files which will help in spinning GCP server automatically and install Mediawiki application on it.

## Tools Used:

* Terraform for Infrastructure setup
* Ansible for Configuration Management

## Infrastructure setup:

Follow the below steps to spin a CentOS instance in GCP:

* Clone the repository
* Add terrform_key.json file which contains your service account details in main directory
* Modify variable.tfvars file as per requirement
* Initialize the Terraform working directory -- terrform init
* View the execution plan -- terraform plan -var-file=variable.tfvars
* Execute the changes -- terraform apply -var-file=variable.tfvars
* The output will contain IP address of the spinned instance


## Configuration Management:

Follow the below steps to spin a CentOS instance in GCP:

* Clone the repository
* Update the hosts file with the instance's IP address
* Execute the playbook by ansible-playbook -i hosts playbook.yml -u <username> --private-key <path to user's private key file>

## Infrastructure scalaility considerations

* If you wish to create multiple GCP instances, add a variable node_count in *main.tf* file

`variable "node_count" {
  type = number
}`

* Define the variable in *variable.tfvars*
* Modify the vm instance creation part in *vm.tf* file

`resource "google_compute_instance" "vm_instance" {
    count        = var.node_count
    name         = "mediawiki-vm-${count.index}"`


*In order to get the IP addresses of the multiple instances, you need to check the GCP console
or you can declare a range of static IPs which will be assigned to the instances while creation 
and configure locdbalaning module in terraform*

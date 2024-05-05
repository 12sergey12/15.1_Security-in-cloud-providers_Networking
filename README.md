### Домашнее задание к занятию «Организация сети» Баранов Сергей

### Подготовка к выполнению задания

1. Домашнее задание состоит из обязательной части, которую нужно выполнить на провайдере Yandex Cloud, и дополнительной части в AWS (выполняется по желанию). 
2. Все домашние задания в блоке 15 связаны друг с другом и в конце представляют пример законченной инфраструктуры.  
3. Все задания нужно выполнить с помощью Terraform. Результатом выполненного домашнего задания будет код в репозитории. 
4. Перед началом работы настройте доступ к облачным ресурсам из Terraform, используя материалы прошлых лекций и домашнее задание по теме «Облачные провайдеры и синтаксис Terraform». Заранее выберите регион (в случае AWS) и зону.

---
### Задание 1. Yandex Cloud 

**Что нужно сделать**

[Terraform]()

1. Создать пустую VPC. Выбрать зону.

2. Публичная подсеть.

 - Создать в VPC subnet с названием public, сетью 192.168.10.0/24.
 - Создать в этой подсети NAT-инстанс, присвоив ему адрес 192.168.10.254. В качестве image_id использовать fd80mrhj8fl2oe87o4e1.
 - Создать в этой публичной подсети виртуалку с публичным IP, подключиться к ней и убедиться, что есть доступ к интернету.
3. Приватная подсеть.
 - Создать в VPC subnet с названием private, сетью 192.168.20.0/24.
 - Создать route table. Добавить статический маршрут, направляющий весь исходящий трафик private сети в NAT-инстанс.
 - Создать в этой приватной подсети виртуалку с внутренним IP, подключиться к ней через виртуалку, созданную ранее, и убедиться, что есть доступ к интернету.

Resource Terraform для Yandex Cloud:

- [VPC subnet](https://registry.terraform.io/providers/yandex-cloud/yandex/latest/docs/resources/vpc_subnet).
- [Route table](https://registry.terraform.io/providers/yandex-cloud/yandex/latest/docs/resources/vpc_route_table).
- [Compute Instance](https://registry.terraform.io/providers/yandex-cloud/yandex/latest/docs/resources/compute_instance).

<details><summary>Инициализация проекта</summary>

```
root@baranovsa:/home/baranovsa/15.1_cloud/terraform# terraform plan
yandex_vpc_network.network-1: Refreshing state... [id=enpqj81t14h4hiuu1akn]
yandex_vpc_subnet.subnet-public: Refreshing state... [id=e9bc7rpctuit2l2aslft]
yandex_vpc_route_table.nat-route-table: Refreshing state... [id=enphl7q8vqdq351f2ue4]
yandex_vpc_subnet.subnet-private: Refreshing state... [id=e9bluroftcskqd6pkdmd]
yandex_compute_instance.nat-instance: Refreshing state... [id=fhm225ce309ohrq2hv0a]

Terraform used the selected providers to generate the following execution plan. Resource actions are
indicated with the following symbols:
  + create
-/+ destroy and then create replacement

Terraform will perform the following actions:

  # yandex_compute_instance.nat-instance must be replaced
-/+ resource "yandex_compute_instance" "nat-instance" {
  	~ created_at            	= "2024-05-05T12:18:21Z" -> (known after apply)
  	~ folder_id             	= "b1gi8mor51pqsp67s6t9" -> (known after apply)
  	~ fqdn                  	= "nat-instance-vm1.netology.cloud" -> (known after apply)
  	+ gpu_cluster_id        	= (known after apply)
  	~ id                    	= "fhm225ce309ohrq2hv0a" -> (known after apply)
  	- labels                	= {} -> null
  	+ maintenance_grace_period  = (known after apply)
  	+ maintenance_policy    	= (known after apply)
    	name                  	= "nat-instance-vm1"
  	+ service_account_id    	= (known after apply)
  	~ status                	= "running" -> (known after apply)
    	# (5 unchanged attributes hidden)

  	~ boot_disk {
      	~ device_name = "fhmtrh7f7llg79e5oqfm" -> (known after apply)
      	~ disk_id 	= "fhmtrh7f7llg79e5oqfm" -> (known after apply)
      	~ mode    	= "READ_WRITE" -> (known after apply)
        	# (1 unchanged attribute hidden)

      	~ initialize_params {
          	~ block_size  = 4096 -> (known after apply)
          	+ description = (known after apply)
            	name    	= "root-nat-instance-vm1"
          	+ snapshot_id = (known after apply)
          	~ type    	= "network-ssd" -> "network-nvme" # forces replacement
            	# (2 unchanged attributes hidden)
        	}
    	}

  	- metadata_options {
      	- aws_v1_http_endpoint = 1 -> null
      	- aws_v1_http_token	= 2 -> null
      	- gce_http_endpoint	= 1 -> null
      	- gce_http_token   	= 1 -> null
    	}

  	~ network_interface {
      	~ index          	= 0 -> (known after apply)
      	~ ipv6           	= false -> (known after apply)
      	+ ipv6_address   	= (known after apply)
      	~ mac_address    	= "d0:0d:21:15:8e:18" -> (known after apply)
      	~ nat_ip_address 	= "51.250.82.176" -> (known after apply)
      	~ nat_ip_version 	= "IPV4" -> (known after apply)
      	~ security_group_ids = [] -> (known after apply)
        	# (4 unchanged attributes hidden)
    	}

  	- placement_policy {
      	- host_affinity_rules   	= [] -> null
      	- placement_group_partition = 0 -> null
    	}

  	~ resources {
      	- gpus      	= 0 -> null
        	# (3 unchanged attributes hidden)
    	}

  	- scheduling_policy {
      	- preemptible = false -> null
    	}
	}

  # yandex_compute_instance.private-vm will be created
  + resource "yandex_compute_instance" "private-vm" {
  	+ created_at            	= (known after apply)
  	+ folder_id             	= (known after apply)
  	+ fqdn                  	= (known after apply)
  	+ gpu_cluster_id        	= (known after apply)
  	+ hostname              	= "private-vm1.netology.cloud"
  	+ id                    	= (known after apply)
  	+ maintenance_grace_period  = (known after apply)
  	+ maintenance_policy    	= (known after apply)
  	+ metadata              	= {
      	+ "ssh-keys" = <<-EOT
            	ubuntu:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCmdCHVMNMaUlhrJZgwsh115WrO5MaFxQNmaocsaD3Zc8fVx7piLbnawOPAIy6ydlM2ia5ozdAwgVV4Gm16XJnFlewnPRqja6+PTCpGodugaj8NAy8NudOTROYvj1wxo3BmISMsvJW+FSqqyWY+1D70EODa3670VP/Nt+QTQt8+WWbLr5LuUSbzxYlFsas4h+POs4c+AycDbZAb3Wlg8NQ92DjnV4zTUeljUCN9MarhXltKjWvTlDHK7a3lcwVdsLUfSfv36aA5QpQTlovOkDvbEkwYmmumzUck58F1GBk5CNU7q2RjjtPilB51FFY3y9vM8jdk54k857WK6tTLoHRlDEWAt5oXPoOkV7VuPC273gvxmAmT2Mc2dsxU99WgDeggSCC/V4UGoJOTOqxIGAV1TAFYJItdmT9cpsZoJZIRNT9YLTUC6wk4L5u5lLMZ0mHU8slTQUs13mzkEt4cr6GcpkhA2hRLovbHxyOagG3mN7cDdJMVpVHSuEuO0CxDULk= root@baranovsa
        	EOT
    	}
  	+ name                  	= "private-vm1"
  	+ network_acceleration_type = "standard"
  	+ platform_id           	= "standard-v1"
  	+ service_account_id    	= (known after apply)
  	+ status                	= (known after apply)
  	+ zone                  	= "ru-central1-a"

  	+ boot_disk {
      	+ auto_delete = true
      	+ device_name = (known after apply)
      	+ disk_id 	= (known after apply)
      	+ mode    	= (known after apply)

      	+ initialize_params {
          	+ block_size  = (known after apply)
          	+ description = (known after apply)
          	+ image_id	= "fd8cnj92ad0th7m7krqh"
          	+ name    	= "root-private-vm1"
          	+ size    	= 50
          	+ snapshot_id = (known after apply)
          	+ type    	= "network-nvme"
        	}
    	}

  	+ network_interface {
      	+ index          	= (known after apply)
      	+ ip_address     	= (known after apply)
      	+ ipv4           	= true
      	+ ipv6           	= (known after apply)
      	+ ipv6_address   	= (known after apply)
      	+ mac_address    	= (known after apply)
      	+ nat            	= false
      	+ nat_ip_address 	= (known after apply)
      	+ nat_ip_version 	= (known after apply)
      	+ security_group_ids = (known after apply)
      	+ subnet_id      	= "e9bluroftcskqd6pkdmd"
    	}

  	+ resources {
      	+ core_fraction = 100
      	+ cores     	= 2
      	+ memory    	= 2
    	}
	}

  # yandex_compute_instance.public-vm will be created
  + resource "yandex_compute_instance" "public-vm" {
  	+ created_at            	= (known after apply)
  	+ folder_id             	= (known after apply)
  	+ fqdn                  	= (known after apply)
  	+ gpu_cluster_id        	= (known after apply)
  	+ hostname              	= "public-vm1.netology.cloud"
  	+ id                    	= (known after apply)
  	+ maintenance_grace_period  = (known after apply)
  	+ maintenance_policy    	= (known after apply)
  	+ metadata              	= {
      	+ "ssh-keys" = <<-EOT
            	ubuntu:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCmdCHVMNMaUlhrJZgwsh115WrO5MaFxQNmaocsaD3Zc8fVx7piLbnawOPAIy6ydlM2ia5ozdAwgVV4Gm16XJnFlewnPRqja6+PTCpGodugaj8NAy8NudOTROYvj1wxo3BmISMsvJW+FSqqyWY+1D70EODa3670VP/Nt+QTQt8+WWbLr5LuUSbzxYlFsas4h+POs4c+AycDbZAb3Wlg8NQ92DjnV4zTUeljUCN9MarhXltKjWvTlDHK7a3lcwVdsLUfSfv36aA5QpQTlovOkDvbEkwYmmumzUck58F1GBk5CNU7q2RjjtPilB51FFY3y9vM8jdk54k857WK6tTLoHRlDEWAt5oXPoOkV7VuPC273gvxmAmT2Mc2dsxU99WgDeggSCC/V4UGoJOTOqxIGAV1TAFYJItdmT9cpsZoJZIRNT9YLTUC6wk4L5u5lLMZ0mHU8slTQUs13mzkEt4cr6GcpkhA2hRLovbHxyOagG3mN7cDdJMVpVHSuEuO0CxDULk= root@baranovsa
        	EOT
    	}
  	+ name                  	= "public-vm1"
  	+ network_acceleration_type = "standard"
  	+ platform_id           	= "standard-v1"
  	+ service_account_id    	= (known after apply)
  	+ status                	= (known after apply)
  	+ zone                  	= "ru-central1-a"

  	+ boot_disk {
      	+ auto_delete = true
      	+ device_name = (known after apply)
      	+ disk_id 	= (known after apply)
      	+ mode    	= (known after apply)

      	+ initialize_params {
          	+ block_size  = (known after apply)
          	+ description = (known after apply)
          	+ image_id	= "fd8cnj92ad0th7m7krqh"
          	+ name    	= "root-public-vm1"
          	+ size    	= 50
          	+ snapshot_id = (known after apply)
          	+ type    	= "network-nvme"
        	}
    	}

  	+ network_interface {
      	+ index          	= (known after apply)
      	+ ip_address     	= (known after apply)
      	+ ipv4           	= true
      	+ ipv6           	= (known after apply)
      	+ ipv6_address   	= (known after apply)
      	+ mac_address    	= (known after apply)
      	+ nat            	= true
      	+ nat_ip_address 	= (known after apply)
      	+ nat_ip_version 	= (known after apply)
      	+ security_group_ids = (known after apply)
      	+ subnet_id      	= "e9bc7rpctuit2l2aslft"
    	}

  	+ resources {
      	+ core_fraction = 100
      	+ cores     	= 2
      	+ memory    	= 2
    	}
	}

Plan: 3 to add, 0 to change, 1 to destroy.

Changes to Outputs:
  ~ external_ip_address_nat_vm 	= "51.250.82.176" -> (known after apply)
  + external_ip_address_private_vm = (known after apply)
  + external_ip_address_public_vm  = (known after apply)
  + internal_ip_address_private_vm = (known after apply)
  + internal_ip_address_public_vm  = (known after apply)

───────────────────────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly
these actions if you run "terraform apply" now.
root@baranovsa:/home/baranovsa/15.1_cloud/terraform#

root@baranovsa:/home/baranovsa/15.1_cloud/terraform# terraform apply
yandex_vpc_network.network-1: Refreshing state... [id=enpqj81t14h4hiuu1akn]
yandex_vpc_subnet.subnet-public: Refreshing state... [id=e9bc7rpctuit2l2aslft]
yandex_vpc_route_table.nat-route-table: Refreshing state... [id=enphl7q8vqdq351f2ue4]
yandex_vpc_subnet.subnet-private: Refreshing state... [id=e9bluroftcskqd6pkdmd]
yandex_compute_instance.nat-instance: Refreshing state... [id=fhm225ce309ohrq2hv0a]

Terraform used the selected providers to generate the following execution plan. Resource actions are
indicated with the following symbols:
  + create
-/+ destroy and then create replacement

Terraform will perform the following actions:

  # yandex_compute_instance.nat-instance must be replaced
-/+ resource "yandex_compute_instance" "nat-instance" {
  	~ created_at            	= "2024-05-05T12:18:21Z" -> (known after apply)
  	~ folder_id             	= "b1gi8mor51pqsp67s6t9" -> (known after apply)
  	~ fqdn                  	= "nat-instance-vm1.netology.cloud" -> (known after apply)
  	+ gpu_cluster_id        	= (known after apply)
  	~ id                    	= "fhm225ce309ohrq2hv0a" -> (known after apply)
  	- labels                	= {} -> null
  	+ maintenance_grace_period  = (known after apply)
  	+ maintenance_policy    	= (known after apply)
    	name                  	= "nat-instance-vm1"
  	+ service_account_id    	= (known after apply)
  	~ status                	= "running" -> (known after apply)
    	# (5 unchanged attributes hidden)

  	~ boot_disk {
      	~ device_name = "fhmtrh7f7llg79e5oqfm" -> (known after apply)
      	~ disk_id 	= "fhmtrh7f7llg79e5oqfm" -> (known after apply)
      	~ mode    	= "READ_WRITE" -> (known after apply)
        	# (1 unchanged attribute hidden)

      	~ initialize_params {
          	~ block_size  = 4096 -> (known after apply)
          	+ description = (known after apply)
            	name    	= "root-nat-instance-vm1"
          	+ snapshot_id = (known after apply)
          	~ type    	= "network-ssd" -> "network-nvme" # forces replacement
            	# (2 unchanged attributes hidden)
        	}
    	}

  	- metadata_options {
      	- aws_v1_http_endpoint = 1 -> null
      	- aws_v1_http_token	= 2 -> null
      	- gce_http_endpoint	= 1 -> null
      	- gce_http_token   	= 1 -> null
    	}

  	~ network_interface {
      	~ index          	= 0 -> (known after apply)
      	~ ipv6           	= false -> (known after apply)
      	+ ipv6_address   	= (known after apply)
      	~ mac_address    	= "d0:0d:21:15:8e:18" -> (known after apply)
      	~ nat_ip_address 	= "51.250.82.176" -> (known after apply)
      	~ nat_ip_version 	= "IPV4" -> (known after apply)
      	~ security_group_ids = [] -> (known after apply)
        	# (4 unchanged attributes hidden)
    	}

  	- placement_policy {
      	- host_affinity_rules   	= [] -> null
      	- placement_group_partition = 0 -> null
    	}

  	~ resources {
      	- gpus      	= 0 -> null
        	# (3 unchanged attributes hidden)
    	}

  	- scheduling_policy {
      	- preemptible = false -> null
    	}
	}

  # yandex_compute_instance.private-vm will be created
  + resource "yandex_compute_instance" "private-vm" {
  	+ created_at            	= (known after apply)
  	+ folder_id             	= (known after apply)
  	+ fqdn                  	= (known after apply)
  	+ gpu_cluster_id        	= (known after apply)
  	+ hostname              	= "private-vm1.netology.cloud"
  	+ id                    	= (known after apply)
  	+ maintenance_grace_period  = (known after apply)
  	+ maintenance_policy    	= (known after apply)
  	+ metadata              	= {
      	+ "ssh-keys" = <<-EOT
            	ubuntu:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCmdCHVMNMaUlhrJZgwsh115WrO5MaFxQNmaocsaD3Zc8fVx7piLbnawOPAIy6ydlM2ia5ozdAwgVV4Gm16XJnFlewnPRqja6+PTCpGodugaj8NAy8NudOTROYvj1wxo3BmISMsvJW+FSqqyWY+1D70EODa3670VP/Nt+QTQt8+WWbLr5LuUSbzxYlFsas4h+POs4c+AycDbZAb3Wlg8NQ92DjnV4zTUeljUCN9MarhXltKjWvTlDHK7a3lcwVdsLUfSfv36aA5QpQTlovOkDvbEkwYmmumzUck58F1GBk5CNU7q2RjjtPilB51FFY3y9vM8jdk54k857WK6tTLoHRlDEWAt5oXPoOkV7VuPC273gvxmAmT2Mc2dsxU99WgDeggSCC/V4UGoJOTOqxIGAV1TAFYJItdmT9cpsZoJZIRNT9YLTUC6wk4L5u5lLMZ0mHU8slTQUs13mzkEt4cr6GcpkhA2hRLovbHxyOagG3mN7cDdJMVpVHSuEuO0CxDULk= root@baranovsa
        	EOT
    	}
  	+ name                  	= "private-vm1"
  	+ network_acceleration_type = "standard"
  	+ platform_id           	= "standard-v1"
  	+ service_account_id    	= (known after apply)
  	+ status                	= (known after apply)
  	+ zone                  	= "ru-central1-a"

  	+ boot_disk {
      	+ auto_delete = true
      	+ device_name = (known after apply)
      	+ disk_id 	= (known after apply)
      	+ mode    	= (known after apply)

      	+ initialize_params {
          	+ block_size  = (known after apply)
          	+ description = (known after apply)
          	+ image_id	= "fd8cnj92ad0th7m7krqh"
          	+ name    	= "root-private-vm1"
          	+ size    	= 50
          	+ snapshot_id = (known after apply)
          	+ type    	= "network-nvme"
        	}
    	}

  	+ network_interface {
      	+ index          	= (known after apply)
      	+ ip_address     	= (known after apply)
      	+ ipv4           	= true
      	+ ipv6           	= (known after apply)
      	+ ipv6_address   	= (known after apply)
      	+ mac_address    	= (known after apply)
      	+ nat            	= false
      	+ nat_ip_address 	= (known after apply)
      	+ nat_ip_version 	= (known after apply)
      	+ security_group_ids = (known after apply)
      	+ subnet_id      	= "e9bluroftcskqd6pkdmd"
    	}

  	+ resources {
      	+ core_fraction = 100
      	+ cores     	= 2
      	+ memory    	= 2
    	}
	}

  # yandex_compute_instance.public-vm will be created
  + resource "yandex_compute_instance" "public-vm" {
  	+ created_at            	= (known after apply)
  	+ folder_id             	= (known after apply)
  	+ fqdn                  	= (known after apply)
  	+ gpu_cluster_id        	= (known after apply)
  	+ hostname              	= "public-vm1.netology.cloud"
  	+ id                    	= (known after apply)
  	+ maintenance_grace_period  = (known after apply)
  	+ maintenance_policy    	= (known after apply)
  	+ metadata              	= {
      	+ "ssh-keys" = <<-EOT
            	ubuntu:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCmdCHVMNMaUlhrJZgwsh115WrO5MaFxQNmaocsaD3Zc8fVx7piLbnawOPAIy6ydlM2ia5ozdAwgVV4Gm16XJnFlewnPRqja6+PTCpGodugaj8NAy8NudOTROYvj1wxo3BmISMsvJW+FSqqyWY+1D70EODa3670VP/Nt+QTQt8+WWbLr5LuUSbzxYlFsas4h+POs4c+AycDbZAb3Wlg8NQ92DjnV4zTUeljUCN9MarhXltKjWvTlDHK7a3lcwVdsLUfSfv36aA5QpQTlovOkDvbEkwYmmumzUck58F1GBk5CNU7q2RjjtPilB51FFY3y9vM8jdk54k857WK6tTLoHRlDEWAt5oXPoOkV7VuPC273gvxmAmT2Mc2dsxU99WgDeggSCC/V4UGoJOTOqxIGAV1TAFYJItdmT9cpsZoJZIRNT9YLTUC6wk4L5u5lLMZ0mHU8slTQUs13mzkEt4cr6GcpkhA2hRLovbHxyOagG3mN7cDdJMVpVHSuEuO0CxDULk= root@baranovsa
        	EOT
    	}
  	+ name                  	= "public-vm1"
  	+ network_acceleration_type = "standard"
  	+ platform_id           	= "standard-v1"
  	+ service_account_id    	= (known after apply)
  	+ status                	= (known after apply)
  	+ zone                  	= "ru-central1-a"

  	+ boot_disk {
      	+ auto_delete = true
      	+ device_name = (known after apply)
      	+ disk_id 	= (known after apply)
      	+ mode    	= (known after apply)

      	+ initialize_params {
          	+ block_size  = (known after apply)
          	+ description = (known after apply)
          	+ image_id	= "fd8cnj92ad0th7m7krqh"
          	+ name    	= "root-public-vm1"
          	+ size    	= 50
          	+ snapshot_id = (known after apply)
          	+ type    	= "network-nvme"
        	}
    	}

  	+ network_interface {
      	+ index          	= (known after apply)
      	+ ip_address     	= (known after apply)
      	+ ipv4           	= true
      	+ ipv6           	= (known after apply)
      	+ ipv6_address   	= (known after apply)
      	+ mac_address    	= (known after apply)
      	+ nat            	= true
      	+ nat_ip_address 	= (known after apply)
      	+ nat_ip_version 	= (known after apply)
      	+ security_group_ids = (known after apply)
      	+ subnet_id      	= "e9bc7rpctuit2l2aslft"
    	}

  	+ resources {
      	+ core_fraction = 100
      	+ cores     	= 2
      	+ memory    	= 2
    	}
	}

Plan: 3 to add, 0 to change, 1 to destroy.

Changes to Outputs:
  ~ external_ip_address_nat_vm 	= "51.250.82.176" -> (known after apply)
  + external_ip_address_private_vm = (known after apply)
  + external_ip_address_public_vm  = (known after apply)
  + internal_ip_address_private_vm = (known after apply)
  + internal_ip_address_public_vm  = (known after apply)

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

yandex_compute_instance.nat-instance: Destroying... [id=fhm225ce309ohrq2hv0a]
yandex_compute_instance.private-vm: Creating...
yandex_compute_instance.public-vm: Creating...
yandex_compute_instance.nat-instance: Still destroying... [id=fhm225ce309ohrq2hv0a, 10s elapsed]
yandex_compute_instance.private-vm: Still creating... [10s elapsed]
yandex_compute_instance.public-vm: Still creating... [10s elapsed]
yandex_compute_instance.private-vm: Still creating... [20s elapsed]
yandex_compute_instance.nat-instance: Still destroying... [id=fhm225ce309ohrq2hv0a, 20s elapsed]
yandex_compute_instance.public-vm: Still creating... [20s elapsed]
yandex_compute_instance.private-vm: Still creating... [30s elapsed]
yandex_compute_instance.nat-instance: Still destroying... [id=fhm225ce309ohrq2hv0a, 30s elapsed]
yandex_compute_instance.public-vm: Still creating... [30s elapsed]
yandex_compute_instance.public-vm: Creation complete after 31s [id=fhmn6i9e125qutt140oo]
yandex_compute_instance.private-vm: Creation complete after 35s [id=fhm5n07e5ivh2oqfisoh]
yandex_compute_instance.nat-instance: Destruction complete after 39s
yandex_compute_instance.nat-instance: Creating...
yandex_compute_instance.nat-instance: Still creating... [10s elapsed]
yandex_compute_instance.nat-instance: Still creating... [20s elapsed]
yandex_compute_instance.nat-instance: Still creating... [30s elapsed]
yandex_compute_instance.nat-instance: Still creating... [40s elapsed]
yandex_compute_instance.nat-instance: Still creating... [50s elapsed]
yandex_compute_instance.nat-instance: Creation complete after 57s [id=fhmt7pcn1r71v6h49rei]

Apply complete! Resources: 3 added, 0 changed, 1 destroyed.

Outputs:

external_ip_address_nat_vm = "158.160.44.158"
external_ip_address_private_vm = ""
external_ip_address_public_vm = "158.160.105.247"
internal_ip_address_nat_vm = "192.168.10.254"
internal_ip_address_private_vm = "192.168.20.21"
internal_ip_address_public_vm = "192.168.10.4"
root@baranovsa:/home/baranovsa/15.1_cloud/terraform#

```

</details>


------

Виртуальные машины

[monitoring](https://github.com/12sergey12/15.1_Security-in-cloud-providers_Networking/blob/main/images/15.1_vm.png)


Подсети 

[monitoring](https://github.com/12sergey12/15.1_Security-in-cloud-providers_Networking/blob/main/images/15.1_podset.png)


Таблицы маршрутизации

[monitoring](https://github.com/12sergey12/15.1_Security-in-cloud-providers_Networking/blob/main/images/15.1_tabl.marsh..png)


Проверка интернета с public

```
root@baranovsa:/home/baranovsa/15.1_cloud/terraform# ssh ubuntu@158.160.105.247
Welcome to Ubuntu 20.04.6 LTS (GNU/Linux 5.4.0-177-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management: 	https://landscape.canonical.com
 * Support:    	https://ubuntu.com/pro

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

ubuntu@public-vm1:~$ curl ifconfig.me
158.160.105.247ubuntu@public-vm1:~$ curl ya.ru
ubuntu@public-vm1:~$
ubuntu@public-vm1:~$ ping ya.ru
PING ya.ru (5.255.255.242) 56(84) bytes of data.
64 bytes from ya.ru (5.255.255.242): icmp_seq=1 ttl=56 time=0.917 ms
64 bytes from ya.ru (5.255.255.242): icmp_seq=2 ttl=56 time=0.312 ms
64 bytes from ya.ru (5.255.255.242): icmp_seq=3 ttl=56 time=0.298 ms
64 bytes from ya.ru (5.255.255.242): icmp_seq=4 ttl=56 time=0.341 ms
64 bytes from ya.ru (5.255.255.242): icmp_seq=5 ttl=56 time=0.342 ms
64 bytes from ya.ru (5.255.255.242): icmp_seq=6 ttl=56 time=0.327 ms
64 bytes from ya.ru (5.255.255.242): icmp_seq=7 ttl=56 time=0.370 ms
64 bytes from ya.ru (5.255.255.242): icmp_seq=8 ttl=56 time=0.350 ms
^C
--- ya.ru ping statistics ---
8 packets transmitted, 8 received, 0% packet loss, time 7152ms
rtt min/avg/max/mdev = 0.298/0.407/0.917/0.193 ms
ubuntu@public-vm1:~$


ubuntu@public-vm1:~$ ping ya.ru
PING ya.ru (5.255.255.242) 56(84) bytes of data.
64 bytes from ya.ru (5.255.255.242): icmp_seq=1 ttl=56 time=0.362 ms
64 bytes from ya.ru (5.255.255.242): icmp_seq=2 ttl=56 time=0.341 ms
64 bytes from ya.ru (5.255.255.242): icmp_seq=3 ttl=56 time=0.363 ms
64 bytes from ya.ru (5.255.255.242): icmp_seq=4 ttl=56 time=0.365 ms
64 bytes from ya.ru (5.255.255.242): icmp_seq=5 ttl=56 time=0.351 ms
64 bytes from ya.ru (5.255.255.242): icmp_seq=6 ttl=56 time=0.371 ms
64 bytes from ya.ru (5.255.255.242): icmp_seq=7 ttl=56 time=0.356 ms
^C
--- ya.ru ping statistics ---
7 packets transmitted, 7 received, 0% packet loss, time 6108ms
rtt min/avg/max/mdev = 0.341/0.358/0.371/0.009 ms
ubuntu@public-vm1:~$
```


Проверка интернета с private

```
root@baranovsa:/home/baranovsa/15.1_cloud/terraform# ssh ubuntu@158.160.44.158
The authenticity of host '158.160.44.158 (158.160.44.158)' can't be established.
ECDSA key fingerprint is SHA256:eABVfhUk6Tt1oFbV72I2WBlDORIHi94r1kUtO6Yy5es.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '158.160.44.158' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 18.04.1 LTS (GNU/Linux 4.15.0-29-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management: 	https://landscape.canonical.com
 * Support:    	https://ubuntu.com/advantage



#################################################################
This instance runs Yandex.Cloud Marketplace product
Please wait while we configure your product...

Documentation for Yandex Cloud Marketplace images available at https://cloud.yandex.ru/docs

#################################################################


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

ubuntu@nat-instance-vm1:~$ curl ifconfig.me
158.160.44.158ubuntu@nat-instance-vm1:~$ ping ya.ru
PING ya.ru (5.255.255.242) 56(84) bytes of data.
64 bytes from ya.ru (5.255.255.242): icmp_seq=1 ttl=56 time=0.747 ms
64 bytes from ya.ru (5.255.255.242): icmp_seq=2 ttl=56 time=0.333 ms
64 bytes from ya.ru (5.255.255.242): icmp_seq=3 ttl=56 time=0.401 ms
64 bytes from ya.ru (5.255.255.242): icmp_seq=4 ttl=56 time=0.309 ms
64 bytes from ya.ru (5.255.255.242): icmp_seq=5 ttl=56 time=0.346 ms
^C
--- ya.ru ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4073ms
rtt min/avg/max/mdev = 0.309/0.427/0.747/0.163 ms
ubuntu@nat-instance-vm1:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
	link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
	inet 127.0.0.1/8 scope host lo
   	valid_lft forever preferred_lft forever
	inet6 ::1/128 scope host
   	valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
	link/ether d0:0d:1d:3e:59:70 brd ff:ff:ff:ff:ff:ff
	inet 192.168.10.254/24 brd 192.168.10.255 scope global eth0
   	valid_lft forever preferred_lft forever
	inet6 fe80::d20d:1dff:fe3e:5970/64 scope link
   	valid_lft forever preferred_lft forever
ubuntu@nat-instance-vm1:~$
ubuntu@nat-instance-vm1:~$
ubuntu@nat-instance-vm1:~$ exit
logout
Connection to 158.160.44.158 closed.
root@baranovsa:/home/baranovsa/15.1_cloud/terraform# ssh ubuntu@158.160.44.158
Welcome to Ubuntu 18.04.1 LTS (GNU/Linux 4.15.0-29-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management: 	https://landscape.canonical.com
 * Support:    	https://ubuntu.com/advantage

New release '20.04.6 LTS' available.
Run 'do-release-upgrade' to upgrade to it.



#################################################################
This instance runs Yandex.Cloud Marketplace product
Please wait while we configure your product...

Documentation for Yandex Cloud Marketplace images available at https://cloud.yandex.ru/docs

#################################################################

Last login: Sun May  5 12:54:38 2024 from 95.191.248.141
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

ubuntu@nat-instance-vm1:~$ ping ya.ru
PING ya.ru (77.88.55.242) 56(84) bytes of data.
64 bytes from ya.ru (77.88.55.242): icmp_seq=1 ttl=249 time=4.04 ms
64 bytes from ya.ru (77.88.55.242): icmp_seq=2 ttl=249 time=3.57 ms
64 bytes from ya.ru (77.88.55.242): icmp_seq=3 ttl=249 time=3.49 ms
64 bytes from ya.ru (77.88.55.242): icmp_seq=4 ttl=249 time=3.54 ms
64 bytes from ya.ru (77.88.55.242): icmp_seq=5 ttl=249 time=3.45 ms
^C
--- ya.ru ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4006ms
rtt min/avg/max/mdev = 3.452/3.623/4.049/0.217 ms
ubuntu@nat-instance-vm1:~$
```


---


### Задание 2. AWS* (задание со звёздочкой)

Это необязательное задание. Его выполнение не влияет на получение зачёта по домашней работе.

**Что нужно сделать**

1. Создать пустую VPC с подсетью 10.10.0.0/16.
2. Публичная подсеть.

 - Создать в VPC subnet с названием public, сетью 10.10.1.0/24.
 - Разрешить в этой subnet присвоение public IP по-умолчанию.
 - Создать Internet gateway.
 - Добавить в таблицу маршрутизации маршрут, направляющий весь исходящий трафик в Internet gateway.
 - Создать security group с разрешающими правилами на SSH и ICMP. Привязать эту security group на все, создаваемые в этом ДЗ, виртуалки.
 - Создать в этой подсети виртуалку и убедиться, что инстанс имеет публичный IP. Подключиться к ней, убедиться, что есть доступ к интернету.
 - Добавить NAT gateway в public subnet.
3. Приватная подсеть.
 - Создать в VPC subnet с названием private, сетью 10.10.2.0/24.
 - Создать отдельную таблицу маршрутизации и привязать её к private подсети.
 - Добавить Route, направляющий весь исходящий трафик private сети в NAT.
 - Создать виртуалку в приватной сети.
 - Подключиться к ней по SSH по приватному IP через виртуалку, созданную ранее в публичной подсети, и убедиться, что с виртуалки есть выход в интернет.

Resource Terraform:

1. [VPC](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/vpc).
1. [Subnet](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/subnet).
1. [Internet Gateway](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/internet_gateway).

### Правила приёма работы

Домашняя работа оформляется в своём Git репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
Файл README.md должен содержать скриншоты вывода необходимых команд, а также скриншоты результатов.
Репозиторий должен содержать тексты манифестов или ссылки на них в файле README.md.

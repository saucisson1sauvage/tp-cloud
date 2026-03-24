j pense que l'espece de filtre pour redirer les secrets retire un peut trop :d

## 🌞 Déterminer quel algorithme de chiffrement utiliser pour vos clés
https://www.openssh.com/txt/release-8.8

ed

## 🌞 Générer une paire de clés pour ce TP
```
ssh-keygen -t ed25519 -f ~/.ssh/cloud_tp1
The key's randomart image is:
+--[ED25519 256]--+
|    ..+.  . +    |
|     o = . o +  .|
|      o *   ...+ |
|       . o   .= o|
|        S   .o o.|
|         .  ..= o|
|     .. E... o+X |
|      o*+.o o+B. |
|     o==++ . oo  |
+----[SHA256]-----+
```

## 🌞 Configurer un agent SSH sur votre poste





```
(.venv) saus@pop-os ~/.ssh [0|123]> eval (ssh-agent -c)
Agent pid 3568270

(.venv) saus@pop-os ~/.ssh> ssh-add cloud_tp1
Enter passphrase for cloud_tp1: 
Identity added: cloud_tp1 (saus@pop-os)
```

## 🌞 Connectez-vous en SSH à la VM pour preuve

```
saus@pop-os ~ [255]> ssh azureuser@[secret]
Linux debianeu 6.12.74+deb13+1-cloud-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.12.74-2 (2026-03-08) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
azureuser@debianeu:~$ 
```

## 🌞 Créez une VM depuis le Azure CLI

~~~
az>> az vm create -g meo -n super_vm --size Standard_B1s --image almalinux:almalinux-x86_64:10-gen2:10.1.202512150 --admin-username saus --ssh-key-values ~/.ssh/cloud_tp1.pub
Consider upgrading security for your workloads using Azure Trusted Launch VMs. To know more about Trusted Launch, please visit https://aka.ms/TrustedLaunch.
{
  "fqdns": "",
  "id": "/subscriptions/resourceGroups/meo/providers/Microsoft.Compute/virtualMachines/super_vm",
  "location": "denmarkeast",
  "macAddress": "[secret]",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "[secret]",
  "resourceGroup": "meo"
}
~~~
js j me suis mis sur mon pc au lieu du docker c pa bien 


## 🌞 Assurez-vous que vous pouvez vous connecter à la VM en SSH sur son IP publique

~~~
saus@pop-os ~ [1]> ssh saus@[secret]
The authenticity of host '[secret]' can't be established.
ED25519 key fingerprint is SHA256:[secret].
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[secret]' (ED25519) to the list of known hosts.
~~~
## 🌞 Une fois connecté, prouvez la présence...

~~~
[saus@supervm /]$ sudo find / -name "waagent.service"
/sys/fs/cgroup/azure.slice/waagent.service
/etc/systemd/system/multi-user.target.wants/waagent.service
/usr/lib/systemd/system/waagent.service


[saus@supervm /]$ sudo find / -name "cloud-init.service"
/etc/systemd/system/cloud-init.target.wants/cloud-init.service
/usr/lib/systemd/system/cloud-init.service
~~~

## 🌞 Utilisez Terraform pour créer une VM dans Azure

 <details>
  <summary> terraform apply </summary>


```

 saus@pop-os ~/tepeeeeqepfepmfeqpmo> terraform apply

Terraform used the selected providers to generate the following execution plan. Resource actions are
indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # azurerm_linux_virtual_machine.main will be created
  + resource "azurerm_linux_virtual_machine" "main" {
      + admin_username                                         = "saus"
      + allow_extension_operations                             = (known after apply)
      + bypass_platform_safety_checks_on_user_schedule_enabled = false
      + computer_name                                          = (known after apply)
      + disable_password_authentication                        = (known after apply)
      + disk_controller_type                                   = (known after apply)
      + extensions_time_budget                                 = "PT1H30M"
      + id                                                     = (known after apply)
      + location                                               = "denmarkeast"
      + max_bid_price                                          = -1
      + name                                                   = "super-vm"
      + network_interface_ids                                  = (known after apply)
      + os_managed_disk_id                                     = (known after apply)
      + patch_assessment_mode                                  = (known after apply)
      + patch_mode                                             = (known after apply)
      + platform_fault_domain                                  = -1
      + priority                                               = "Regular"
      + private_ip_address                                     = (known after apply)
      + private_ip_addresses                                   = (known after apply)
      + provision_vm_agent                                     = (known after apply)
      + public_ip_address                                      = (known after apply)
      + public_ip_addresses                                    = (known after apply)
      + resource_group_name                                    = "OOOOGAA_BOOGAAAAAAAA_xd"
      + size                                                   = "Standard_B1s"
      + virtual_machine_id                                     = (known after apply)
      + vm_agent_platform_updates_enabled                      = (known after apply)

      + admin_ssh_key {
          + public_key = <<-EOT
                [secret]
            EOT
          + username   = "saus"
        }

      + os_disk {
          + caching                   = "ReadWrite"
          + disk_size_gb              = (known after apply)
          + id                        = (known after apply)
          + name                      = "vm-os-disk"
          + storage_account_type      = "Standard_LRS"
          + write_accelerator_enabled = false
        }

      + source_image_reference {
          + offer     = "almalinux-x86_64"
          + publisher = "almalinux"
          + sku       = "10-gen2"
          + version   = "latest"
        }

      + termination_notification (known after apply)
    }

  # azurerm_network_interface.main will be created
  + resource "azurerm_network_interface" "main" {
      + accelerated_networking_enabled = false
      + applied_dns_servers            = (known after apply)
      + id                             = (known after apply)
      + internal_domain_name_suffix    = (known after apply)
      + ip_forwarding_enabled          = false
      + location                       = "denmarkeast"
      + mac_address                    = (known after apply)
      + name                           = "vm-nic"
      + private_ip_address             = (known after apply)
      + private_ip_addresses           = (known after apply)
      + resource_group_name            = "OOOOGAA_BOOGAAAAAAAA_xd"
      + virtual_machine_id             = (known after apply)

      + ip_configuration {
          + gateway_load_balancer_frontend_ip_configuration_id = (known after apply)
          + name                                               = "internal"
          + primary                                            = (known after apply)
          + private_ip_address                                 = (known after apply)
          + private_ip_address_allocation                      = "Dynamic"
          + private_ip_address_version                         = "IPv4"
          + public_ip_address_id                               = (known after apply)
          + subnet_id                                          = (known after apply)
        }
    }

  # azurerm_public_ip.main will be created
  + resource "azurerm_public_ip" "main" {
      + allocation_method       = "Static"
      + ddos_protection_mode    = "VirtualNetworkInherited"
      + fqdn                    = (known after apply)
      + id                      = (known after apply)
      + idle_timeout_in_minutes = 4
      + ip_address              = (known after apply)
      + ip_version              = "IPv4"
      + location                = "denmarkeast"
      + name                    = "vm-ip"
      + resource_group_name     = "OOOOGAA_BOOGAAAAAAAA_xd"
      + sku                     = "Standard"
      + sku_tier                = "Regional"
    }

  # azurerm_resource_group.main will be created
  + resource "azurerm_resource_group" "main" {
      + id       = (known after apply)
      + location = "denmarkeast"
      + name     = "OOOOGAA_BOOGAAAAAAAA_xd"
    }

  # azurerm_subnet.main will be created
  + resource "azurerm_subnet" "main" {
      + address_prefixes                              = [
          + "10.0.1.0/24",
        ]
      + default_outbound_access_enabled               = true
      + id                                            = (known after apply)
      + name                                          = "vm-subnet"
      + private_endpoint_network_policies             = "Disabled"
      + private_link_service_network_policies_enabled = true
      + resource_group_name                           = "OOOOGAA_BOOGAAAAAAAA_xd"
      + virtual_network_name                          = "vm-vnet"
    }

  # azurerm_virtual_network.main will be created
  + resource "azurerm_virtual_network" "main" {
      + address_space                  = [
          + "10.0.0.0/16",
        ]
      + dns_servers                    = (known after apply)
      + guid                           = (known after apply)
      + id                             = (known after apply)
      + location                       = "denmarkeast"
      + name                           = "vm-vnet"
      + private_endpoint_vnet_policies = "Disabled"
      + resource_group_name            = "OOOOGAA_BOOGAAAAAAAA_xd"
      + subnet                         = (known after apply)
    }

Plan: 6 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

azurerm_resource_group.main: Creating...
azurerm_resource_group.main: Still creating... [00m10s elapsed]
azurerm_resource_group.main: Still creating... [00m20s elapsed]
azurerm_resource_group.main: Creation complete after 24s [id=/subscriptions/resourceGroups/OOOOGAA_BOOGAAAAAAAA_xd]
azurerm_virtual_network.main: Creating...
azurerm_public_ip.main: Creating...
azurerm_public_ip.main: Creation complete after 4s [id=/subscriptions/[secret]/resourceGroups/OOOOGAA_BOOGAAAAAAAA_xd/providers/Microsoft.Network/publicIPAddresses/vm-ip]
azurerm_virtual_network.main: Creation complete after 7s [id=/subscriptions/[secret]/resourceGroups/OOOOGAA_BOOGAAAAAAAA_xd/providers/Microsoft.Network/virtualNetworks/vm-vnet]
azurerm_subnet.main: Creating...
azurerm_subnet.main: Creation complete after 6s [id=/subscriptions/[secret]/resourceGroups/OOOOGAA_BOOGAAAAAAAA_xd/providers/Microsoft.Network/virtualNetworks/vm-vnet/subnets/vm-subnet]
azurerm_network_interface.main: Creating...
azurerm_network_interface.main: Creation complete after 5s [id=/subscriptions/[secret]/resourceGroups/OOOOGAA_BOOGAAAAAAAA_xd/providers/Microsoft.Network/networkInterfaces/vm-nic]
azurerm_linux_virtual_machine.main: Creating...
azurerm_linux_virtual_machine.main: Still creating... [00m10s elapsed]
azurerm_linux_virtual_machine.main: Still creating... [00m20s elapsed]
azurerm_linux_virtual_machine.main: Creation complete after 20s [id=/subscriptions/[secret]/resourceGroups/OOOOGAA_BOOGAAAAAAAA_xd/providers/Microsoft.Compute/virtualMachines/super-vm]

Apply complete! Resources: 6 added, 0 changed, 0 destroyed.
```

</details>

## 🌞 Prouvez avec une connexion SSH sur l'IP publique que la VM est up

```
saus@pop-os ~/tepeeeeqepfepmfeqpmo> ssh saus@[secret]
The authenticity of host '[secret] ([secret])' can't be established.
ED25519 key fingerprint is SHA256:[secret].
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[secret]' (ED25519) to the list of known hosts.
[saus@super-vm ~]$ waw
```

 <details>
  <summary> terraform.tfstate ( les autres sont inchange ) </summary>


```

saus@pop-os ~/tepeeeeqepfepmfeqpmo> cat terraform.tfstate 
{
  "version": 4,
  "terraform_version": "1.14.7",
  "serial": 7,
  "lineage": "[secret]",
  "outputs": {},
  "resources": [
    {
      "mode": "managed",
      "type": "azurerm_linux_virtual_machine",
      "name": "main",
      "provider": "provider[\"registry.terraform.io/hashicorp/azurerm\"]",
      "instances": [
        {
          "schema_version": 0,
          "attributes": {
            "additional_capabilities": [],
            "admin_password": null,
            ,
            "admin_username": "saus",
            "allow_extension_operations": true,
            "availability_set_id": "",
            "boot_diagnostics": [],
            "bypass_platform_safety_checks_on_user_schedule_enabled": false,
            "capacity_reservation_group_id": "",
            "computer_name": "super-vm",
            "custom_data": null,
            "dedicated_host_group_id": "",
            "dedicated_host_id": "",
            "disable_password_authentication": true,
            "disk_controller_type": "SCSI",
            "edge_zone": "",
            "encryption_at_host_enabled": false,
            "eviction_policy": "",
            "extensions_time_budget": "PT1H30M",
            "gallery_application": [],
            "id": "/subscriptions/[secret]/resourceGroups/OOOOGAA_BOOGAAAAAAAA_xd/providers/Microsoft.Compute/virtualMachines/super-vm",
            "identity": [],
            "license_type": "",
            "location": "denmarkeast",
            "max_bid_price": -1,
            "name": "super-vm",
            "network_interface_ids": [
              "/subscriptions/[secret]/resourceGroups/OOOOGAA_BOOGAAAAAAAA_xd/providers/Microsoft.Network/networkInterfaces/vm-nic"
            ],
            "os_disk": [
              {
                "caching": "ReadWrite",
                "diff_disk_settings": [],
                "disk_encryption_set_id": "",
                "disk_size_gb": 30,
                "id": "/subscriptions/[secret]/resourceGroups/OOOOGAA_BOOGAAAAAAAA_xd/providers/Microsoft.Compute/disks/vm-os-disk",
                "name": "vm-os-disk",
                "secure_vm_disk_encryption_set_id": "",
                "security_encryption_type": "",
                "storage_account_type": "Standard_LRS",
                "write_accelerator_enabled": false
              }
            ],
            "os_image_notification": [],
            "os_managed_disk_id": "/subscriptions/[secret]/resourceGroups/OOOOGAA_BOOGAAAAAAAA_xd/providers/Microsoft.Compute/disks/vm-os-disk",
            "patch_assessment_mode": "ImageDefault",
            "patch_mode": "ImageDefault",
            "plan": [],
            "platform_fault_domain": -1,
            "priority": "Regular",
            "private_ip_address": "10.0.1.4",
            "private_ip_addresses": [
              "10.0.1.4"
            ],
            "provision_vm_agent": true,
            "proximity_placement_group_id": "",
            "public_ip_address": "[secret]",
            "public_ip_addresses": [
              "[secret]"
            ],
            "reboot_setting": "",
            "resource_group_name": "OOOOGAA_BOOGAAAAAAAA_xd",
            "secret": [],
            "secure_boot_enabled": false,
            "size": "Standard_B1s",
            "source_image_id": "",
            "source_image_reference": [
              {
                "offer": "almalinux-x86_64",
                "publisher": "almalinux",
                "sku": "10-gen2",
                "version": "latest"
              }
            ],
            "tags": null,
            "termination_notification": null,
            "timeouts": null,
            "user_data": "",
            "virtual_machine_id": "[secret]",
            "virtual_machine_scale_set_id": "",
            "vm_agent_platform_updates_enabled": false,
            "vtpm_enabled": false,
            "zone": ""
          },
          "sensitive_attributes": [
            [
              {
                "type": "get_attr",
                "value": "admin_password"
              }
            ],
            [
              {
                "type": "get_attr",
                "value": "custom_data"
              }
            ]
          ],
          "identity_schema_version": 0,
          "private": "[secret]",
          "dependencies": [
            "azurerm_network_interface.main",
            "azurerm_public_ip.main",
            "azurerm_resource_group.main",
            "azurerm_subnet.main",
            "azurerm_virtual_network.main"
          ]
        }
      ]
    },
    {
      "mode": "managed",
      "type": "azurerm_network_interface",
      "name": "main",
      "provider": "provider[\"registry.terraform.io/hashicorp/azurerm\"]",
      "instances": [
        {
          "schema_version": 0,
          "attributes": {
            "accelerated_networking_enabled": false,
            "applied_dns_servers": [],
            "auxiliary_mode": "",
            "auxiliary_sku": "",
            "dns_servers": null,
            "edge_zone": "",
            "id": "/subscriptions/[secret]/resourceGroups/OOOOGAA_BOOGAAAAAAAA_xd/providers/Microsoft.Network/networkInterfaces/vm-nic",
            "internal_dns_name_label": "",
            "internal_domain_name_suffix": "[secret]",
            "ip_configuration": [
              {
                "gateway_load_balancer_frontend_ip_configuration_id": "",
                "name": "internal",
                "primary": true,
                "private_ip_address": "10.0.1.4",
                "private_ip_address_allocation": "Dynamic",
                "private_ip_address_version": "IPv4",
                "public_ip_address_id": "/subscriptions/[secret]/resourceGroups/OOOOGAA_BOOGAAAAAAAA_xd/providers/Microsoft.Network/publicIPAddresses/vm-ip",
                "subnet_id": "/subscriptions/[secret]/resourceGroups/OOOOGAA_BOOGAAAAAAAA_xd/providers/Microsoft.Network/virtualNetworks/vm-vnet/subnets/vm-subnet"
              }
            ],
            "ip_forwarding_enabled": false,
            "location": "denmarkeast",
            "mac_address": "",
            "name": "vm-nic",
            "private_ip_address": "10.0.1.4",
            "private_ip_addresses": [
              "10.0.1.4"
            ],
            "resource_group_name": "OOOOGAA_BOOGAAAAAAAA_xd",
            "tags": null,
            "timeouts": null,
            "virtual_machine_id": ""
          },
          "sensitive_attributes": [],
          "identity_schema_version": 0,
          "identity": {
            "name": "vm-nic",
            "resource_group_name": "OOOOGAA_BOOGAAAAAAAA_xd",
            "subscription_id": "[secret]"
          },
          "private": "[secret]",
          "dependencies": [
            "azurerm_public_ip.main",
            "azurerm_resource_group.main",
            "azurerm_subnet.main",
            "azurerm_virtual_network.main"
          ]
        }
      ]
    },
    {
      "mode": "managed",
      "type": "azurerm_public_ip",
      "name": "main",
      "provider": "provider[\"registry.terraform.io/hashicorp/azurerm\"]",
      "instances": [
        {
          "schema_version": 0,
          "attributes": {
            "allocation_method": "Static",
            "ddos_protection_mode": "VirtualNetworkInherited",
            "ddos_protection_plan_id": null,
            "domain_name_label": null,
            "domain_name_label_scope": null,
            "edge_zone": "",
            "fqdn": null,
            "id": "/subscriptions/[secret]/resourceGroups/OOOOGAA_BOOGAAAAAAAA_xd/providers/Microsoft.Network/publicIPAddresses/vm-ip",
            "idle_timeout_in_minutes": 4,
            "ip_address": "[secret]",
            "ip_tags": null,
            "ip_version": "IPv4",
            "location": "denmarkeast",
            "name": "vm-ip",
            "public_ip_prefix_id": null,
            "resource_group_name": "OOOOGAA_BOOGAAAAAAAA_xd",
            "reverse_fqdn": null,
            "sku": "Standard",
            "sku_tier": "Regional",
            "tags": null,
            "timeouts": null,
            "zones": null
          },
          "sensitive_attributes": [],
          "identity_schema_version": 0,
          "identity": {
            "name": "vm-ip",
            "resource_group_name": "OOOOGAA_BOOGAAAAAAAA_xd",
            "subscription_id": "[secret]"
          },
          "private": "[secret]",
          "dependencies": [
            "azurerm_resource_group.main"
          ]
        }
      ]
    },
    {
      "mode": "managed",
      "type": "azurerm_resource_group",
      "name": "main",
      "provider": "provider[\"registry.terraform.io/hashicorp/azurerm\"]",
      "instances": [
        {
          "schema_version": 0,
          "attributes": {
            "id": "/subscriptions/[secret]/resourceGroups/OOOOGAA_BOOGAAAAAAAA_xd",
            "location": "denmarkeast",
            "managed_by": "",
            "name": "OOOOGAA_BOOGAAAAAAAA_xd",
            "tags": null,
            "timeouts": null
          },
          "sensitive_attributes": [],
          "identity_schema_version": 0,
          "identity": {
            "name": "OOOOGAA_BOOGAAAAAAAA_xd",
            "subscription_id": "[secret]"
          },
          "private": "[secret]"
        }
      ]
    },
    {
      "mode": "managed",
      "type": "azurerm_subnet",
      "name": "main",
      "provider": "provider[\"registry.terraform.io/hashicorp/azurerm\"]",
      "instances": [
        {
          "schema_version": 0,
          "attributes": {
            "address_prefixes": [
              "10.0.1.0/24"
            ],
            "default_outbound_access_enabled": true,
            "delegation": [],
            "id": "/subscriptions/[secret]/resourceGroups/OOOOGAA_BOOGAAAAAAAA_xd/providers/Microsoft.Network/virtualNetworks/vm-vnet/subnets/vm-subnet",
            "ip_address_pool": [],
            "name": "vm-subnet",
            "private_endpoint_network_policies": "Disabled",
            "private_link_service_network_policies_enabled": true,
            "resource_group_name": "OOOOGAA_BOOGAAAAAAAA_xd",
            "service_endpoint_policy_ids": null,
            "service_endpoints": null,
            "sharing_scope": "",
            "timeouts": null,
            "virtual_network_name": "vm-vnet"
          },
          "sensitive_attributes": [],
          "identity_schema_version": 0,
          "identity": {
            "name": "vm-subnet",
            "resource_group_name": "OOOOGAA_BOOGAAAAAAAA_xd",
            "subscription_id": "[secret]",
            "virtual_network_name": "vm-vnet"
          },
          "private": "[secret]",
          "dependencies": [
            "azurerm_resource_group.main",
            "azurerm_virtual_network.main"
          ]
        }
      ]
    },
    {
      "mode": "managed",
      "type": "azurerm_virtual_network",
      "name": "main",
      "provider": "provider[\"registry.terraform.io/hashicorp/azurerm\"]",
      "instances": [
        {
          "schema_version": 0,
          "attributes": {
            "address_space": [
              "10.0.0.0/16"
            ],
            "bgp_community": "",
            "ddos_protection_plan": [],
            "dns_servers": [],
            "edge_zone": "",
            "encryption": [],
            "flow_timeout_in_minutes": 0,
            "guid": "[secret]",
            "id": "/subscriptions/[secret]/resourceGroups/OOOOGAA_BOOGAAAAAAAA_xd/providers/Microsoft.Network/virtualNetworks/vm-vnet",
            "ip_address_pool": [],
            "location": "denmarkeast",
            "name": "vm-vnet",
            "private_endpoint_vnet_policies": "Disabled",
            "resource_group_name": "OOOOGAA_BOOGAAAAAAAA_xd",
            "subnet": [],
            "tags": null,
            "timeouts": null
          },
          "sensitive_attributes": [],
          "identity_schema_version": 0,
          "identity": {
            "name": "vm-vnet",
            "resource_group_name": "OOOOGAA_BOOGAAAAAAAA_xd",
            "subscription_id": "[secret]"
          },
          "private": "[secret]",
          "dependencies": [
            "azurerm_resource_group.main"
          ]
        }
      ]
    }
  ],
  "check_results": null
}

```
</details>

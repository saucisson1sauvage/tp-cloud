## 🌞 Créez une VM azure 

```
az vm create -g meo -n super_vm --size Standard_B1s --image almalinux:almalinux-x86_64:10-gen2:10.1.20251
Consider upgrading security for your workloads using Azure Trusted Launch VMs. To know more about Trusted Launch, please visit https://aka.ms/TrustedLaunch.
{
  "fqdns": "",
  "id": "/subscriptions/939e58d5-d8d8-47ab-9014-d7324ae9ae23/resourceGroups/meo/providers/Microsoft.Compute/virtualMachines/super_vm",
  "location": "denmarkeast",
  "macAddress": "7C-ED-8D-6A-F3-00",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "9.205.156.154",
  "resourceGroup": "meo"
}
```
## 🌞 Connexion SSH
~~~
saus@pop-os ~> ssh saus@9.205.156.154
The authenticity of host '9.205.156.154 (9.205.156.154)' can't be established.
ED25519 key fingerprint is SHA256:pGLYVJpdlhHMSTfX6dggU1tCSqYveVOD88dBoYqk21s.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '9.205.156.154' (ED25519) to the list of known hosts.
[saus@supervm ~]$ :d
~~~

## 🌞 Effectuez la conf suivante :

` [saus@supervm ~]$ sudo dnf update -y && sudo dnf upgrade -y ` 

```
[saus@supervm ~]$ htop -V ; dig -v ; ping -V ; vim --version
htop 3.3.0
DiG 9.18.33
ping from iputils 20240905
libcap: yes, IDN: yes, NLS: yes, error.h: yes, getrandom(): yes, __fpending(): yes
VIM - Vi IMproved 9.1 (2024 Jan 02, compiled Feb 25 2026 00:00:00)
Included patches: 1-83
```

## 🌞 Go lancer ça :


~~~
[saus@supervm ~]$ sudo journalctl --flush --rotate --vacuum-time=1s
~~~

~~~
[saus@supervm ~]$ rm ./.bash_history 

~~~

## 🌞 Let's go, balancez :

~~~
az>> vm deallocate --resource-group meo --name super_vm
az>> vm generalize --resource-group meo --name super_vm
e-group meo --name alma_chad --source super_vm --hyper-v-generation V2
{
  "hyperVGeneration": "V2",
  "id": "/subscriptions/939e58d5-d8d8-47ab-9014-d7324ae9ae23/resourceGroups/meo/providers/Microsoft.Compute/images/alma_chad",
  "location": "denmarkeast",
  "name": "alma_chad",
  "provisioningState": "Succeeded",
  "resourceGroup": "meo",
  "sourceVirtualMachine": {
    "id": "/subscriptions/939e58d5-d8d8-47ab-9014-d7324ae9ae23/resourceGroups/meo/providers/Microsoft.Compute/virtualMachines/super_vm",
    "resourceGroup": "meo"
  },
  "storageProfile": {
    "dataDisks": [],
    "osDisk": {
      "caching": "ReadWrite",
      "diskSizeGB": 30,
      "managedDisk": {
        "id": "/subscriptions/939e58d5-d8d8-47ab-9014-d7324ae9ae23/resourceGroups/meo/providers/Microsoft.Compute/disks/super_vm_disk1_b22d1bd330a84b8aaddf99056ea689d0",
        "resourceGroup": "meo"
      },
      "osState": "Generalized",
      "osType": "Linux",
      "storageAccountType": "Premium_LRS"
    }
  },
  "tags": {},
  "type": "Microsoft.Compute/images"
}
~~~


## 🌞 Lancer une VM à partir de votre template

~~~
meo -n customvm --size Standard_B1s --image alma_chad --admin-username saus --ssh-key-values ~/.ssh/cloud
{
  "fqdns": "",
  "id": "/subscriptions/939e58d5-d8d8-47ab-9014-d7324ae9ae23/resourceGroups/meo/providers/Microsoft.Compute/virtualMachines/customvm",
  "location": "denmarkeast",
  "macAddress": "7C-ED-8D-6A-95-B6",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.5",
  "publicIpAddress": "9.205.157.16",
  "resourceGroup": "meo"
}
~~~


## 🌞 Vérification !

~~~
saus@pop-os ~> ssh saus@9.205.157.16
The authenticity of host '9.205.157.16 (9.205.157.16)' can't be established.
ED25519 key fingerprint is SHA256:vt2HQRY9dhczvVjzgVJCwPbrH2kUODC5TyFmTi8E58M.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '9.205.157.16' (ED25519) to the list of known hosts
~~~

~~~
[saus@customvm ~]$ vim --version                                              
VIM - Vi IMproved 9.1 (2024 Jan 02, compiled Feb 25 2026 00:00:00)
Included patches: 1-83
[saus@customvm ~]$ dig -v
DiG 9.18.33
[saus@customvm ~]$ ping -V
ping from iputils 20240905
[saus@customvm ~]$ htop -V
htop 3.3.0

[saus@customvm ~]$ cloud-init status
status: done
~~~



## 🌞 Créez une VM qui servira à créer le template
```
az vm create --resource-group rg-alma --name vm-alma --image almalinux:almalinux-x86_64:9-gen2:latest --admin-username azureuser --ssh-key-value ~/.ssh/id_ed25519.pub --public-ip-sku Standard --size Standard_D2s_v3
```
## 🌞 Firewall conf
```
[azureuser@vm-alma ~]$ sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: eth0
  sources:
  services:
  ports: 22/tcp
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

## 🌞 Proposez une conf OpenSSH forte
```
Port 22
LogLevel VERBOSE

PermitRootLogin no
MaxAuthTries 3
PubkeyAuthentication yes
PasswordAuthentication no
PermitEmptyPasswords no

KexAlgorithms curve25519-sha256@libssh.org
Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com
```

j arrivais pas a faire fonctionner convenablement fail2ban ;-;

## 🌞 Proposer une conf sysctl
```
net.ipv4.tcp_syncookies = 1
net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.all.accept_source_route = 0
```

## 🌞 Installer l'IDS AIDE

```
[azureuser@vm-alma ~]$ rpm -q aide
aide-0.16-105.el9.x86_64
[azureuser@vm-alma ~]$ tail -n 2 /etc/aide.conf
tail: cannot open '/etc/aide.conf' for reading: Permission denied
[azureuser@vm-alma ~]$ sudo tail -n 2 /etc/aide.conf
/etc/ssh/sshd_config p+i+n+u+g+s+m+c+md5+sha256
/etc/sysctl.d/99-security-hardening.conf p+i+n+u+g+s+m+c+md5+sha256
```


## 🌞 Initialiser la base de données AIDE
```
[azureuser@vm-alma ~]$ sudo aide --init
AIDE initialized database at /var/lib/aide/aide.db.new.gz
[azureuser@vm-alma ~]$ sudo mv /var/lib/aide/aide.db.new.gz /var/lib/aide/aide.db.gz
[azureuser@vm-alma ~]$ sudo ls -l /var/lib/aide/aide.db.gz
-rw-------. 1 root root 64523 Apr  7 23:43 /var/lib/aide/aide.db.gz
```

## 🌞 Jouer avec les tests d'intégrité AIDE
```
[azureuser@vm-alma ~]$ sudo aide --check
[azureuser@vm-alma ~]$ echo "# Corruption" | sudo tee -a /etc/sysctl.d/99-security-hardening.conf
Corruption
[azureuser@vm-alma ~]$ sudo aide --check
ba y a une dif la /etc/sysctl.d/99-security-hardening.conf
```


### 🌞 Créer un service systemd pour lancer un test AIDE
```
[azureuser@vm-alma ~]$ cat /etc/systemd/system/aide-test.service
[Unit]
Description=AIDE check

[Service]
Type=oneshot
ExecStart=/usr/sbin/aide --check
RemainAfterExit=true

[Install]
WantedBy=multi-user.target
```
## 🌞 Indiquer à systemd qu'on a modifié les services
```
[azureuser@vm-alma ~]$ sudo systemctl daemon-reload
```

## 🌞 Tester le service


[azureuser@vm-alma ~]$ sudo systemctl start aide-test


AH bah ca marche pas
bah bye bye pr ce soir '-'


## 🌞 Installer Docker votre machine Azure
```
saus@pop-os ~ (master)> docker info
Client: Docker Engine - Community
 Version:    29.3.0
 Context:    default
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.31.1
    Path:     /usr/libexec/docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v5.1.0
    Path:     /usr/libexec/docker/cli-plugins/docker-compose
```

```
saus@pop-os ~ (master) [1]> sudo systemctl start docker
saus@pop-os ~ (master) [0|1]> pidstat | grep docker
09:21:59 AM     0      2373    0.02    0.00    0.00    0.00    0.02     3  dockerd
```

## 🌞 Utiliser la commande docker run

` docker run -p 80:80 nginx `

` docker run -p 9999:80 nginx `


## 🌞 Rendre le service dispo sur internet



```html
saus@pop-os ~ [SIGINT]> curl localhost:9999
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, nginx is successfully installed and working.
Further configuration is required for the web server, reverse proxy, 
API gateway, load balancer, content cache, or other features.</p>

<p>For online documentation and support please refer to
<a href="https://nginx.org/">nginx.org</a>.<br/>
To engage with the community please visit
<a href="https://community.nginx.org/">community.nginx.org</a>.<br/>
For enterprise grade support, professional services, additional 
security features and capabilities please refer to
<a href="https://f5.com/nginx">f5.com/nginx</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

## 🌞 Custom un peu le lancement du conteneur

```
docker run -d -m 512m --name meow -v /home/saus/tepeeeeqepfepmfeqpmo/tkt.conf:/etc/nginx/conf.d/default.conf -v /home/saus/tepeeeeqepfepmfeqpmo/ndex.html:/var/www/tp_docker/index.html -p 7777:7777 nginx
```
```html
saus@pop-os ~> curl http://localhost:7777
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <p>ooga booga</p>
</body>
</html>
```

## 🌞 Construire votre propre image
## 🌞 Dans les deux cas, j'attends juste votre Dockerfile dans le compte-rendu

```
FROM alpine

RUN apk update

RUN apk add apache2

COPY apache2.conf /etc/apache2/apache2.conf
COPY ndex.html /var/www/localhost/htdocs/index.html

CMD ["httpd", "-D", "FOREGROUND"]
```

## 🌞 Installez un WikiJS en utilisant Docker

```
saus@pop-os ~/tepeeeeqepfepmfeqpmo> curl http://localhost:80
<!DOCTYPE html><html lang="en"><head><meta http-equiv="X-UA-Compatible" content="IE=edge"><meta charset="UTF-8"><meta name="viewport" content="user-scalable=yes, width=device-width, initial-scale=1, maximum-scale=5"><meta name="theme-color" content="#1976d2"><meta name="msapplication-TileColor" content="#1976d2"><meta name="msapplication-TileImage" content="/_assets/favicons/mstile-150x150.png"><title>Welcome | Wiki.js</title><meta name="description"><meta property="og:title" content="Welcome"><meta property="og:type" content="website"><meta property="og:description"><meta property="og:image"><meta property="og:url" content="https://wiki.yourdomain.com/"><meta property="og:site_name" content="Wiki.js"><link rel="apple-touch-icon" sizes="180x180" href="/_assets/favicons/apple-touch-icon.png"><link rel="icon" type="image/png" sizes="192x192" href="/_assets/favicons/android-chrome-192x192.png"><link rel="icon" type="image/png" sizes="32x32" href="/_assets/favicons/favicon-32x32.png"><link rel="icon" type="image/png" sizes="16x16" href="/_assets/favicons/favicon-16x16.png"><link rel="mask-icon" href="/_assets/favicons/safari-pinned-tab.svg" color="#1976d2"><link rel="manifest" href="/_assets/manifest.json"><script>var siteConfig = {"title":"Wiki.js","theme":"default","darkMode":false,"tocPosition":"left","lang":"en","rtl":false,"company":"","contentLicense":"","footerOverride":"","logoUrl":"https://static.requarks.io/logo/wikijs-butterfly.svg"}
var siteLangs = []
</script><link type="text/css" rel="stylesheet" href="/_assets/css/app.7e4d2a2ea92425e9d187.css"><script type="text/javascript" src="/_assets/js/runtime.js?1770864460"></script><script type="text/javascript" src="/_assets/js/app.js?1770864460"></script></head><body><div class="is-fullscreen" id="root"><welcome locale="en"></welcome></div></body></html>
```

## 🌞 Vous devez :

```
FROM python

WORKDIR /app

COPY requirements.txt .

RUN pip install -r requirements.txt

COPY app.py .

COPY index.html ./templates/

EXPOSE 8888

CMD ["python", "app.py"]
```

```yaml
version: "3.8"

services:
  app:
    build: .
    ports:
      - "8888:8888"
    depends_on:
      - db

  db:
    image: redis:7-alpine
 ```

 ## 🌞 Prouvez que vous pouvez devenir root

 ` saus@pop-os ~> docker run -v /:/host-root alpine cat /host-root/etc/shadow `

## 🌞 Utilisez Trivy

` trivy image rahh ` ( rahh = app python )

 <details>
  <summary>pas bien</summary>

  ca c'est fait crop xd

 ────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-71202      │          │ affected     │                                      │                       │ kernel: Linux kernel: Memory Corruption and Kernel Crashes   │
│                              │                     │          │              │                                      │                       │ via IOMMU SVA coherency...                                   │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-71202                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-71227      │          │              │                                      │                       │ kernel: wifi: mac80211: don't WARN for connections on        │
│                              │                     │          │              │                                      │                       │ invalid channels                                             │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-71227                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-71239      │          │              │                                      │                       │ kernel: audit: add fchmodat2() to change attributes class    │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-71239                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-22981      │          │              │                                      │                       │ kernel: idpf: detach and close netdevs while handling a      │
│                              │                     │          │              │                                      │                       │ reset                                                        │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-22981                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-22985      │          │              │                                      │                       │ kernel: idpf: Fix RSS LUT NULL pointer crash on early        │
│                              │                     │          │              │                                      │                       │ ethtool operations...                                        │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-22985                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-22986      │          │              │                                      │                       │ kernel: Linux kernel: Denial of Service due to a race        │
│                              │                     │          │              │                                      │                       │ condition in...                                              │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-22986                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-22993      │          │              │                                      │                       │ kernel: idpf: Fix RSS LUT NULL ptr issue after soft reset    │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-22993                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-23004      │          │              │                                      │                       │ kernel: dst: fix races in rt6_uncached_list_del() and        │
│                              │                     │          │              │                                      │                       │ rt_del_uncached_list()                                       │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-23004                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-23007      │          │              │                                      │                       │ kernel: block: zero non-PI portion of auto integrity buffer  │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-23007                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-23017      │          │              │                                      │                       │ kernel: idpf: fix error handling in the init_task on load    │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-23017                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-23066      │          │              │                                      │                       │ kernel: Linux kernel: Denial of Service via unsafe requeue   │
│                              │                     │          │              │                                      │                       │ in rxrpc_recvmsg                                             │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-23066                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-23070      │          │              │                                      │                       │ kernel: Octeontx2-af: Add proper checks for fwdata           │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-23070                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-23102      │          │              │                                      │                       │ kernel: Linux kernel: Denial of Service due to incorrect SVE │
│                              │                     │          │              │                                      │                       │ context restoration...                                       │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-23102                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-23104      │          │              │                                      │                       │ kernel: ice: fix devlink reload call trace                   │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-23104                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-23137      │          │              │                                      │                       │ kernel: of: unittest: Fix memory leak in unittest_data_add() │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-23137                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-23138      │          │              │                                      │                       │ kernel: tracing: Add recursion protection in kernel stack    │
│                              │                     │          │              │                                      │                       │ trace recording                                              │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-23138                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-23152      │          │              │                                      │                       │ kernel: wifi: mac80211: correctly decode TTLM with default   │
│                              │                     │          │              │                                      │                       │ link map                                                     │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-23152                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-23157      │          │              │                                      │                       │ kernel: btrfs: do not strictly require dirty metadata        │
│                              │                     │          │              │                                      │                       │ threshold for metadata writepages...                         │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-23157                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-23207      │          │              │                                      │                       │ kernel: spi: tegra210-quad: Protect curr_xfer check in IRQ   │
│                              │                     │          │              │                                      │                       │ handler                                                      │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-23207                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-23210      │          │              │                                      │                       │ kernel: Linux kernel: Denial of Service in ice driver due to │
│                              │                     │          │              │                                      │                       │ race...                                                      │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-23210                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-23217      │          │              │                                      │                       │ kernel: riscv: trace: fix snapshot deadlock with sbi ecall   │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-23217                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-23239      │          │              │                                      │                       │ kernel: Kernel: Race condition in espintcp can lead to       │
│                              │                     │          │              │                                      │                       │ denial of service...                                         │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-23239                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-23240      │          │              │                                      │                       │ kernel: Linux kernel: Denial of service due to a race        │
│                              │                     │          │              │                                      │                       │ condition in...                                              │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-23240                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-23244      │          │              │                                      │                       │ kernel: nvme: fix memory allocation in nvme_pr_read_keys()   │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-23244                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-23246      │          │              │                                      │                       │ kernel: Linux kernel: Denial of Service in mac80211 Wi-Fi    │
│                              │                     │          │              │                                      │                       │ due to out-of-bounds...                                      │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-23246                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-23247      │          │              │                                      │                       │ kernel: tcp: secure_seq: add back ports to TS offset         │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-23247                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-23252      │          │              │                                      │                       │ kernel: xfs: get rid of the xchk_xfile_*_descr calls         │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-23252                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-23259      │          │              │                                      │                       │ kernel: io_uring/rw: free potentially allocated iovec on     │
│                              │                     │          │              │                                      │                       │ cache put failure                                            │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-23259                   │
│                              ├─────────────────────┼──────────┤              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2004-0230       │ LOW      │              │                                      │                       │ TCP, when using a large Window Size, makes it easier for     │
│                              │                     │          │              │                                      │                       │ remote...                                                    │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2004-0230                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2005-3660       │          │              │                                      │                       │ Linux kernel 2.4 and 2.6 allows attackers to cause a denial  │
│                              │                     │          │              │                                      │                       │ of...                                                        │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2005-3660                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2007-3719       │          │              │                                      │                       │ kernel: secretly Monopolizing the CPU Without Superuser      │
│                              │                     │          │              │                                      │                       │ Privileges                                                   │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2007-3719                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2008-2544       │          │              │                                      │                       │ kernel: mounting proc readonly on a different mount point    │
│                              │                     │          │              │                                      │                       │ silently mounts it...                                        │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2008-2544                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2008-4609       │          │              │                                      │                       │ kernel: TCP protocol vulnerabilities from Outpost24          │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2008-4609                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2010-4563       │          │              │                                      │                       │ kernel: ipv6: sniffer detection                              │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2010-4563                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2010-5321       │          │              │                                      │                       │ kernel: v4l: videobuf: hotfix a bug on multiple calls to     │
│                              │                     │          │              │                                      │                       │ mmap()                                                       │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2010-5321                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2011-4915       │          │              │                                      │                       │ fs/proc/base.c in the Linux kernel through 3.1 allows local  │
│                              │                     │          │              │                                      │                       │ users to o...                                                │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2011-4915                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2011-4916       │          │              │                                      │                       │ Linux kernel through 3.1 allows local users to obtain        │
│                              │                     │          │              │                                      │                       │ sensitive keystr ......                                      │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2011-4916                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2011-4917       │          │              │                                      │                       │ In the Linux kernel through 3.1 there is an information      │
│                              │                     │          │              │                                      │                       │ disclosure iss...                                            │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2011-4917                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2012-4542       │          │              │                                      │                       │ kernel: block: default SCSI command filter does not          │
│                              │                     │          │              │                                      │                       │ accomodate commands overlap across...                        │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2012-4542                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2014-9892       │          │              │                                      │                       │ The snd_compr_tstamp function in                             │
│                              │                     │          │              │                                      │                       │ sound/core/compress_offload.c in the ...                     │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2014-9892                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2014-9900       │          │              │                                      │                       │ kernel: Info leak in uninitialized structure ethtool_wolinfo │
│                              │                     │          │              │                                      │                       │ in ethtool_get_wol()                                         │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2014-9900                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2015-2877       │          │              │                                      │                       │ Kernel: Cross-VM ASL INtrospection (CAIN)                    │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2015-2877                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2016-10723      │          │              │                                      │                       │ An issue was discovered in the Linux kernel through 4.17.2.  │
│                              │                     │          │              │                                      │                       │ Since the...                                                 │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2016-10723                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2016-8660       │          │              │                                      │                       │ kernel: xfs: local DoS due to a page lock order bug in...    │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2016-8660                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2017-0630       │          │              │                                      │                       │ kernel: Information disclosure vulnerability in kernel trace │
│                              │                     │          │              │                                      │                       │ subsystem                                                    │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2017-0630                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2017-13693      │          │              │                                      │                       │ kernel: ACPI operand cache leak in dsutils.c                 │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2017-13693                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2017-13694      │          │              │                                      │                       │ kernel: ACPI node and node_ext cache leak                    │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2017-13694                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2018-1121       │          │              │                                      │                       │ procps: process hiding through race condition enumerating    │
│                              │                     │          │              │                                      │                       │ /proc                                                        │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2018-1121                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2018-12928      │          │              │                                      │                       │ kernel: NULL pointer dereference in hfs_ext_read_extent in   │
│                              │                     │          │              │                                      │                       │ hfs.ko                                                       │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2018-12928                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2018-17977      │          │              │                                      │                       │ kernel: Mishandled interactions among XFRM Netlink messages, │
│                              │                     │          │              │                                      │                       │ IPPROTO_AH packets, and IPPROTO_IP packets...                │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2018-17977                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2019-11191      │          │              │                                      │                       │ kernel: race condition in load_aout_binary() allows local    │
│                              │                     │          │              │                                      │                       │ users to bypass ASLR on...                                   │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2019-11191                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2019-12378      │          │              │                                      │                       │ kernel: unchecked kmalloc of new_ra in ip6_ra_control leads  │
│                              │                     │          │              │                                      │                       │ to denial of service...                                      │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2019-12378                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2019-12379      │          │              │                                      │                       │ kernel: memory leak in con_insert_unipair in                 │
│                              │                     │          │              │                                      │                       │ drivers/tty/vt/consolemap.c                                  │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2019-12379                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2019-12380      │          │              │                                      │                       │ kernel: memory allocation failure in the efi subsystem leads │
│                              │                     │          │              │                                      │                       │ to denial of...                                              │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2019-12380                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2019-12381      │          │              │                                      │                       │ kernel: unchecked kmalloc of new_ra in ip_ra_control leads   │
│                              │                     │          │              │                                      │                       │ to denial of service...                                      │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2019-12381                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2019-12382      │          │              │                                      │                       │ kernel: unchecked kstrdup of fwstr in drm_load_edid_firmware │
│                              │                     │          │              │                                      │                       │ leads to denial of service...                                │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2019-12382                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2019-12455      │          │              │                                      │                       │ kernel: null pointer dereference in sunxi_divs_clk_setup in  │
│                              │                     │          │              │                                      │                       │ drivers/clk/sunxi/clk-sunxi.c causing denial of service...   │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2019-12455                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2019-12456      │          │              │                                      │                       │ kernel: double fetch in the MPT3COMMAND case in              │
│                              │                     │          │              │                                      │                       │ _ctl_ioctl_main in drivers/scsi/mpt3sas/mpt3sas_ctl.c        │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2019-12456                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2019-16229      │          │              │                                      │                       │ kernel: null pointer dereference in                          │
│                              │                     │          │              │                                      │                       │ drivers/gpu/drm/amd/amdkfd/kfd_interrupt.c                   │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2019-16229                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2019-16230      │          │              │                                      │                       │ kernel: null pointer dereference in                          │
│                              │                     │          │              │                                      │                       │ drivers/gpu/drm/radeon/radeon_display.c                      │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2019-16230                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2019-16231      │          │              │                                      │                       │ kernel: null-pointer dereference in                          │
│                              │                     │          │              │                                      │                       │ drivers/net/fjes/fjes_main.c                                 │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2019-16231                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2019-16232      │          │              │                                      │                       │ kernel: null-pointer dereference in                          │
│                              │                     │          │              │                                      │                       │ drivers/net/wireless/marvell/libertas/if_sdio.c              │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2019-16232                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2019-16233      │          │              │                                      │                       │ kernel: null pointer dereference in                          │
│                              │                     │          │              │                                      │                       │ drivers/scsi/qla2xxx/qla_os.c                                │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2019-16233                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2019-16234      │          │              │                                      │                       │ kernel: null pointer dereference in                          │
│                              │                     │          │              │                                      │                       │ drivers/net/wireless/intel/iwlwifi/pcie/trans.c              │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2019-16234                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2019-19070      │          │              │                                      │                       │ kernel: A memory leak in the spi_gpio_probe() function in    │
│                              │                     │          │              │                                      │                       │ drivers/spi/spi-gpio.c allows for...                         │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2019-19070                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2019-19378      │          │              │                                      │                       │ kernel: out-of-bounds write in index_rbio_pages in           │
│                              │                     │          │              │                                      │                       │ fs/btrfs/raid56.c                                            │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2019-19378                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2020-11725      │          │              │                                      │                       │ kernel: improper handling of private_size*count              │
│                              │                     │          │              │                                      │                       │ multiplication due to count=info->owner typo                 │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2020-11725                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2020-35501      │          │              │                                      │                       │ kernel: audit not logging access to syscall                  │
│                              │                     │          │              │                                      │                       │ open_by_handle_at for users with CAP_DAC_READ_SEARCH...      │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2020-35501                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2021-26934      │          │              │                                      │                       │ An issue was discovered in the Linux kernel 4.18 through     │
│                              │                     │          │              │                                      │                       │ 5.10.16, as...                                               │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2021-26934                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2021-3714       │          │              │                                      │                       │ kernel: Remote Page Deduplication Attacks                    │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2021-3714                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2022-0400       │          │              │                                      │                       │ kernel: Out of bounds read in the smc protocol stack         │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2022-0400                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2022-1247       │          │              │                                      │                       │ kernel: A race condition bug in rose_connect()               │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2022-1247                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2022-25265      │          │              │                                      │                       │ kernel: Executable Space Protection Bypass                   │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2022-25265                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2022-2961       │          │              │                                      │                       │ kernel: race condition in rose_bind()                        │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2022-2961                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2022-3238       │          │              │                                      │                       │ kernel: ntfs3 local privledge escalation if NTFS character   │
│                              │                     │          │              │                                      │                       │ set and remount and...                                       │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2022-3238                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2022-41848      │          │              │                                      │                       │ kernel: Race condition between mgslpc_ioctl and              │
│                              │                     │          │              │                                      │                       │ mgslpc_detach                                                │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2022-41848                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2022-44032      │          │              │                                      │                       │ Kernel: Race between cmm_open() and cm4000_detach() result   │
│                              │                     │          │              │                                      │                       │ in UAF                                                       │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2022-44032                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2022-44033      │          │              │                                      │                       │ Kernel: A race condition between cm4040_open() and           │
│                              │                     │          │              │                                      │                       │ reader_detach() may result in UAF...                         │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2022-44033                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2022-4543       │          │              │                                      │                       │ kernel: KASLR Prefetch Bypass Breaks KPTI                    │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2022-4543                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2022-45884      │          │              │                                      │                       │ kernel: use-after-free due to race condition occurring in    │
│                              │                     │          │              │                                      │                       │ dvb_register_device()                                        │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2022-45884                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2022-45885      │          │              │                                      │                       │ kernel: use-after-free due to race condition occurring in    │
│                              │                     │          │              │                                      │                       │ dvb_frontend.c                                               │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2022-45885                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2023-23039      │          │              │                                      │                       │ kernel: tty: vcc: race condition leading to use-after-free   │
│                              │                     │          │              │                                      │                       │ in vcc_open()                                                │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2023-23039                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2023-26242      │          │              │                                      │                       │ afu_mmio_region_get_by_offset in                             │
│                              │                     │          │              │                                      │                       │ drivers/fpga/dfl-afu-region.c in the ...                     │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2023-26242                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2023-31081      │          │              │                                      │                       │ An issue was discovered in                                   │
│                              │                     │          │              │                                      │                       │ drivers/media/test-drivers/vidtv/vidtv_brid ...              │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2023-31081                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2023-31085      │          │              │                                      │                       │ kernel: divide-by-zero error in ctrl_cdev_ioctl when do_div  │
│                              │                     │          │              │                                      │                       │ happens and erasesize is 0...                                │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2023-31085                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2023-3640       │          │              │                                      │                       │ Kernel: x86/mm: a per-cpu entry area leak was identified     │
│                              │                     │          │              │                                      │                       │ through the init_cea_offsets...                              │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2023-3640                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2024-0564       │          │              │                                      │                       │ kernel: max page sharing of Kernel Samepage Merging (KSM)    │
│                              │                     │          │              │                                      │                       │ may cause memory...                                          │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2024-0564                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-40113      │          │              │                                      │                       │ kernel: remoteproc: qcom: pas: Shutdown lite ADSP DTB on X1E │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-40113                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-68751      │          │              │                                      │                       │ kernel: s390/fpu: Fix false-positive kmsan report in         │
│                              │                     │          │              │                                      │                       │ fpu_vstl()                                                   │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-68751                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ TEMP-0000000-F7A20F │          │              │                                      │                       │ [Kernel: Unprivileged user can freeze journald]              │
│                              │                     │          │              │                                      │                       │ https://security-tracker.debian.org/tracker/TEMP-0000000-F7- │
│                              │                     │          │              │                                      │                       │ A20F                                                         │
│                              ├─────────────────────┼──────────┤              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-71265      │ UNKNOWN  │              │                                      │                       │ kernel: fs: ntfs3: fix infinite loop in attr_load_runs_range │
│                              │                     │          │              │                                      │                       │ on inconsistent metadata                                     │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-71265                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-71266      │          │              │                                      │                       │ kernel: fs: ntfs3: check return value of indx_find to avoid  │
│                              │                     │          │              │                                      │                       │ infinite loop...                                             │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-71266                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-71267      │          │              │                                      │                       │ kernel: fs: ntfs3: fix infinite loop triggered by zero-sized │
│                              │                     │          │              │                                      │                       │ ATTR_LIST                                                    │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-71267                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-23245      │          │              │                                      │                       │ kernel: net/sched: act_gate: snapshot parameters with RCU on │
│                              │                     │          │              │                                      │                       │ replace                                                      │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-23245                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-23250      │          │              │                                      │                       │ In the Linux kernel, the following vulnerability has been    │
│                              │                     │          │              │                                      │                       │ resolved: x...                                               │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-23250                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-23253      │          │              │                                      │                       │ In the Linux kernel, the following vulnerability has been    │
│                              │                     │          │              │                                      │                       │ resolved: m...                                               │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-23253                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-23265      │          │              │                                      │                       │ kernel: f2fs: fix to do sanity check on node footer in       │
│                              │                     │          │              │                                      │                       │ {read,write}_end_io...                                       │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-23265                   │
├──────────────────────────────┼─────────────────────┼──────────┤              ├──────────────────────────────────────┼───────────────────────┼──────────────────────────────────────────────────────────────┤
│ login                        │ CVE-2022-0563       │ LOW      │              │ 1:4.16.0-2+really2.41-5              │                       │ util-linux: partial disclosure of arbitrary files in chfn    │
│                              │                     │          │              │                                      │                       │ and chsh when compiled...                                    │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2022-0563                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-14104      │          │              │                                      │                       │ util-linux: util-linux: Heap buffer overread in setpwnam()   │
│                              │                     │          │              │                                      │                       │ when processing 256-byte usernames                           │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-14104                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-3184       │          │              │                                      │                       │ util-linux: util-linux: Access control bypass due to         │
│                              │                     │          │              │                                      │                       │ improper hostname canonicalization                           │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-3184                    │
├──────────────────────────────┼─────────────────────┤          │              ├──────────────────────────────────────┼───────────────────────┼──────────────────────────────────────────────────────────────┤
│ login.defs                   │ CVE-2007-5686       │          │              │ 1:4.17.4-2                           │                       │ initscripts in rPath Linux 1 sets insecure permissions for   │
│                              │                     │          │              │                                      │                       │ the /var/lo ......                                           │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2007-5686                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2024-56433      │          │              │                                      │                       │ shadow-utils: Default subordinate ID configuration in        │
│                              │                     │          │              │                                      │                       │ /etc/login.defs could lead to compromise                     │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2024-56433                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ TEMP-0628843-DBAD28 │          │              │                                      │                       │ [more related to CVE-2005-4890]                              │
│                              │                     │          │              │                                      │                       │ https://security-tracker.debian.org/tracker/TEMP-0628843-DB- │
│                              │                     │          │              │                                      │                       │ AD28                                                         │
├──────────────────────────────┼─────────────────────┤          │              ├──────────────────────────────────────┼───────────────────────┼──────────────────────────────────────────────────────────────┤
│ m4                           │ CVE-2008-1687       │          │              │ 1.4.19-8                             │                       │ m4: unquoted output of maketemp and mkstemp                  │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2008-1687                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2008-1688       │          │              │                                      │                       │ m4: code execution via -F argument                           │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2008-1688                    │
├──────────────────────────────┼─────────────────────┤          │              ├──────────────────────────────────────┼───────────────────────┼──────────────────────────────────────────────────────────────┤
│ mount                        │ CVE-2022-0563       │          │              │ 2.41-5                               │                       │ util-linux: partial disclosure of arbitrary files in chfn    │
│                              │                     │          │              │                                      │                       │ and chsh when compiled...                                    │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2022-0563                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-14104      │          │              │                                      │                       │ util-linux: util-linux: Heap buffer overread in setpwnam()   │
│                              │                     │          │              │                                      │                       │ when processing 256-byte usernames                           │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-14104                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-3184       │          │              │                                      │                       │ util-linux: util-linux: Access control bypass due to         │
│                              │                     │          │              │                                      │                       │ improper hostname canonicalization                           │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-3184                    │
├──────────────────────────────┼─────────────────────┤          │              ├──────────────────────────────────────┼───────────────────────┼──────────────────────────────────────────────────────────────┤
│ ncurses-base                 │ CVE-2025-6141       │          │              │ 6.5+20250216-2                       │                       │ gnu-ncurses: ncurses Stack Buffer Overflow                   │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-6141                    │
├──────────────────────────────┤                     │          │              │                                      ├───────────────────────┤                                                              │
│ ncurses-bin                  │                     │          │              │                                      │                       │                                                              │
│                              │                     │          │              │                                      │                       │                                                              │
├──────────────────────────────┼─────────────────────┼──────────┤              ├──────────────────────────────────────┼───────────────────────┼──────────────────────────────────────────────────────────────┤
│ openssh-client               │ CVE-2026-3497       │ HIGH     │              │ 1:10.0p1-7+deb13u1                   │                       │ openssh: OpenSSH GSSAPI: Information disclosure or denial of │
│                              │                     │          │              │                                      │                       │ service due to uninitialized...                              │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-3497                    │
│                              ├─────────────────────┼──────────┤              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2007-2243       │ LOW      │              │                                      │                       │ OpenSSH 4.6 and earlier, when                                │
│                              │                     │          │              │                                      │                       │ ChallengeResponseAuthentication is enabl ...                 │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2007-2243                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2007-2768       │          │              │                                      │                       │ OpenSSH, when using OPIE (One-Time Passwords in Everything)  │
│                              │                     │          │              │                                      │                       │ for PAM, a ......                                            │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2007-2768                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2008-3234       │          │              │                                      │                       │ sshd in OpenSSH 4 on Debian GNU/Linux, and the 20070303      │
│                              │                     │          │              │                                      │                       │ OpenSSH snapsh...                                            │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2008-3234                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2016-20012      │          │              │                                      │                       │ openssh: Public key information leak                         │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2016-20012                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2018-15919      │          │              │                                      │                       │ openssh: User enumeration via malformed packets in           │
│                              │                     │          │              │                                      │                       │ authentication requests                                      │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2018-15919                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2019-6110       │          │              │                                      │                       │ openssh: Acceptance and display of arbitrary stderr allows   │
│                              │                     │          │              │                                      │                       │ for spoofing of scp...                                       │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2019-6110                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2020-14145      │          │              │                                      │                       │ openssh: Observable discrepancy leading to an information    │
│                              │                     │          │              │                                      │                       │ leak in the algorithm negotiation...                         │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2020-14145                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2020-15778      │          │              │                                      │                       │ openssh: scp allows command injection when using backtick    │
│                              │                     │          │              │                                      │                       │ characters in the destination...                             │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2020-15778                   │
├──────────────────────────────┼─────────────────────┤          │              ├──────────────────────────────────────┼───────────────────────┼──────────────────────────────────────────────────────────────┤
│ openssl                      │ CVE-2026-2673       │          │              │ 3.5.5-1~deb13u1                      │                       │ openssl: OpenSSL TLS 1.3 server may choose unexpected key    │
│                              │                     │          │              │                                      │                       │ agreement group                                              │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-2673                    │
├──────────────────────────────┤                     │          │              │                                      ├───────────────────────┤                                                              │
│ openssl-provider-legacy      │                     │          │              │                                      │                       │                                                              │
│                              │                     │          │              │                                      │                       │                                                              │
│                              │                     │          │              │                                      │                       │                                                              │
├──────────────────────────────┼─────────────────────┤          │              ├──────────────────────────────────────┼───────────────────────┼──────────────────────────────────────────────────────────────┤
│ passwd                       │ CVE-2007-5686       │          │              │ 1:4.17.4-2                           │                       │ initscripts in rPath Linux 1 sets insecure permissions for   │
│                              │                     │          │              │                                      │                       │ the /var/lo ......                                           │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2007-5686                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2024-56433      │          │              │                                      │                       │ shadow-utils: Default subordinate ID configuration in        │
│                              │                     │          │              │                                      │                       │ /etc/login.defs could lead to compromise                     │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2024-56433                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ TEMP-0628843-DBAD28 │          │              │                                      │                       │ [more related to CVE-2005-4890]                              │
│                              │                     │          │              │                                      │                       │ https://security-tracker.debian.org/tracker/TEMP-0628843-DB- │
│                              │                     │          │              │                                      │                       │ AD28                                                         │
├──────────────────────────────┼─────────────────────┤          │              ├──────────────────────────────────────┼───────────────────────┼──────────────────────────────────────────────────────────────┤
│ patch                        │ CVE-2010-4651       │          │              │ 2.8-2                                │                       │ patch: directory traversal flaw allows for arbitrary file    │
│                              │                     │          │              │                                      │                       │ creation                                                     │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2010-4651                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2018-6951       │          │              │                                      │                       │ patch: NULL pointer dereference in pch.c:intuit_diff_type()  │
│                              │                     │          │              │                                      │                       │ causes a crash                                               │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2018-6951                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2018-6952       │          │              │                                      │                       │ patch: Double free of memory in pch.c:another_hunk() causes  │
│                              │                     │          │              │                                      │                       │ a crash                                                      │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2018-6952                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2021-45261      │          │              │                                      │                       │ patch: Invalid Pointer via another_hunk function             │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2021-45261                   │
├──────────────────────────────┼─────────────────────┤          │              ├──────────────────────────────────────┼───────────────────────┼──────────────────────────────────────────────────────────────┤
│ perl                         │ CVE-2011-4116       │          │              │ 5.40.1-6                             │                       │ perl: File:: Temp insecure temporary file handling           │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2011-4116                    │
├──────────────────────────────┤                     │          │              │                                      ├───────────────────────┤                                                              │
│ perl-base                    │                     │          │              │                                      │                       │                                                              │
│                              │                     │          │              │                                      │                       │                                                              │
├──────────────────────────────┤                     │          │              │                                      ├───────────────────────┤                                                              │
│ perl-modules-5.40            │                     │          │              │                                      │                       │                                                              │
│                              │                     │          │              │                                      │                       │                                                              │
├──────────────────────────────┼─────────────────────┼──────────┤              ├──────────────────────────────────────┼───────────────────────┼──────────────────────────────────────────────────────────────┤
│ python3.13                   │ CVE-2025-13836      │ HIGH     │              │ 3.13.5-2                             │                       │ cpython: Excessive read buffering DoS in http.client         │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-13836                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-15366      │          │              │                                      │                       │ cpython: IMAP command injection in user-controlled commands  │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-15366                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-15367      │          │              │                                      │                       │ cpython: POP3 command injection in user-controlled commands  │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-15367                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-8194       │          │              │                                      │                       │ cpython: Cpython infinite loop when parsing a tarfile        │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-8194                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-1299       │          │              │                                      │                       │ cpython: email header injection due to unquoted newlines     │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-1299                    │
│                              ├─────────────────────┼──────────┤              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-11468      │ MEDIUM   │              │                                      │                       │ cpython: Missing character filtering in Python               │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-11468                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-12084      │          │              │                                      │                       │ cpython: python: cpython: Quadratic algorithm in             │
│                              │                     │          │              │                                      │                       │ xml.dom.minidom leads to denial of service...                │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-12084                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-12781      │          │              │                                      │                       │ cpython: base64.b64decode() always accepts "+/" characters,  │
│                              │                     │          │              │                                      │                       │ despite setting altchars                                     │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-12781                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-13837      │          │              │                                      │                       │ cpython: Out-of-memory when loading Plist                    │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-13837                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-15282      │          │              │                                      │                       │ cpython: Header injection via newlines in data URL mediatype │
│                              │                     │          │              │                                      │                       │ in Python                                                    │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-15282                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-6069       │          │              │                                      │                       │ cpython: Python HTMLParser quadratic complexity              │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-6069                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-6075       │          │              │                                      │                       │ python: Quadratic complexity in os.path.expandvars() with    │
│                              │                     │          │              │                                      │                       │ user-controlled template                                     │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-6075                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-8291       │          │              │                                      │                       │ cpython: python: Python zipfile End of Central Directory     │
│                              │                     │          │              │                                      │                       │ (EOCD) Locator record offset...                              │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-8291                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-0672       │          │              │                                      │                       │ cpython: Header injection in http.cookies.Morsel in Python   │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-0672                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-0865       │          │              │                                      │                       │ cpython: wsgiref.headers.Headers allows header newline       │
│                              │                     │          │              │                                      │                       │ injection in Python                                          │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-0865                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-3644       │          │              │                                      │                       │ cpython: Incomplete control character validation in          │
│                              │                     │          │              │                                      │                       │ http.cookies                                                 │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-3644                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-4224       │          │              │                                      │                       │ cpython: Stack overflow parsing XML with deeply nested DTD   │
│                              │                     │          │              │                                      │                       │ content models                                               │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-4224                    │
│                              ├─────────────────────┼──────────┤              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-2297       │ LOW      │              │                                      │                       │ cpython: CPython: Logging Bypass in Legacy .pyc File         │
│                              │                     │          │              │                                      │                       │ Handling                                                     │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-2297                    │
├──────────────────────────────┼─────────────────────┼──────────┤              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│ python3.13-minimal           │ CVE-2025-13836      │ HIGH     │              │                                      │                       │ cpython: Excessive read buffering DoS in http.client         │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-13836                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-15366      │          │              │                                      │                       │ cpython: IMAP command injection in user-controlled commands  │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-15366                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-15367      │          │              │                                      │                       │ cpython: POP3 command injection in user-controlled commands  │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-15367                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-8194       │          │              │                                      │                       │ cpython: Cpython infinite loop when parsing a tarfile        │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-8194                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-1299       │          │              │                                      │                       │ cpython: email header injection due to unquoted newlines     │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-1299                    │
│                              ├─────────────────────┼──────────┤              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-11468      │ MEDIUM   │              │                                      │                       │ cpython: Missing character filtering in Python               │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-11468                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-12084      │          │              │                                      │                       │ cpython: python: cpython: Quadratic algorithm in             │
│                              │                     │          │              │                                      │                       │ xml.dom.minidom leads to denial of service...                │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-12084                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-12781      │          │              │                                      │                       │ cpython: base64.b64decode() always accepts "+/" characters,  │
│                              │                     │          │              │                                      │                       │ despite setting altchars                                     │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-12781                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-13837      │          │              │                                      │                       │ cpython: Out-of-memory when loading Plist                    │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-13837                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-15282      │          │              │                                      │                       │ cpython: Header injection via newlines in data URL mediatype │
│                              │                     │          │              │                                      │                       │ in Python                                                    │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-15282                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-6069       │          │              │                                      │                       │ cpython: Python HTMLParser quadratic complexity              │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-6069                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-6075       │          │              │                                      │                       │ python: Quadratic complexity in os.path.expandvars() with    │
│                              │                     │          │              │                                      │                       │ user-controlled template                                     │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-6075                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-8291       │          │              │                                      │                       │ cpython: python: Python zipfile End of Central Directory     │
│                              │                     │          │              │                                      │                       │ (EOCD) Locator record offset...                              │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-8291                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-0672       │          │              │                                      │                       │ cpython: Header injection in http.cookies.Morsel in Python   │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-0672                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-0865       │          │              │                                      │                       │ cpython: wsgiref.headers.Headers allows header newline       │
│                              │                     │          │              │                                      │                       │ injection in Python                                          │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-0865                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-3644       │          │              │                                      │                       │ cpython: Incomplete control character validation in          │
│                              │                     │          │              │                                      │                       │ http.cookies                                                 │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-3644                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-4224       │          │              │                                      │                       │ cpython: Stack overflow parsing XML with deeply nested DTD   │
│                              │                     │          │              │                                      │                       │ content models                                               │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-4224                    │
│                              ├─────────────────────┼──────────┤              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-2297       │ LOW      │              │                                      │                       │ cpython: CPython: Logging Bypass in Legacy .pyc File         │
│                              │                     │          │              │                                      │                       │ Handling                                                     │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-2297                    │
├──────────────────────────────┼─────────────────────┤          │              ├──────────────────────────────────────┼───────────────────────┼──────────────────────────────────────────────────────────────┤
│ sysvinit-utils               │ TEMP-0517018-A83CE6 │          │              │ 3.14-4                               │                       │ [sysvinit: no-root option in expert installer exposes        │
│                              │                     │          │              │                                      │                       │ locally exploitable security flaw]                           │
│                              │                     │          │              │                                      │                       │ https://security-tracker.debian.org/tracker/TEMP-0517018-A8- │
│                              │                     │          │              │                                      │                       │ 3CE6                                                         │
├──────────────────────────────┼─────────────────────┤          │              ├──────────────────────────────────────┼───────────────────────┼──────────────────────────────────────────────────────────────┤
│ tar                          │ CVE-2005-2541       │          │              │ 1.35+dfsg-3.1                        │                       │ tar: does not properly warn the user when extracting setuid  │
│                              │                     │          │              │                                      │                       │ or setgid...                                                 │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2005-2541                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ TEMP-0290435-0B57B5 │          │              │                                      │                       │ [tar's rmt command may have undesired side effects]          │
│                              │                     │          │              │                                      │                       │ https://security-tracker.debian.org/tracker/TEMP-0290435-0B- │
│                              │                     │          │              │                                      │                       │ 57B5                                                         │
├──────────────────────────────┼─────────────────────┤          │              ├──────────────────────────────────────┼───────────────────────┼──────────────────────────────────────────────────────────────┤
│ tcl8.6                       │ CVE-2021-35331      │          │              │ 8.6.16+dfsg-1                        │                       │ In Tcl 8.6.11, a format string vulnerability in nmakehlp.c   │
│                              │                     │          │              │                                      │                       │ might allow ......                                           │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2021-35331                   │
├──────────────────────────────┤                     │          │              │                                      ├───────────────────────┤                                                              │
│ tcl8.6-dev                   │                     │          │              │                                      │                       │                                                              │
│                              │                     │          │              │                                      │                       │                                                              │
│                              │                     │          │              │                                      │                       │                                                              │
├──────────────────────────────┼─────────────────────┤          │              ├──────────────────────────────────────┼───────────────────────┼──────────────────────────────────────────────────────────────┤
│ unzip                        │ CVE-2021-4217       │          │              │ 6.0-29                               │                       │ unzip: Null pointer dereference in Unicode strings code      │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2021-4217                    │
├──────────────────────────────┼─────────────────────┤          │              ├──────────────────────────────────────┼───────────────────────┼──────────────────────────────────────────────────────────────┤
│ util-linux                   │ CVE-2022-0563       │          │              │ 2.41-5                               │                       │ util-linux: partial disclosure of arbitrary files in chfn    │
│                              │                     │          │              │                                      │                       │ and chsh when compiled...                                    │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2022-0563                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-14104      │          │              │                                      │                       │ util-linux: util-linux: Heap buffer overread in setpwnam()   │
│                              │                     │          │              │                                      │                       │ when processing 256-byte usernames                           │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-14104                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-3184       │          │              │                                      │                       │ util-linux: util-linux: Access control bypass due to         │
│                              │                     │          │              │                                      │                       │ improper hostname canonicalization                           │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-3184                    │
├──────────────────────────────┼─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│ uuid-dev                     │ CVE-2022-0563       │          │              │                                      │                       │ util-linux: partial disclosure of arbitrary files in chfn    │
│                              │                     │          │              │                                      │                       │ and chsh when compiled...                                    │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2022-0563                    │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2025-14104      │          │              │                                      │                       │ util-linux: util-linux: Heap buffer overread in setpwnam()   │
│                              │                     │          │              │                                      │                       │ when processing 256-byte usernames                           │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2025-14104                   │
│                              ├─────────────────────┤          │              │                                      ├───────────────────────┼──────────────────────────────────────────────────────────────┤
│                              │ CVE-2026-3184       │          │              │                                      │                       │ util-linux: util-linux: Access control bypass due to         │
│                              │                     │          │              │                                      │                       │ improper hostname canonicalization                           │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-3184                    │
├──────────────────────────────┼─────────────────────┼──────────┼──────────────┼──────────────────────────────────────┼───────────────────────┼──────────────────────────────────────────────────────────────┤
│ wget                         │ CVE-2021-31879      │ MEDIUM   │ fix_deferred │ 1.25.0-2                             │                       │ wget: authorization header disclosure on redirect            │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2021-31879                   │
├──────────────────────────────┼─────────────────────┤          ├──────────────┼──────────────────────────────────────┼───────────────────────┼──────────────────────────────────────────────────────────────┤
│ zlib1g                       │ CVE-2026-27171      │          │ affected     │ 1:1.3.dfsg+really1.3.1-1+b1          │                       │ zlib: zlib: Denial of Service via infinite loop in CRC32     │
│                              │                     │          │              │                                      │                       │ combine functions...                                         │
│                              │                     │          │              │                                      │                       │ https://avd.aquasec.com/nvd/cve-2026-27171                   │
├──────────────────────────────┤                     │          │              │                                      ├───────────────────────┤                                                              │
│ zlib1g-dev                   │                     │          │              │                                      │                       │                                                              │
│                              │                     │          │              │                                      │                       │                                                              │
│                              │                     │          │              │                                      │                       │                                                              │
└──────────────────────────────┴─────────────────────┴──────────┴──────────────┴──────────────────────────────────────┴───────────────────────┴──────────────────────────────────────────────────────────────┘
2026-03-20T00:34:11+01:00	INFO	Table result includes only package filenames. Use '--format json' option to get the full path to the package file.

Python (python-pkg)

Total: 1 (UNKNOWN: 0, LOW: 1, MEDIUM: 0, HIGH: 0, CRITICAL: 0)

┌────────────────┬───────────────┬──────────┬────────┬───────────────────┬───────────────┬──────────────────────────────────────────────────────────┐
│    Library     │ Vulnerability │ Severity │ Status │ Installed Version │ Fixed Version │                          Title                           │
├────────────────┼───────────────┼──────────┼────────┼───────────────────┼───────────────┼──────────────────────────────────────────────────────────┤
│ pip (METADATA) │ CVE-2026-1703 │ LOW      │ fixed  │ 25.3              │ 26.0          │ pip: pip: Information disclosure via path traversal when │
│                │               │          │        │                   │               │ installing crafted wheel archives...                     │
│                │               │          │        │                   │               │ https://avd.aquasec.com/nvd/cve-2026-1703                │
└────────────────┴───────────────┴──────────┴────────┴───────────────────┴──────
</details>



## 🌞 Utilisez l'outil Docker Bench for Security

:d

y a beaucoup de couleurs partou

fo de tout pour faire un mondem, j suis pas trop raciste meme si j prefere les verts
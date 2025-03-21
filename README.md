<pre>
# chmurowesprawozdanielab3

Zapis z konsoli:

Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

Try the new cross-platform PowerShell https://aka.ms/pscore6

PS C:\WINDOWS\system32> wsl
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

Welcome to Ubuntu 22.04.5 LTS (GNU/Linux 5.15.167.4-microsoft-standard-WSL2 x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Fri Mar 21 12:32:27 CET 2025

  System load:  0.32                Processes:             39
  Usage of /:   0.2% of 1006.85GB   Users logged in:       2
  Memory usage: 8%                  IPv4 address for eth0: 172.23.0.120
  Swap usage:   0%

 * Strictly confined Kubernetes makes edge and IoT secure. Learn how MicroK8s
   just raised the bar for easy, resilient and secure K8s cluster deployment.

   https://ubuntu.com/engage/secure-kubernetes-at-the-edge

This message is shown once a day. To disable it please create the
/home/piotr/.hushlogin file.
piotr@DESKTOP-VQG076S:/mnt/c/WINDOWS/system32$ exit
logout
PS C:\WINDOWS\system32> wsl -d "Ubuntu-22.04"
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

piotr@DESKTOP-VQG076S:/mnt/c/WINDOWS/system32$ mkdir -p ~/registry/certs
/registry/certs
piotr@DESKTOP-VQG076S:/mnt/c/WINDOWS/system32$ cd ~/registry/certs
piotr@DESKTOP-VQG076S:~/registry/certs$ openssl req -newkey rsa:4096 -nodes -sha256 -keyout domain.key -x509 -days 365 -out domain.crt
.+......+...+........+...+....+.....+.+...........+.+........+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*.+...+...+..+...+....+......+......+.....+....+.....+.......+..+.......+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*.........+..........+...+......+...+......+......+........+.+..+....+.....+...+......+....+........+.......+..+.+............+..+...+......+.........+.+..+............+............+.+...+............+.......................+....+...+.........+.....+......+....+...+.....+......+.+.........+......+.....+.+..+.+.....+.........+.............+..............+...+.......+...+..+......+....+......+........+......+...................+..+......................+...+...+..+...+......+....+..+.............+......+...+...+...............+..+......+............+.................................+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
.+........+......+...............+.+.....+....+...+.....+......+.......+........+.+...+.....+.+.....+.........+..........+..+.+..+...+....+...........+.........+.+..+...............+.......+...+.....+.+..+...+.+........+............+.+.....+....+...+..+.............+..+.+........+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*.......................+..........+.....+.......+..+.+...+............+.....+.........+................+..+......+.+...+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*..+...+...+................+.....+...............+....+..+........................+.........+....+..+..........+.....+...+......+.......+..+...+.........+.+...+...............+.....+......+...+............+.............+...+.........+.....+..........+..............+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:PL
State or Province Name (full name) [Some-State]:Lubelskie
Locality Name (eg, city) []:Lublin
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Pollub
Organizational Unit Name (eg, section) []:WEII
Common Name (e.g. server FQDN or YOUR name) []:testregistry.local
Email Address []:testowy@mail.pl
piotr@DESKTOP-VQG076S:~/registry/certs$ docker run -d --restart=always --name registry -v ~/registry/certs:/certs -e REGISTRY_HTTP_ADDR=0.0.0.0:5000 -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key -p 5000:5000 registry:2
docker: Error response from daemon: Conflict. The container name "/registry" is already in use by container "bd18c8f8c51a8220dd4b19f1b0d823bbd2e334955a3748afdec7f3dcc7ad2e10". You have to remove (or rename) that container to be able to reuse that name.
See 'docker run --help'.
piotr@DESKTOP-VQG076S:~/registry/certs$ docker ps -a
CONTAINER ID   IMAGE              COMMAND                  CREATED         STATUS                      PORTS                    NAMES
bd18c8f8c51a   registry:2         "/entrypoint.sh /etc…"   6 days ago      Up 7 minutes                0.0.0.0:5000->5000/tcp   registry
12298e22210a   ubuntu:22.04       "/bin/bash"              6 days ago      Exited (137) 6 days ago                              amogus_kontener
5d828c5b7728   hello-world        "/hello"                 2 weeks ago     Exited (0) 2 weeks ago                               eager_golick
56b4133433fc   gvenzl/oracle-xe   "container-entrypoin…"   14 months ago   Exited (255) 2 months ago   0.0.0.0:1521->1521/tcp   priceless_saha
piotr@DESKTOP-VQG076S:~/registry/certs$ curl -k https://localhost:5000/v2/_catalog
curl: (35) error:0A00010B:SSL routines::wrong version number
piotr@DESKTOP-VQG076S:~/registry/certs$ exit
logout
PS C:\WINDOWS\system32> wsl -d "Ubuntu-22.04"
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

piotr@DESKTOP-VQG076S:/mnt/c/WINDOWS/system32$ docker run -d \
>   --restart=always \
>   --name registry \
>   -v ~/registry/certs:/certs \
>   -e REGISTRY_HTTP_ADDR=0.0.0.0:5000 \
>   -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \
>   -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key \
>   -p 5000:5000 \
>   registry:2
ddf7b68365554c47ccbfaae6d3d13c1ccabe5a8d3b52064fad2e980d41e6f647
piotr@DESKTOP-VQG076S:/mnt/c/WINDOWS/system32$ curl -k https://localhost:5000/v2/_catalog
cker pull ubuntu:latest{"repositories":[]}
piotr@DESKTOP-VQG076S:/mnt/c/WINDOWS/system32$ docker pull ubuntu:latest
latest: Pulling from library/ubuntu
5a7813e071bf: Pull complete
Digest: sha256:72297848456d5d37d1262630108ab308d3e9ec7ed1c3286a32fe09856619a782
Status: Downloaded newer image for ubuntu:latest
docker.io/library/ubuntu:latest
piotr@DESKTOP-VQG076S:/mnt/c/WINDOWS/system32$ sudo mkdir -p /etc/docker/certs.d/testregistry.local:5000
 cp domain.crt /etc/docker/certs.d/registry.domain.com:5000/ca.crt[sudo] password for piotr:
Sorry, try again.
[sudo] password for piotr:
piotr@DESKTOP-VQG076S:/mnt/c/WINDOWS/system32$ sudo mkdir -p /etc/docker/certs.d/testregistry.local:5000
piotr@DESKTOP-VQG076S:/mnt/c/WINDOWS/system32$ sudo cp domain.crt /etc/docker/certs.d/testregistry.local:5000/ca.crt
cp: cannot stat 'domain.crt': No such file or directory
piotr@DESKTOP-VQG076S:/mnt/c/WINDOWS/system32$ cd ~/registry/certs\
>
piotr@DESKTOP-VQG076S:~/registry/certs$ sudo mkdir -p /etc/docker/certs.d/testregistry.local:5000
piotr@DESKTOP-VQG076S:~/registry/certs$ sudo cp domain.crt /etc/docker/certs.d/testregistry.local:5000/ca.crt
piotr@DESKTOP-VQG076S:~/registry/certs$ sudo nano /etc/docker/daemon.json
piotr@DESKTOP-VQG076S:~/registry/certs$ sudo nano /etc/docker/daemon.json
piotr@DESKTOP-VQG076S:~/registry/certs$ ls -la /etc/docker/certs.d/testregistry.local:5000/
total 12
drwxr-xr-x 2 root root 4096 Mar 21 12:50 .
drwxr-xr-x 4 root root 4096 Mar 21 12:48 ..
-rw-r--r-- 1 root root 2143 Mar 21 12:50 ca.crt
piotr@DESKTOP-VQG076S:~/registry/certs$ sudo nano /etc/hosts
piotr@DESKTOP-VQG076S:~/registry/certs$ ping testregistry.local
PING testregistry.local (127.0.0.1) 56(84) bytes of data.
64 bytes from localhost (127.0.0.1): icmp_seq=1 ttl=64 time=0.016 ms
64 bytes from localhost (127.0.0.1): icmp_seq=2 ttl=64 time=0.020 ms
64 bytes from localhost (127.0.0.1): icmp_seq=3 ttl=64 time=0.019 ms
64 bytes from localhost (127.0.0.1): icmp_seq=4 ttl=64 time=0.020 ms
64 bytes from localhost (127.0.0.1): icmp_seq=5 ttl=64 time=0.020 ms
64 bytes from localhost (127.0.0.1): icmp_seq=6 ttl=64 time=0.019 ms
64 bytes from localhost (127.0.0.1): icmp_seq=7 ttl=64 time=0.019 ms
64 bytes from localhost (127.0.0.1): icmp_seq=8 ttl=64 time=0.019 ms
64 bytes from localhost (127.0.0.1): icmp_seq=9 ttl=64 time=0.020 ms
^C
--- testregistry.local ping statistics ---
9 packets transmitted, 9 received, 0% packet loss, time 8515ms
rtt min/avg/max/mdev = 0.016/0.019/0.020/0.001 ms
piotr@DESKTOP-VQG076S:~/registry/certs$ exit
logout
PS C:\WINDOWS\system32> notepad C:\Windows\System32\drivers\etc\hosts
PS C:\WINDOWS\system32> ping testregistry.local
Ping request could not find host testregistry.local. Please check the name and try again.
PS C:\WINDOWS\system32> notepad C:\Windows\System32\drivers\etc\hosts
PS C:\WINDOWS\system32> ping testregistry.local

Pinging testregistry.local [127.0.0.1] with 32 bytes of data:
Reply from 127.0.0.1: bytes=32 time<1ms TTL=128
Reply from 127.0.0.1: bytes=32 time<1ms TTL=128
Reply from 127.0.0.1: bytes=32 time<1ms TTL=128
Reply from 127.0.0.1: bytes=32 time<1ms TTL=128

Ping statistics for 127.0.0.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
PS C:\WINDOWS\system32> docker tag ubuntu:latest testregistry.local:5000/my-ubuntu
PS C:\WINDOWS\system32> docker push testregistry.local:5000/my-ubuntu
Using default tag: latest
The push refers to repository [testregistry.local:5000/my-ubuntu]
4b7c01ed0534: Pushed
latest: digest: sha256:104f82606ea66da00e6cfecbcccdb53ba4238a7057bed809f004107ad8e90c97 size: 529
PS C:\WINDOWS\system32> curl -k https://localhost:5000/v2/_catalog
Invoke-WebRequest : A parameter cannot be found that matches parameter name 'k'.
At line:1 char:6
+ curl -k https://localhost:5000/v2/_catalog
+      ~~
    + CategoryInfo          : InvalidArgument: (:) [Invoke-WebRequest], ParameterBindingException
    + FullyQualifiedErrorId : NamedParameterNotFound,Microsoft.PowerShell.Commands.InvokeWebRequestCommand

PS C:\WINDOWS\system32> wsl
piotr@DESKTOP-VQG076S:/mnt/c/WINDOWS/system32$ docker tag ubuntu:latest testregistry.local:5000/my-ubuntu
piotr@DESKTOP-VQG076S:/mnt/c/WINDOWS/system32$ docker push testregistry.local:5000/my-ubuntu
Using default tag: latest
The push refers to repository [testregistry.local:5000/my-ubuntu]
4b7c01ed0534: Layer already exists
latest: digest: sha256:104f82606ea66da00e6cfecbcccdb53ba4238a7057bed809f004107ad8e90c97 size: 529
piotr@DESKTOP-VQG076S:/mnt/c/WINDOWS/system32$ curl -k https://localhost:5000/v2/_catalog
{"repositories":["my-ubuntu"]}
piotr@DESKTOP-VQG076S:/mnt/c/WINDOWS/system32$
</pre>


----------------------------------------------------------------------------
Laboratorium 3 - lista kroków

# 1. Utworzenie folderu i certyfikatu SSL
```bash
mkdir -p ~/registry/certs
cd ~/registry/certs
openssl req -newkey rsa:4096 -nodes -sha256 -keyout domain.key -x509 -days 365 -out domain.crt
```

Wypełnione dane:
- Country: PL
- State: Lubelskie
- City: Lublin
- Organization: Pollub
- Org Unit: WEII
- Common Name: testregistry.local
- Email: testowy@mail.pl

---

#2. Uruchomienie rejestru Dockera z certyfikatem
```bash
docker run -d --restart=always --name registry \
  -v ~/registry/certs:/certs \
  -e REGISTRY_HTTP_ADDR=0.0.0.0:5000 \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \
  -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key \
  -p 5000:5000 \
  registry:2
```

---

#3. Dodanie certyfikatu do Dockera
```bash
sudo mkdir -p /etc/docker/certs.d/testregistry.local:5000
sudo cp domain.crt /etc/docker/certs.d/testregistry.local:5000/ca.crt
```

---

# 4.Edytowanie konfiguracji Dockera
```bash
sudo nano /etc/docker/daemon.json
```

Dodać:
```json
{
  "insecure-registries": ["testregistry.local:5000"]
}
```

---

# 5. Dodanie wpisu DNS na Linuxie + Windowsie
**Linux:**
```bash
sudo nano /etc/hosts
```

**Windows (PowerShell jako administrator):**
```powershell
notepad C:\Windows\System32\drivers\etc\hosts
```
Dodć linijkę:
```
127.0.0.1 testregistry.local
```

---

# 6. Sprawdzenie działania
```bash
ping testregistry.local
curl -k https://localhost:5000/v2/_catalog
```

Oczekiwany wynik:
```json
{"repositories":[]}
```

---

#7. Dodanie obrazu i wypchnięcie obrazu
```bash
docker pull ubuntu:latest
docker tag ubuntu:latest testregistry.local:5000/my-ubuntu
docker push testregistry.local:5000/my-ubuntu
```

---

#8. Ostatnie sprawdzenie
```bash
curl -k https://localhost:5000/v2/_catalog
```

Oczekiwany wynik:
```json
{"repositories":["my-ubuntu"]}
```


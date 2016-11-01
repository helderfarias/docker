# Running Docker behind a proxy

### Pulling images

* RedHat/CentOS version 6
```bash
cat <<EOF | sudo tee -a /etc/sysconfig/docker
export http_proxy="http://<HOST>:<PORT>"
export https_proxy="https://<HOST>:<PORT>"
export no_proxy=<REGISTRY_IP>
EOF
 
sudo service docker restart
```

* RedHat/CentOS version 7
```bash
cat <<EOF | sudo tee -a /etc/sysconfig/docker
http_proxy="http://<HOST>:<PORT>"
https_proxy="https://<HOST>:<PORT>"
no_proxy=<REGISTRY_IP>
EOF
 
sudo sed -i '/\[Service\]/a EnvironmentFile=/etc/sysconfig/docker' /usr/lib/systemd/system/docker.service
sudo systemctl daemon-reload
sudo service docker restart
```

* Ubuntu 14.04
```bash
cat <<EOF | sudo tee -a /etc/default/docker
export http_proxy="http://<HOST>:<PORT>"
export https_proxy="https://<HOST>:<PORT>"
export no_proxy=<REGISTRY_IP>
EOF
 
sudo restart docker
```

* Ubuntu 16
```bash
sudo mkdir /etc/systemd/system/docker.service.d

cat <<EOF | sudo tee /etc/systemd/system/docker.service.d/http-proxy.conf
http_proxy="http://<HOST>:<PORT>"
https_proxy="https://<HOST>:<PORT>"
no_proxy=<REGISTRY_IP>
EOF
 
sudo systemctl daemon-reload
sudo service docker restart
```

### Building images

```bash
docker build -t <name> \
   --build-arg http_proxy="http://<HOST>:<PORT>" \
   --build-arg https_proxy="https://<HOST>:<PORT>" \
   .
   
RUN \
  echo -e "proxy=$http_proxy\nproxy=$https_proxy" >> /etc/yum.conf
```

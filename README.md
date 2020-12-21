# README.md

How to generate rke cluster.yaml config file:

https://academy.rancher.com/courses/course-v1:RANCHER+K101+2019/courseware/af8b3309e57347f89474757bdce2298b/45070291f62c4fdc919974631a8ab1b9/?child=first

@1:36

rke confg 


https://www.ssh.com/ssh/keygen/


Used this to generate ssh-key:

ssh-keygen --f ~/.ssh/id_ed25519_rancher -t ed25519


Copy into clipboard:

pbcopy < ~/.ssh/id_ed25519_rancher


In ~/.ssh/config

Host ubuntu-s-1vcpu-2gb-lon1-01
  AddKeysToAgent yes
  HostName 178.62.5.23
  Port 22
  User root~/.ssh/id_rsa_rancher
  IdentityFile ~/.ssh/id_rsa_rancher
  IdentitiesOnly=yes

Host ubuntu-s-1vcpu-2gb-lon1-02
  AddKeysToAgent yes
  HostName  178.62.5.112
  Port 22
  User root
  IdentityFile ~/.ssh/id_rsa_rancher
  IdentitiesOnly=yes



Get the Docker for your Linux version from https://rancher.com/docs/rancher/v2.x/en/installation/requirements/installing-docker/

  curl https://releases.rancher.com/install-docker/19.03.sh | sh



https://rancher.com/docs/rke/latest/en/os/#ports



kubectl --kubeconfig kube_config_cluster.yml version

Client Version: version.Info{Major:"1", Minor:"19", GitVersion:"v1.19.4", GitCommit:"d360454c9bcd1634cf4cc52d1867af5491dc9c5f", GitTreeState:"clean", BuildDate:"2020-11-14T14:49:35Z", GoVersion:"go1.15.5", Compiler:"gc", Platform:"darwin/amd64"}
Server Version: version.Info{Major:"1", Minor:"19", GitVersion:"v1.19.4", GitCommit:"d360454c9bcd1634cf4cc52d1867af5491dc9c5f", GitTreeState:"clean", BuildDate:"2020-11-11T13:09:17Z", GoVersion:"go1.15.2", Compiler:"gc", Platform:"linux/amd64"}
➜  rancher kubectl --kubeconfig kube_config_cluster.yml get nodes


kubectl --kubeconfig kube_config_cluster.yml get nodes

NAME          STATUS   ROLES                      AGE   VERSION
178.62.5.23   Ready    controlplane,etcd,worker   41m   v1.19.4


Default snapshot every 6 hours and kept for 24 hours


Manual backup

rke etcd snapshot-save 

this will write to
/opt/rke/etcd-snapshots on etcd nodes

and get data off those etcd nodes manually or mount nfs volume at that location

also can config rke  copy to S3

What if not wanting to use AWS?

Alernative using Minio

- Drop-in replacement for S3
- Works with private and self-signed certs
- Place CA signing cert in RKE config

How to restore from a snapshot?

- Use "rke etcd snapshot-restore"
- Run from directory with configs

- Place snapshot in the directory "opt/rke/etcd-snapshots" on one node

Took, verified checksum and remove and re-deployed from backup 123 sec to rebuild from S3!

TODO: Deploy Kafka-Samara(Golang) on Kubernetes



Upgrading Kubernetes
- Upgrade your local copy of "rke"
- Use "rke config --list-version --all" to see available versions

- Set the version in the "kubernetes_version" key in cluster.yml

- Don't set it under "system_images"


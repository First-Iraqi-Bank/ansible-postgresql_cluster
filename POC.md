##### VM network (Ubuntu)
sudo vim /etc/netplan/99-netcfg-vmware.yaml
sudo netplan apply


## Ansible PG
###
automation/vars/system.yml#122 - ssh pub key
automation/inventory#67 - ssh credentials


### STEP 0 and PRE-REQ:
See step-by-step guide at [README.md#Command line](./README.md#command-line).

#### Deploy PG
ansible-playbook deploy_pgcluster.yml

#### Destroy ALL PG
ansible-playbook remove_cluster.yml -e "remove_postgres=true remove_etcd=true"

### Backup S3 (minio)
TODO:
Should be done somewhere at [vars/main.yml](./automation/vars/main.yml#518) line 518 should add Minio compatibility (probably).
But what to do with pgbackerst??


### Standby cluster
TODO:
Should be done somewhere at [vars/main.yml](./automation/vars/main.yml#401) line 401.



## Minio Install
ssh to host and execute:
```sh
sudo groupadd -r minio-user
sudo useradd -M -r -g minio-user minio-user
sudo mkdir -p /opt/minio
sudo chown minio-user:minio-user /opt/minio

wget https://dl.min.io/server/minio/release/linux-amd64/archive/minio_20240913202602.0.0_amd64.deb -O minio.deb
sudo dpkg -i minio.deb

cat <<EOF > ./minio
MINIO_ROOT_USER=minio
MINIO_ROOT_PASSWORD=password
MINIO_VOLUMES="/opt/minio"
MINIO_OPTS="--console-address :9001"
EOF
sudo cp ./minio /etc/default/minio

sudo systemctl restart minio.service
sudo systemctl status minio.service
```

### Minio is already available at:
http://10.228.86.184:9001 site a
http://10.228.86.185:9001 site b


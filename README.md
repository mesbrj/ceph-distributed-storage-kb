# ceph-distributed-storage-kb

[ceph-ansible](https://github.com/ceph/ceph-ansible) containerized deployment on virtualized Fedora Server nodes using old SATA HDDs (from Hypervisors) as OSDs.
- Ceph Pools for RDB images/block-devices


---

```bash
mkdir deploy
git clone git@github.com:ceph/ceph-ansible.git deploy/

cp ceph-ansible/*.yml deploy/
cp ceph-ansible/inventory.ini deploy/
cp ceph-ansible/group_vars/*.yml deploy/group_vars/
```

```bash
cd deploy

ansible-playbook -v -i inventory.ini fedora-pre-deployment.yml

ansible-playbook -v -i inventory.ini site-container.yml

ansible-playbook -v -i inventory.ini fedora-post-deployment.yml
```


|ceph-ansible site-container.yml playbook|
|:-:|
|![](/ceph-ansible/docs/img/ceph-ansible-deployment.png)|

|Ceph containers as Systemd services|
|:-:|
|![](/ceph-ansible/docs/img/systemd-containers.png)|

|Ceph OSDs and Pools|
|:-:|
|![](/ceph-ansible/docs/img/ceph-osd-tree-status.png)|

|Ceph Mgr modules and (containerized) services|
|:-:|
|![](/ceph-ansible/docs/img/ceph-mgr-modules-monitoring.png)|

|Ceph Cluster Dashboard|Ceph Cluster API|
|:-:|:-:|
|![](/ceph-ansible/docs/img/ceph-cluster-dashboard.png)|![](/ceph-ansible/docs/img/ceph-cluster-api-swagger.png)|

|Ceph Exporter and Node Exporter (each ceph node)|
|:-:|
|![](/ceph-ansible/docs/img/exporters-prometheus.png)|

|Ceph Cluster Prometheus|Ceph Cluster Alertmanager|Ceph Cluster Grafana|
|:-:|:-:|:-:|
|![](/ceph-ansible/docs/img/ceph-cluster-prometheus.png)|![](/ceph-ansible/docs/img/ceph-cluster-alertmanager.png)|![](/ceph-ansible/docs/img/ceph-cluster-grafana.png)|


## Cluster shutdown / restart procedure:

- Set OSD flags: `noout`, `nobackfill`, `norecover`
    - If only `noout` is set during the process, it will work as well
- Shutdown all Ceph nodes in any order (one by one)
- Start all Ceph nodes in any order
- Wait for cluster to be healthy with all PGs in active+clean state
- Unset OSD flag(s)


|Degraded OSDs|Recovering|Pgs active+clean|
|:-:|:-:|:-:|
|![](/ceph-ansible/docs/img/restarting_1.png)|![](/ceph-ansible/docs/img/restarting_2.png)|![](/ceph-ansible/docs/img/restarting_3.png)|

## RDB images / block-devices usage examples:

### Linux KVM (QEMU/libvirt) / [Windows Hyper-V](https://docs.ceph.com/en/reef/rbd/rbd-windows/)

### Rook Storage Operator for Kubernetes

## Lab improvements

### Ceph RGW (Rados Gateway) object storage (S3 compatible)


*References and general information*

- [ceph-ansible](https://docs.ceph.com/projects/ceph-ansible/en/latest/) - [containerized deployment](https://docs.ceph.com/projects/ceph-ansible/en/latest/installation/containerized.html)
- [cephadm](https://docs.ceph.com/en/latest/cephadm/) and [cephadm-ansible](https://github.com/ceph/cephadm-ansible)
- [Rook - Storage Operators for Kubernetes](https://rook.io/)
- [How (not) to shut down a Ceph cluster](https://www.croit.io/blog/how-not-to-shut-down-a-ceph-cluster) / [Shutting down and restarting the cluster](https://documentation.suse.com/ses/7.1/html/ses-all/admin-caasp-cluster.html) / [A completely shutdown and restart procedure](https://www.ibm.com/docs/en/storage-ceph/8.1.0?topic=cluster-powering-down-rebooting-that-uses-systemctl-commands)

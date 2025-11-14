# ceph-distributed-storage-kb

<img src="/ceph-ansible/docs/img/Ceph-Lab.png" style="float: left; margin-left: 20px; margin-top: 100px; width: 400px;" alt="Ceph Lab Diagram" />

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
python(3) -m venv deploy/venv
source deploy/venv/bin/activate
```

```bash
cd deploy

pip install -r requirements.txt
ansible-galaxy install -r requirements.yml

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

## Linux KVM (QEMU/libvirt)

|libvirt pool (virsh / virt-manager) for RBD| FreeBSD guest installed on Ceph RBD disk|
|:-:|:-:|
|![](/ceph-ansible/docs/img/libvirt_rbd_storage_pool_.png)|![](/ceph-ansible/docs/img/FreeBSD-Guest-RBD-Disk.png)|

## Banchmarking Ceph RBD

Throughput, IOPS and latency measurements with fine-tuning tests using [FIO (Flexible I/O tester)](https://fio.readthedocs.io/en/latest/).

## Rook Storage Operator for Kubernetes

Using ceph-ansible deployed Ceph cluster as backend (external) storage for Rook Operator in Kubernetes.

## Ceph RGW (Rados Gateway) object storage (S3 compatible)

## Infrastructure improvements

![](/ceph-ansible/docs/img/Ceph-Lab_update.png)
*requirements:*
- One Single-Board-Computer with at least 2 Ethernet NICs
    - Rpi3s with USB Ethernet adapters are a nightmare (ate least in my experience)
- Two simple flat switches
- One additional NIC in each Hypervisor

*Since it's a Lab environment no Out-Of-Band and Management networks are needed.*

## Ceph Multi-site

- Multi-site block mirroring
- Multi-site object storage (RGW)


### *References and general information*

- [ceph-ansible](https://docs.ceph.com/projects/ceph-ansible/en/latest/) - [containerized deployment](https://docs.ceph.com/projects/ceph-ansible/en/latest/installation/containerized.html)
- [cephadm](https://docs.ceph.com/en/latest/cephadm/) and [cephadm-ansible](https://github.com/ceph/cephadm-ansible)
- [Rook - Storage Operators for Kubernetes](https://rook.io/)
- [How (not) to shut down a Ceph cluster (Croit)](https://www.croit.io/blog/how-not-to-shut-down-a-ceph-cluster) \ [Shutting down and restarting the cluster (Suse)](https://documentation.suse.com/ses/7.1/html/ses-all/admin-caasp-cluster.html) \ [A completely shutdown and restart procedure (IBM)](https://www.ibm.com/docs/en/storage-ceph/8.1.0?topic=cluster-powering-down-rebooting-that-uses-systemctl-commands)
- [Benchmark Persistent Disk performance on a Linux VM (Google)](https://docs.cloud.google.com/compute/docs/disks/benchmarking-pd-performance-linux)
- [QEMU](https://docs.ceph.com/en/latest/rbd/qemu-rbd/), [libvirt](https://docs.ceph.com/en/latest/rbd/libvirt/) and RBD \ [Windows Hyper-V](https://docs.ceph.com/en/reef/rbd/rbd-windows/) and RBD
- [libvirt pool (virsh / virt-manager)](https://daegonk.medium.com/ceph-rbd-for-virtual-machine-images-70305e1d1d3b) for RBD


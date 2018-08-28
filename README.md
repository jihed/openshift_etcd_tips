# Manage openshift and kubernetes object in etcd
use etcdctl to manage (list, delete ...) openshift and kubernetes object.
ETCD version 3:

### Backup ETCD data and configuration:

Check [openshift official docs ](https://docs.openshift.com/container-platform/3.10/day_two_guide/host_level_tasks.html#backing-up-etcd-configuration-files-2)

### Get the current etcd configuration from the master config.
```
easy_install shyaml

etcd_endpoint="$(cat /etc/origin/master/master-config.yaml | shyaml get-value etcdClientInfo.urls.0)"
cert_file='/etc/origin/master/master.etcd-client.crt'
key_file='/etc/origin/master/master.etcd-client.key'
ca_file="/etc/origin/master/$(cat /etc/origin/master/master-config.yaml | shyaml get-value etcdClientInfo.ca)"
```

### List all the keys inside etcd 

#### openshift.io for openshift objects
```
ETCDCTL_API=3 etcdctl --endpoints=${etcd_endpoint} --cert ${cert_file} --key ${key_file} --cacert ${ca_file} get /openshift.io --prefix  --keys-only
```
#### kubernetes.io for kubernetes
```
ETCDCTL_API=3 etcdctl --endpoints=${etcd_endpoint} --cert ${cert_file} --key ${key_file} --cacert ${ca_file} get /kubernetes.io/namespaces --prefix  --keys-only
```
### Delete the keys
```
ETCDCTL_API=3 etcdctl --endpoints=${etcd_endpoint} --cert ${cert_file} --key ${key_file} --cacert ${ca_file} del /kubernetes.io/namespaces/test01
```
## More options: 

```
ETCDCTL_API=3 etcdctl --endpoints=${etcd_endpoint} --cert ${cert_file} --key ${key_file} --cacert ${ca_file} etcd -h
```

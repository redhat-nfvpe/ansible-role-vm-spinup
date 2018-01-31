# ansible-role-vm-spinup

A set of roles for spinning up simple libvirt virtual machines for labs. This is primarily used in labs for spinning up machines for running Kubernetes, so some of the VMs used you'll note are tailored to this situation.

## Requirements

The host you run this against should be a CentOS (or potentially Fedora may work) host capable of running virtual machines.

## Example Playbook

An example inclusion of this role may look like:

    - hosts: servers
      roles:
         - { role: redhat-nfvpe.vm-spinup }

## Role Variables

By default, this role will spin up virtual machines, should you wish to remove those, use the `teardown` variable set to true.

```
# default to spin up, but, if teardown is true, tear down the VMs.
teardown: false
```

### Virtual machine options

By default we spin up 4 machines. You may

```
ssh_proxy_enabled: false
ssh_proxy_user: root
system_default_ram_mb: 2048
system_default_cpus: 4
virtual_machines:
  - name: kube-master
    node_type: master
  - name: kube-node-1
    node_type: nodes
  - name: kube-node-2
    node_type: nodes
  - name: kube-node-3
    node_type: nodes
```

### Disk Options

```
# Disk options -----------------------------------------

# Spare disk sizes (for virtual machines)
spare_disk_attach: true
spare_disk_size_megs: 10240
spare_disk_location: "/var/lib/libvirt/images"
spare_disk_dev: vdb
```

### Network options

`network_type` indicates network environment for the virtual machines. `"default"` has one bridge and others are different for each purpose.

Note that this creates a bridge on the host as the name specified by `bridge_network_name`.

The virtual machines will be bridged to the bridge specified by `bridge_name`.

```
# Network type (default/ipv6/kokonet)
network_type: "default"

# Enables a bridge to the outside LAN
# (as opposed to using virbr0)
bridge_name: virbr0
bridge_networking: false

# Virtualization host options.
bridge_physical_nic: "enp1s0f1"
bridge_network_name: "br0"
bridge_network_cidr: 192.168.1.0/24

# Creates a secondary nic on each VM with IPv6 addressing.
ipv6_enabled: false
```

### Virtualization host options

Options to control how you setup the host.

```
images_directory: /home/images
centos_genericcloud_url: "http://cloud.centos.org/centos/7/images/CentOS-7-x86_64-GenericCloud-1611.qcow2"
image_destination_name: CentOS-7-x86_64-GenericCloud.qcow2

# Allows one to skip the steps to initially setup a virthost
# convenient when iterating quickly.
skip_virthost_depedencies: false
```

#### Less frequently utilized options

One less used option is in the case of nested virtualization and using QEMU, 
you may need to work-around setting the vCPUs, and can do so by setting, which 
will cause there to be no `--vcpus` flag set during `virt-install`.

```
nested_virt_qemu_skip_vcpus: true
```


## License

Apache v2.0

## Author Information

NFVPE @ Red Hat, Inc.

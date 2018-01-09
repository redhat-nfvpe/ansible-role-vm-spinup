# ansible-vm-spinup

A set of roles for spinning up simple libvirt virtual machines for labs. This is primarily used in labs for spinning up machines for running Kubernetes, so some of the VMs used you'll note are tailored to this situation.

## Requirements

The host you run this against should be a CentOS (or potentially Fedora may work) host capable of running virtual machines.

## Example Playbook

An example inclusion of this role may look like:

    - hosts: servers
      roles:
         - { role: dougbtv.ansible-vm-spinup }

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

Note that this creates a bridge on the host as the name specified by `bridge_network_name`.

The virtual machines will be bridged to the bridge specified by `bridge_name`.

```
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

## License

Apache

## Author Information

NFVPE @ Red Hat, Inc.

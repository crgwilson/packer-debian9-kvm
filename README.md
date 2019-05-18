# Packer Template - Debian 9 - KVM

Easily build Debian 9 qcow2 images for your KVM/QEMU server.

## Building

Assuming you have all the necessary software installed the following command can be used to have packer build a qcow2 file

```console
foo@bar:~$ packer build -var-file vars.json debian9.json
```

The resulting qcow2 file will be spat out within this project's `output` directory.

## SSH

The created VM will be bootstrapped with [the provided authorized keys file](provision/ssh/authorized_keys). For easy access to the
created machine simply drop your SSH public key in that file.

Otherwise, a default `packer` user is created which can be accessed via ssh using the password `packer`.

## Deployment

Once your qcow has been created it can be deployed using `virt-install`

```bash
#!/bin/bash
virt-install \
  --connect=qemu:///system \
  --name qcow-test \
  --memory 1024 \
  --vcpus 1 \
  --os-variant debian9 \
  --import \
  --disk path=/var/lib/libvirt/images/test-qcow.qcow2,bus=ide \
  --network bridge=br0,model=virtio \
  --graphics vnc
```

Once deployed a vnc session can be opened via

```console
foo@bar:~$ virsh console --connect=qemu:///system MY-VM-NAME
```

## Prerequisites

The following packages must be present on your system for this project to be usable

* [Packer](https://www.packer.io/)
* [QEMU](https://www.qemu.org/)

## TODOs

* Add an option to provision w/ luks

Skillup uses KVM-based virtualization and Vagrant to configure virtual hosts.
1. Run KVM virtualization on the host
2. The host must have vagrant and vagrant-libvirt installed
3. You can download the images of Centos 7 and Ubuntu 18 necessary for the skillup

vagrant box add centos/7 --provider=libvirt
vagrant box add generic/ubuntu1804 --provider=libvirt
4. The user must be a member of the libvirt, kvm, qemu groups (otherwise you will have to interactively enter the root password to connect to the KVM)
5. Install ansible and collection to interact with libvirt

ansible-galaxy collection install community.libvirt
6. Run playbook ansible_skill_up.yml

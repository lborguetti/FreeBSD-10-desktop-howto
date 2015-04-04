FreeBSD-10-desktop-howto
========================

FreeBSD 10 Desktop How-to - Tips and Tricks


## Xorg

### Keyboard layout

        Section "InputDevice"
            Identifier  "Keyboard0"
            Driver      "kbd"
            Option      "XkbRules" "xorg"
            Option      "XkbModel" "abnt2"
            Option      "XkbLayout" "br"
        EndSection

## Apps

### Install VirtualBox

  https://www.freebsd.org/doc/handbook/virtualization-host-virtualbox.html

### Install Vagrant

    cd /usr/local/sysutils/vagrant
    make config install distclean BATCH=yes

### Install Ansible

    cd /usr/ports/sysutils/ansible
    make config install distclean BATCH=yes

### Vagrant + Ansible

You need add ***ansible_python_interpreter*** in Vagrantfile to provisioning linux hosts

Example:


    config.vm.provision "ansible" do |ansible|
        ansible.playbook = "ops/playbook.yml"
        ansible.extra_vars = {
            ansible_python_interpreter: "/usr/bin/python",
        }
    end


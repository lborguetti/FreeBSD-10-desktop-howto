FreeBSD-10-desktop-howto
========================

FreeBSD 10 Desktop How-to - Tips and Tricks


## Intel Centrino Advanced N 6235 Wifi

    Update your kernel source tree

        svnup release

    Download Wifi Firmware

        cd /tmp
        curl -O https://wireless.wiki.kernel.org/_media/en/users/drivers/iwlwifi-6000g2b-ucode-18.168.6.1.tgz
        tar -xvzf iwlwifi-6000g2b-ucode-18.168.6.1.tgz
        cd iwlwifi-6000g2b-ucode-18.168.6.1
        b64encode -o iwlwifi-6000g2b-18.168.6.1.fw.uu iwlwifi-6000g2b-6.ucode iwlwifi-6000g2b-18.168.6.1.fw.uu
        cp iwlwifi-6000g2b-18.168.6.1.fw.uu /usr/src/sys/contrib/dev/iwn/

    Change Firmware

        cd /usr/src/sys/modules/iwnfw/iwn6000g2b/
        cp Makefile Makefile-old
        sed 's/iwlwifi-6000g2b.*/iwlwifi-6000g2b-18.168.6.1/' Makefile-old > Makefile

    Add support for Intel Centrino Advanced-N 6235

        cd /usr/src/sys/dev/iwn/
        cp if_iwn.c if_iwn.c-old
        curl -O https://raw.githubusercontent.com/lborguetti/FreeBSD.10.desktop.tips.tricks/master/stuffs/Intel_Centrino_Advanced-N_6235.patch
        patch < Intel_Centrino_Advanced-N_6235.patch

    Compile new kernel

        make buildkernel
        make installkernel

    refer: https://forums.freebsd.org/threads/intel-centrino-advanced-n-6235-wifi-driver.35467/

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

### Vagrant + Ansible

You need add ***ansible_python_interpreter*** in Vagrantfile to provisioning linux hosts

Example:


    config.vm.provision "ansible" do |ansible|
        ansible.playbook = "ops/playbook.yml"
        ansible.extra_vars = {
            ansible_python_interpreter: "/usr/bin/python",
        }
    end

FreeBSD-10-desktop-howto
========================

FreeBSD 10 Desktop How-to - Tips and Tricks


## Xorg

### Keyboard layout

**Copy hal policy file**

    mkdir -p /usr/local/etc/hal/fdi/policy/10osvendor
    cp /usr/local/share/hal/fdi/policy/10osvendor/10-input-policy.fdi /usr/local/etc/hal/fdi/policy/10osvendor

**Default file:**

    cat /usr/local/share/hal/fdi/policy/10osvendor/10-input-policy.fdi
    <?xml version="1.0" encoding="UTF-8"?>

    <deviceinfo version="0.2">

      <device>
        <match key="info.capabilities" contains="input">
          <match key="info.capabilities" contains="button">
            <match key="info.addons.singleton" contains_not="hald-addon-input">
              <append key="info.addons.singleton" type="strlist">hald-addon-input</append>
            </match>
          </match>
          <match key="info.capabilities" contains="input.keys">
            <match key="info.addons.singleton" contains_not="hald-addon-input">
              <append key="info.addons.singleton" type="strlist">hald-addon-input</append>
            </match>
            <match key="info.capabilities" contains_not="button">
              <append key="info.capabilities" type="strlist">button</append>
            </match>
          </match>
        </match>
      </device>

    </deviceinfo>

**Change default file, put your keyboard layout**

    cat /usr/local/etc/hal/fdi/policy/10osvendor/10-x11-input.fdi
    <?xml version="1.0" encoding="ISO-8859-1"?>
    <deviceinfo version="0.2">
      <device>

        <!-- KVM emulates a USB graphics tablet which works in absolute coordinate mode -->
        <match key="input.product" contains="QEMU USB Tablet">
           <merge key="input.x11_driver" type="string">evdev</merge>
        </match>

        <match key="info.capabilities" contains="input.tablet">
          <match key="/org/freedesktop/Hal/devices/computer:system.kernel.name"
                 string="Linux">
            <merge key="input.x11_driver" type="string">evdev</merge>
          </match>
        </match>

        <match key="info.capabilities" contains="input.keyboard">
          <!-- If we're using Linux, we use evdev by default (falling back to
               keyboard otherwise). -->
          <merge key="input.x11_driver" type="string">kbd</merge>
            <merge key="input.xkb.model" type="string">abnt2</merge>
            <merge key="input.xkb.layout" type="string">br</merge>
            <merge key="input.xkb.variant" type="string">abnt2</merge>
          <match key="/org/freedesktop/Hal/devices/computer:system.kernel.name" string="Linux">
          </match>
        </match>
      </device>
    </deviceinfo>


### Install VirtualBox

  https://www.freebsd.org/doc/handbook/virtualization-host-virtualbox.html

### Install Vagrant

    cd /usr/local/sysutils/vagrant
    make config install distclean BATCH=yes

### Install Ansible

    cd /usr/ports/sysutils/ansible/
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


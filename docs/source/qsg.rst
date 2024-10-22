.. _qg:

Quick start guide
*****************

For the users who want to try the role quickly, this guide provides
an example of how to configure ``wpa_cli``.


* Install the role ``vbotka.freebsd_wpa_cli`` ::

    shell> ansible-galaxy role install vbotka.freebsd_wpa_cli

* Install the collections ``ansible.posix`` and ``community.general`` ::

    shell> ansible-galaxy collection install ansible.posix
    shell> ansible-galaxy collection install community.general

* Create the playbook ``playbook.yml`` for single host srv.example.com (2)

.. code-block:: bash
   :emphasize-lines: 2
   :linenos:

   shell> cat playbook.yml
   - hosts: srv.example.com
     gather_facts: true
     connection: ssh
     remote_user: admin
     become: true
     become_user: root
     become_method: sudo
     roles:
       - vbotka.freebsd_wpa_cli


* To speedup the execution disable installation (2). Enable installation of the action script (3). Select template (4), enable logging (5) and synchronization of the date and time (6)

.. code-block:: bash
   :emphasize-lines: 2-6
   :linenos:

   shell> cat host_vars/srv.example.com/wpa-cli.yml
   wpacli_install: false
   wpacli_action_script: true
   wpacli_action_script_template: "1.1.0-wpa_action.sh.j2"
   wpacli_action_script_log_to_file: true
   wpacli_action_script_ntp_set: true


* Test syntax ::

    shell> ansible-playbook playbook.yml --syntax-check


* See what variables will be used ::

    shell> ansible-playbook playbook.yml -t wpacli_debug -e wpacli_debug=true


* Install packages ::

    shell> ansible-playbook playbook.yml -t wpacli_packages -e wpacli_install=true


* Dry-run, display differences and display variables ::

    shell> ansible-playbook playbook.yml -e wpacli_debug=true --check --diff


* Run the playbook ::

    shell> ansible-playbook playbook.yml

* Review the action script ``/root/bin//wpa_action.sh``. Review the log. The action script shall be notified when the interface connects to the network. The date and time shall be synchronized ::

    shell> cat /tmp/wpa_action.wlan0

    Jan 05 13:09:42 wlan0: CONNECTED
    Jan 05 13:09:42 wlan0: SSID: my-access-point
    Jan 05 13:09:44 wlan0: /etc/rc.d/dhclient forcestart wlan0: Starting dhclient.
    DHCPREQUEST on wlan0 to 255.255.255.255 port 67
    DHCPACK from 10.1.0.1
    bound to 10.1.0.20 -- renewal in 21600 seconds.
    Jan 05 13:09:44 wlan0: /etc/rc.d/routing restart: delete host 127.0.0.1: gateway lo0
    delete net default: gateway 10.1.0.1
    delete host ::1: gateway lo0
    delete net fe80::: gateway ::1
    delete net ff02::: gateway ::1
    delete net ::ffff:0.0.0.0: gateway ::1
    delete net ::0.0.0.0: gateway ::1
    ifconfig: ioctl(SIOCGDEFIFACE_IN6): Protocol family not supported
    ifconfig: ioctl(SIOCSDEFIFACE_IN6): Protocol family not supported
    add host 127.0.0.1: gateway lo0
    add net default: gateway 10.1.0.1
    Additional inet routing options: gateway=YES.
    add host ::1: gateway lo0
    add net fe80::: gateway ::1
    add net ff02::: gateway ::1
    add net ::ffff:0.0.0.0: gateway ::1
    add net ::0.0.0.0: gateway ::1
    Jan 05 13:09:44 wlan0: /etc/rc.d/ntpd stop: Stopping ntpd.
    Waiting for PIDS: 59771.
    Jan 05 13:09:50 wlan0: /usr/sbin/ntpdate -b 0.pool.ntp.org:  5 Jan 13:09:50 ntpdate[92243]: step time server 62.168.94.161 offset -0.708092 sec
    Jan 05 13:09:50 wlan0: /etc/rc.d/ntpd start: Starting ntpd.


.. warning:: The role modifies scripts in /etc/rc.d

# TTL LibreNMS Demo
Ubuntu 20.04.2 Server LTS - Download https://bit.ly/331iR4P
LibreNMS Docs - https://docs.librenms.org/

Install Ubuntu:
 - configure static IP / IPv6
 - when creating your user, do not use the username "librenms"
 - install OpenSSH server

When installation is finished, SSH to the server and run:

```
sudo su

 apt update
 apt -y install ansible
 echo "localhost" > /etc/ansible/hosts
 ansible-galaxy collection install community.mysql
 wget https://raw.githubusercontent.com/bakerds/TTL/main/build-librenms.yml
 
 nano build-librenms.yml
 # update timezone, database_password, and hostname
 # create a DNS record for your hostname (or update your hosts file)
 # Ctrl+O, Enter to save; Ctrl+X to exit
 
 ansible-playbook build-librenms.yml
```

Configuration checklist:
- Poller > Datastore: RRDTool > Step, Heartbeat
  - Update the following lines in /etc/cron.d/librenms to set your polling interval to match your RRD step
    - `*/5  *    * * *   librenms    /opt/librenms/discovery.php -h new >> /dev/null 2>&1`
    - `*/5  *    * * *   librenms    /opt/librenms/cronic /opt/librenms/poller-wrapper.py 16`

- Alerting > Email Options
- Authentication > Active Directory Settings
- Authentication > General (set auth method in /opt/librenms/config.php)
- Authorization > Device Group Settings -- enable if you need RBAC
- System > Updates > Set update Channel: release (monthly cadence vs nightly)
- Web UI > Graph Settings > Enable dynamic graphs: true

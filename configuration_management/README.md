#### Display all Ansible configurations for a host:
To display all Ansible facts (including configuration details) for a specific host, use the following command:

```
ansible -m setup app-vm1.fra1.internal

```
#### Configure a cron job for logrotate every 10 minutes between 2h - 4h:

```
*/10 2-4 * * * /usr/sbin/logrotate /etc/logrotate.conf

```

####  run this playbook [cron_logrotate.yml] using:

```
ansible-playbook cron_logrotate.yml

```

#### Deploy the ntpd package to the 3 servers with a custom configuration run this command
```
ansible-playbook -i hosts ntpd_deploy.yml

```

Deploy Nagios monitoring template on the Nagios server:
```
ansible-playbook -i hosts nagios_monitoring.yml

```




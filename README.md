# jira-server-service-operations

Playbook and a set of roles for [deployment](deployment.yml) and [update](update.yml) of the [Atlassian Jira Software Server](https://www.atlassian.com/software/jira/download-journey) in a CentOS 8 Stream (cloud init template required on your proxmox node) + Nginx + PostgreSQL environment based on [Proxmox](https://www.proxmox.com) virtualization.

## Deployment and update

The main variables (version of Jira, postgres, java, etc.) are in [defaults/main.yml](defaults/main.yml). To change the procedure from deployment to update, change the _procedure_ variable from **deployment** to **update**.

**Beware**, in the [deployment procedure](deployment.yml), the first step is to [stop and **remove(!)**](cleanup-vms.yml) existing VMs on the proxmox node and then [create new ones](create-vms.yml). The Proxmox VMs and host configuration are set here - [defaults/proxmox.yml](defaults/proxmox.yml).

[Update](update.yml) only updates [Jira app](roles/jira) and [Java](roles/java).
Previous versions of applications are stored in the _/opt_ directory.

Postgres, nginx, elastic- and zabbix-agents are installed and configured with _procedure == 'deployment'_. Application settings are contained in the description of [roles](roles/).

To launch chosen procedure, run:

```
jira-server-sop$ ansible-playbook playbook.yml
```

## Requirements

For interaction with Promox [community.general.proxmox module](https://docs.ansible.com/ansible/latest/collections/community/general/proxmox_module.html) is used.
Also you should have [cloud-init template image](https://pve.proxmox.com/wiki/Cloud-Init_Support) on your proxmox server, with [resize disk option](https://stafwag.github.io/blog/blog/2019/03/03/howto-use-centos-cloud-images-with-cloud-init) available on it. _Better use terraform_

## Restoring from a backup

To [restore](restore.yml) from a backup, set the _restore_from_backup_ variable to **yes** and set the backup date - _backup_date_ in the "_YYYY-MM-DD_" format, or use "_{{ ansible_date_time.date }}_".

Start the recovery process separately:

```
jira-server-sop$ ansible-playbook restore.yml
```

## Run Jira as a service

On the jira app server run with root privileges:

```
~# systemctl start jira
```

To stop jira, type and execute following command:

```
~# systemctl stop jira
```

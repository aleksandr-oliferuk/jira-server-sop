# jira-server-service-operations

Playbook and a set of roles for [deployment](deployment.yml) and [update](update.yml) of the [Atlassian Jira Software Server](https://www.atlassian.com/software/jira/download-journey) in a CentOS 8 Stream (cloud init template required on your proxmox node) + Nginx + PostgreSQL environment based on [Proxmox](https://www.proxmox.com) virtualization.

## HOWTO

The main variables (version of Jira, postgres, java, etc.) are in [defaults/main.yml](). To change the procedure from install to update, change the _procedure_ variable from **deployment** to **update**, than run with:

```
jira-server-sop$ ansible-playbook playbook.yml
```

**Beware**, in the [deployment procedure](deployment.yml), the first step is to [stop and **remove(!)**](cleanup-vms.yml) existing VMs on the proxmox node and then [create new ones](create-vms.yml). The Proxmox VMs and host configuration are set here - [defaults/proxmox.yml]().

[Update](update.yml) only updates [Jira app](roles/jira/tasks/main.yml) and [Java](roles/java/tasks/main.yml).
Previous versions of applications are stored in the _/opt_ directory.

Postgres, nginx, elastic- and zabbix-agents are installed and configured with _procedure == 'deployment'_. Application settings are contained in the description of [roles](roles/).

## Restoring from a backup
To [restore](restore.yml) from a backup, set the _restore_from_backup_ variable to **yes** and set the backup date - _backup_date_ in the "YYYY-MM-DD" format, or use "{{ ansible_date_time.date }}".

Start the recovery process separately:

```
jira-server-sop$ ansible-playbook restore.yml
```

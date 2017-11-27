# Ansible Role: Fedora 4

An Ansible role that installs Fedora 4 in a Tomcat 8 servlet container on:

* Centos/RHEL 7.x
* Ubuntu Xenial

## Role Variables

Available variables are listed below, along with default values:

Version of Fedora to install
```
fcrepo_version: 4.7.2
```

User with permissions to install:
```
fcrepo_user:
```

Where to put the Fedora war file
```
fcrepo_war_path:
```

A home directory for Fedora
```
fcrepo_home_dir: /opt/fcrepo
```

The activemq configuration file template name
```
fcrepo_activemq_template: activemq.xml.j2
```

Where the configurations are stored
```
fcrepo_config_dir: "{{ fcrepo_home_dir }}/configs"
```

Path to put Fedora data directory
```
fcrepo_data_dir: "{{ fcrepo_home_dir }}/fcrepo4-data"
```

## Dependencies

* Islandora-Devops.tomcat8
     * [Github](https://github.com/Islandora-Devops/ansible-role-tomcat8)
     * [Galaxy](https://galaxy.ansible.com/Islandora-Devops/tomcat8/)
  
## Example Playbook

    - hosts: webservers
      roles:
        - { role: Islandora-Devops.fcrepo }

## License

MIT
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
fcrepo_user: {{ tomcat8_server_user }}
```

Path to put Fedora data directory (see the notes section below)
```
fcrepo_data_dir: /var/lib/tomcat8/fcrepo4-data
```

A home directory for Fedora
```
fcrepo_home_dir: /opt/fcrepo
```

Where to put the Fedora war file
```
fcrepo_war_path: "{{ tomcat8_home }}/webapps/fcrepo.war"
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

Path to put the Fedora data binaries directory
```
fcrepo_binary_directory: "{{ fcrepo_data_dir}}/binaries"
```

Which Fedora object persistence configuration to use
```
fcrepo_persistence: file-simple
```

If 'file-simple persistence' is used (default), where to keep the modeshape repository file
```
fcrepo_object_directory: "{{ fcrepo_data_dir}}/objects"
```

If either 'jdbc-mysql' or 'jdbc-postgres' are used for object persistence, the database settings
```
fcrepo_db_name: fcrepo
fcrepo_db_user: fcrepo
fcrepo_db_password: fcrepo
fcrepo_db_host: "127.0.0.1"
fcrepo_db_port: "3306"
```

Islandora uses the HeaderProvider to pass the users roles into Fedora. To use this you will need to set the below variable.

Header name to acquire roles from
```
fcrepo_auth_header_name:
```

Islandora takes advantage of fcrepo's external content feature.  To enable redirects / proxying, you need to configure:

Where the config file gets stored:
```
fcrepo_allowed_external_content_file: "{{ fcrepo_config_dir }}/allowed-external-content.txt"
```

What paths/urls to expose:
```
fcrepo_allowed_external_content:
  - http://localhost:8000/
```


## Dependencies

* islandora.tomcat8
  
## Example Playbook

    - hosts: webservers
      roles:
        - { role: islandora.fcrepo }

## Notes

This role only uses the fcrepo_data_dir to create a directory. To tell Fedora
to use this directory you either need to incorporate the value into the
your repository.json after installation or add it to the Tomcat
Java Opts. For example, if using
[ansible-role-tomcat8](https://github.com/Islandora-Devops/ansible-role-tomcat8)
adding the following to your inventory:
```
fcrepo_data_dir: "/data/fcrepo-data"

tomcat8_java_opts:
  - -Dfcrepo.binary.directory={{ fcrepo_data_dir }}
```

Similarly, the MySQL and Postgresql object persistence configurations also use Tomcat Java Opts. Again, using ansible-tole-tomcat8:
```
fcrepo_persistence: jdbc-mysql

fcrepo_db_name: fcrepo
fcrepo_db_user: fcrepo
fcrepo_db_host: "127.0.0.1"
fcrepo_db_port: "3306"

tomcat8_java_opts:
- -Dfcrepo.mysql.username={{ fcrepo_db_name }}
- -Dfcrepo.mysql.password={{ fcrepo_db_password }}
- -Dfcrepo.mysql.host={{ fcrepo_db_host }}
- -Dfcrepo.mysql.port={{ fcrepo_db_port }}
```

## License

MIT

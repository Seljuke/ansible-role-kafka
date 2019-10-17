Kafka installation
=========

This role:
  - Installs Kafka
  - Configures it as a single node or a cluster

Requirements
------------
 - At least 1 zookeeper should be already installed on localhost or you can use this role with `seljuke.zookeeper` role to fresh install.

 - Minimal Version of the ansible for installation: 2.7
 Supported OS:
   - CentOS
       7
   - Ubuntu
    - 16.04
    - 18.04
  - Debian
    - 9

Role Variables
--------------

- `version` - version of the package
  default: `2.3.0`

- `scala_version` - scala_version of the package
  default: `2.12`

- `mirror_address` - address of kafka tar packages
  default: `http://archive.apache.org/dist/kafka`

- `download_dir` - path of download dir
  default: `/tmp/download`

- `zookeeper_hosts` - hostnames or IPs of zookeepers without ports
  default: `[]`

- `install_dir` - installation path of kafka
  default: `/opt/kafka_{{ scala_version }}-{{ version }}`

- `initial_heap_size` - initial kafka jvm heap size
  default: `200m`

- `max_heap_size` - maximum kafka jvm heap size
  default: `350m`

- `zk_client_port` - zookeeper client port
  default: `2181`

- `log_dirs` - path of kafka logs(accutual data)
  default: `/tmp/kafka-logs-1, /tmp/kafka-logs-2`

- `kafka_port` - kafka listening port
  default: `9092`

Other variable can be found at defaults

Dependencies
------------

https://github.com/geerlingguy/ansible-role-java

Example Inventory
----------------
```ini
[zookeepers]
zk1 ansible_host=192.168.1.100 zookeepers_id=1
zk2 ansible_host=192.168.1.101 zookeepers_id=2
zk3 ansible_host=192.168.1.102 zookeepers_id=3

[kafka_brokers]
kafka1 ansible_host=192.168.1.100 broker_id=1
kafka2 ansible_host=192.168.1.101 broker_id=2
kafka3 ansible_host=192.168.1.102 broker_id=3
 ```

Example Playbook
----------------

```yaml
- name: Install java
  hosts: all
  roles:
    - role: geerlingguy.java
      when: "ansible_os_family == 'Debian'"
      java_packages:
        - openjdk-8-jdk

- name: Install zookeeper
  hosts: zookeepers
  roles:
    - role: seljuke.zookeeper
      zk_inventory_group: zookeepers

- name: Install kafka
  hosts: zookeepers
  roles:
    - role: seljuke.zookeeper
```

License
-------
Apache

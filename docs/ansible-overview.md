# ansible

[My ansible boilerplate](https://github.com/CrowdSalat/boilerplate/tree/main/ansible/test-playbook)

## concepts

- Ansible runs agentless. The remote machine just needs ssh installed (and for many modules python and pip).
- the machine which got the executables installed is called **control node**
- the managed machines are called **managed nodes** or informally **hosts**
- a list of managed nodes is called **inventory** or informally **hostfile**
- **modules** are the units of code Ansible executes 
- a **task** executes a module with very specific arguments
- a **playbook** is a executable list of ansible task

*playbook and play will often get used synonymously. A playbook is the yaml file which may container one or more plays. A play is seperated by --- in the yaml file)*

## up and running

### install

- control node: intall ansible
- managed node
  - open ssh port
  - python installed (for most modules)
  - pip installed (for most modules)

### create inventory

[Official guide](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html)

You can add managed nodes in the file **/etc/ansible/hosts** either via the ini format or via the yaml format. Or set path via the`-i` flag. Hostfiles need to have the following rights: `sudo chmod 744 /etc/ansible/hosts` and might otherwise clain that the file cannot be parsed ("Unable to parse /etc/ansible/hosts as an inventory source").

A simple host file in ini format:

```ini
192.168.0.2 ansible_user=pi
```

A simple host file in yaml format:

```yaml
all:
  hosts:
    name_of_node:
      ansible_host: 192.168.0.2
      ansible_user: pi
```

Add your public ssh key to the managed nodes ~/.ssh/known_hosts files. Use the `ssh-copy-id` command or copy it manually. Use `ansible all -m ping` to test if all nodes in your inventory are reachable. 


## ad-hoc commands

[official guide](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html#intro-adhoc)

The syntax of a ad-hoc command is as following:

```shell
# ansible [pattern] -m [module] -a "[module options]"

# use of ping module
ansible all -m ping

# a exmaple which is run on all hosts 
ansible all -a "/bin/echo hello"
```

If you do not specify otherwise the ad-hoc will be run by the command module.

## playbooks

[official guide](https://docs.ansible.com/ansible/latest/user_guide/playbooks.html#working-with-playbooks)

- playbooks are defined in yaml format
- To run a playbook: `ansible-playbook playbook_file.yml`
- prerequisite for most modules like docker_image or docker_container: **the host has python and pip installed**

To run a playbook on one host you can:

1. define the host explicitly in the playbook instead of using the all target
2. define the host field as a external variable (`{{target}}`) and set it via command line (`ansible-playbook playbook.yml --extra-vars "target=pi"`)
3. run the playbook with a given a specific host file which contains only the one client (`ansible-playbook -i host-pi.yml playbook.yml`)

install software via playbook:

```yml
---
- hosts: name_of_node
  tasks:
    - name: Install Java
      package: name='java-1.8.0-openjdk' state=latest
```

*This example uses the [package](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/package_module.html) module with no arguments.*

start docker container via playbook:

```yml
---
- hosts: all
  become: true
  vars:
    container_name: hello-world
    container_image: hypriot/armhf-hello-world
  tasks:
  
  - name: install docker module for python
    pip:
      name: docker

  - name: pull docker image
    docker_image:
      name: "{{ container_image }}"
      source: pull

  - name: start docker containers
    docker_container:
      name: "{{ container_name }}"
      image: "{{ container_image }}"
      state: present
```

*This example uses the [pip](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/pip_module.html), [docker_image](https://docs.ansible.com/ansible/latest/collections/community/general/docker_image_module.html) and the [docker_container](https://docs.ansible.com/ansible/latest/collections/community/general/docker_container_module.html) module with a set of arguments.*

**Note: there was an error while using dokcer_image module [No module named ssl_match_hostname](https://github.com/docker/docker-py/issues/1502). The solution was to run `sudo apt-get remove python-configparser`**

## optional grouping of managed nodes

You can address the all nodes in a group via `ansible optional_group_name -m ping` and a specific node by its name `ansible name_of_node -m ping`

A host file witch uses groups (children) and a node without a group in yaml format:

```yaml
all:
  hosts:
    name_of_node_without_group:
      ansible_host: 192.168.0.2
      ansible_user: pi
  children:
    group_name1:
      hosts:
        name_of_node_g1_1:
          ansible_host: 192.168.0.3
          ansible_user: pi
        name_of_node_g1_2:
          ansible_host: 192.168.0.4
          ansible_user: pi
      group_name2:
        hosts:
          name_of_node_g2_1:
            ansible_host: 192.168.0.5
            ansible_user: pi
```

## test stuff locally

run `ansible-playbook filename`

```yaml
- name: My play
  hosts: localhost
  # to prevent ssh
  connection: local
  tasks:
    - name: Caculate Free Harddrive Memory
      assert:
        that:
          - not {{ item.mount == '/' and (item.size_available < 8589934592 ) }}
      with_items: "{{ ansible_facts['mounts'] }}"
    - name: Print all Hostvars
      debug:
        msg: "{{ hostvars }}"
```

## access dicitonary

Define dictionary: 

```yaml
example:
  key1: val1
  key2: val2
  
selected_key: key1

```

```yaml
    - name: print val1 by using literal key
      debug:
        msg: "{{ example ['key1']}"
  - name: print val1 by using variable key
      debug:
        msg: "{{ example [ selected_key ]}"
```

## test ansible in vagrant

[see](../vagrant)

## restart failed task

Seems not to be possible, but you can use `--start-at-task` parameter of ansible-playbook cli.

## running anisble in pipeline

- Container images for ansible: [2.3-2.13](https://github.com/cytopia/docker-ansible)
- To use Ansible galaxy hosted on git you might need a [workaround for authentification via token access](gitlab.md)
- set `host_key_checking` in ansible.cfg to false otheriwse you will get a "Host key verification failed" or set env variable ANSIBLE_HOST_KEY_CHECKING in the variables part of the pipeline

Container image requirements:

  - bash (execute deploy.sh script)
  - ansible
  - git + ssh-agent (to check out ansible galaxy roles from git)

## ansible output config

In ansible.cfg

```ini
[default]
... 

# more human readable log
stdout_callback = yaml
force_color = true

# print summary of task with execution times at the end of play
callback_whitelist = profile_tasks

[callback_profile_tasks]
sort_order = none
task_output_limit = 200
```

Worked with ansible 2.8

## change user

- `become` without `become_user` runs a command with a prepended sudo. 
- When you add `become_user` it runs `sudo -u <become_user> /bin/sh -c <command>`. *Even though `sudo su - <user>` works without password prompt `sudo -u <user>` might not work without password prompt.*
- If you add `become_method: su` it runs `su  <become_user> -c /bin/sh -c <command>`

To run `sudo su - <become_user> /bin/sh -c <command>` add `become_exe='sudo su -'` to ansible.cfg and add `become_method: su`.
See [here](https://www.coveros.com/ansible-privledge-escalation-using-sudo-su/) or [here](https://stackoverflow.com/questions/50512402/can-ansible-use-sudo-su-if-the-sudo-user-is-not-allowed-to-run-arbitrary-scr)

## becoming unpriviliged user

```
Failed to set permissions on the temporary files Ansible needs to create when becoming an unprivileged user (rc: 1, err: chown: changing ownership of '/var/tmp/ansible-tmp-1672820952.604988-42392-112740864206926/': Operation not permitted
    chown: changing ownership of '/var/tmp/ansible-tmp-1672820952.604988-42392-112740864206926/AnsiballZ_command.py': Operation not permitted
    }). For information on working around this, see https://docs.ansible.com/ansible/become.html#becoming-an-unprivileged-user
```
- You can put the `allow_world_readable_tmpfiles` in the ansible.cfg configuration file
- Or make sure the setfacl tool (provided by the acl package) is installed on the remote host.

[Source](https://github.com/georchestra/ansible/issues/55)

## update bashrc

Use lineinfile module.

## hostvars naming convention of variables in roles

Do not use dictionaries e.g. my.var.a, my.bla.b. If you access a top level part of the dictionary like my you likely overwrite all entries e.g. var, bla.

You can change this default behavior by setting hash_behaviour=merge in ansible.cfg

[Source](https://stackoverflow.com/questions/25129728/ansible-override-single-dictionary-key)

## roles accessing templates in playbooks

The following path are searched by the template module (paths get printed as an error when it cannot find a template):

Searched template file: template.yml.j2'

Searched in:

./../role/templates/template.yml.j2
./../role/template.yml.j2
./../role/tasks/templates/template.yml.j2
./../role/tasks/template.yml.j2
./../playbook/templates/template.yml.j2
./../playbook/template.yml.j2

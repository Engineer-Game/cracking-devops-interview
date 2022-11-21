# Automation

## **Ansible**

### Write a Python Module

Ok, let’s get going with an example. We’ll use Python. For starters, save this as a file named`timetest.py`

```
#!/usr/bin/python

import datetime
import json

date = str(datetime.datetime.now())
print json.dumps({
    "time" : date
})
```

```
ansible/hacking/test-module -m ./timetest.py
```

```
#!/usr/bin/python

# import some python modules that we'll use.  These are all
# available in Python's core

import datetime
import sys
import json
import os
import shlex

# read the argument string from the arguments file
args_file = sys.argv[1]
args_data = file(args_file).read()

# For this module, we're going to do key=value style arguments.
# Modules can choose to receive json instead by adding the string:
#   WANT_JSON
# Somewhere in the file.
# Modules can also take free-form arguments instead of key-value or json
# but this is not recommended.

arguments = shlex.split(args_data)
for arg in arguments:

    # ignore any arguments without an equals in it
    if "=" in arg:

        (key, value) = arg.split("=")

        # if setting the time, the key 'time'
        # will contain the value we want to set the time to

        if key == "time":

            # now we'll affect the change.  Many modules
            # will strive to be idempotent, generally
            # by not performing any actions if the current
            # state is the same as the desired state.
            # See 'service' or 'yum' in the main git tree
            # for an illustrative example.

            rc = os.system("date -s \"%s\"" % value)

            # always handle all possible errors
            #
            # when returning a failure, include 'failed'
            # in the return data, and explain the failure
            # in 'msg'.  Both of these conventions are
            # required however additional keys and values
            # can be added.

            if rc != 0:
                print json.dumps({
                    "failed" : True,
                    "msg"    : "failed setting the time"
                })
                sys.exit(1)

            # when things do not fail, we do not
            # have any restrictions on what kinds of
            # data are returned, but it's always a
            # good idea to include whether or not
            # a change was made, as that will allow
            # notifiers to be used in playbooks.

            date = str(datetime.datetime.now())
            print json.dumps({
                "time" : date,
                "changed" : True
            })
            sys.exit(0)

# if no parameters are sent, the module may or
# may not error out, this one will just
# return the time

date = str(datetime.datetime.now())
print json.dumps({
    "time" : date
})
```

```
ansible/hacking/test-module -m ./timetest.py -a "time=\"March 14 12:23\""
```

### Basic Playbook

```
---
- hosts: local
  tasks:
   - name: Install Nginx
     apt: pkg=nginx state=installed update_cache=true
```

### Run Playbook

```
$ ansible-playbook -s nginx.yml

PLAY [local] ******************************************************************

GATHERING FACTS ***************************************************************
ok: [127.0.0.1]

TASK: [Install Nginx] *********************************************************
ok: [127.0.0.1]

PLAY RECAP ********************************************************************
127.0.0.1                  : ok=2    changed=0    unreachable=0    failed=0
```

### Write a Basic Role

tasks/main.yml

```
---

- name: Add Nginx Repository
  apt_repository: repo='ppa:nginx/stable' state=present
  register: ppastable

- name: Install Nginx
  apt: pkg=nginx state=installed update_cache=true
  when: ppastable|success
  register: nginxinstalled
  notify:
    - Start Nginx

- name: Add H5BP Config
  when: nginxinstalled|success
  copy: src=h5bp dest=/etc/nginx owner=root group=root

- name: Disable Default Site
  when: nginxinstalled|success
  file: dest=/etc/nginx/sites-enabled/default state=absent

- name: Add SFH Site Config
  when: nginxinstalled|success
  register: sfhconfig
  template: src=serversforhackers.com.j2 dest=/etc/nginx/sites-available/{{ '{{' }} domain {{ '}}'  }}.conf owner=root group=root

- name: Enable SFH Site Config
  when: sfhconfig|success
  file: src=/etc/nginx/sites-available/{{ '{{' }} domain {{ '}}'  }}.conf dest=/etc/nginx/sites-enabled/{{ '{{' }} domain {{ '}}'  }}.conf state=link


- name: Create Web root
  when: nginxinstalled|success
  file: dest=/var/www/{{ '{{' }} domain {{ '}}'  }}/public mode=775 state=directory owner=www-data group=www-data
  notify:
    - Reload Nginx

- name: Web Root Permissions
  when: nginxinstalled|success
  file: dest=/var/www/{{ '{{' }} domain {{ '}}'  }} mode=775 state=directory owner=www-data group=www-data recurse=yes
  notify:
    - Reload Nginx
```

Write the install\_nginx.yml to invoke the role:

```
---
- hosts: all
  roles:
    - nginx
```

```
ansible-playbook install_nginx.yml
```

### Running commands

`ansible 'host' -m shell -a 'date' --sudo -U user -l webservers.*`

```
ansible all -s -m shell -a 'apt-get install nginx'
```

### Chef

Write a Chef Recipe

```
node.default['main']['doc_root'] = "/vagrant/web"

execute "apt-get update" do
 command "apt-get update"
end

apt_package "apache2" do
 action :install
end

service "apache2" do
 action [ :enable, :start ]
end

directory node['main']['doc_root'] do
 owner 'www-data'
 group 'www-data'
 mode '0644'
 action :create
end

cookbook_file "#{node['main']['doc_root']}/index.html" do
 source 'index.html'
 owner 'www-data'
 group 'www-data'
 action :create
end

template "/etc/apache2/sites-available/000-default.conf" do
 source "vhost.erb"
 variables({ :doc_root => node['main']['doc_root'] })
 action :create
 notifies :restart, resources(:service => "apache2")
end
```

### Puppet

Write a Puppet Module

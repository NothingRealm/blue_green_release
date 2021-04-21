And all his life was his trees.

# blue_green_release


# Overview
I broke the task to three role for seperation of concepts:
- blue-green
- setup
- docker_build

And following config files:
- Inventory
- ansible.cfg
- ssh.config

### Inventory
Here you should specify the groups of server you want ansible to connect to.

### ansible.cfg
This file is for ansible settings indicating which inventory to use and how the connection should be established.

### ssh.config
This file contains the ssh detail of each host

# Setup role
In order to use this role you must run the folowing command:
```
ansible-playbook -K setup.yml
```
This role does system configuration so it needs the root access, so I persumed that the ssh user has the sudo access and used `become` for this role.


# Docker_build role
In order to use this role you must run the folowing command:
```
ansible-playbook docker.yml
```
You can provide the fowoloing argument to the play book by editing the `/roles/docker_build/vars/main.yml`

```
debug: <bool>
image_name: <name>
image_version: <version>
dockerfile_path: </path/to/Dockerfile>
build_args: {}
```


# Blue_Green role
In order to use this role you must run the folowing command:
```
ansible-playbook blue_green.yml -K
```

This is the main role which does the blue green deployment. The main concept behind this role is creating a state machine using the ansible local facts.
It will hold the `image_name` and `image_version` for each deployment and updates the state machine if deployment was successful.
The health check consist of 10 time requesting to the `healthcheck_path` with 2 secondes of delay and if everything was allright then nginx configuration
will be done.\
The following variables are set in the `defaults` and can  be overriden in the vars folder:
```
app_name: "into_the_clouds"
image_name: cloud
image_version: v1.0.0
healthcheck_path: /healthz
nginx_upstream:  clouds
rollback: false

blue_green:
  blue:
    name: 'blue'
    port: 3001
  green:
    name: 'green'
    port: 3002
```

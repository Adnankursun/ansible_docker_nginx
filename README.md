docker-nginx
============

Ansible role to manage and run the nginx docker container.

Notes about nginx settings
--------------------------

When adding backends, if you prefer to add them using DNS ensure server can resolve the DNS name before starting nginx.
If nginx doesn't resolve the DNS name, it will not start.

Requirements
------------

This role has only been tested on Centos 8. Since this uses Ansible's
docker module, you will need to ensure that a recent version of `docker-py` need python3 to run
and `docker` are installed. 

Examples
--------

Install this module into the './roles' directory:

Install docker collection
```bash
ansible-galaxy collection install community.docker
```

Use it in a playbook as follows, assuming you already have docker setup and vault.yml will be used for secure credentials

```yaml
  - name: install nginx docker container 
    hosts: all
    become: yes
    become_user: root
    gather_facts: no
    vars:
       - vault.yml
    tasks:
      - name: "Include ansible-sample"
        include_role:
          name: "ansible_sample"
   
```

Expected to Be Configured in default/main.yml
---------------------------------------------

  * `nginx_container_name`:  name of the container 
  * `nginx_docker_tag`: docker container tag


molecule tesing:
----------------

1) Create python env for molecule testing:
  ```
  python3 -m venv venv
  . venv/bin/activate
  pip install -U pip
  pip install -r requirements.txt
  
  ```
2) To test role with molecule add role inside molecule/converge.yml:
  ```yaml
  - name: Converge
    hosts: all
    gather_facts: no
    tasks:
      - name: "Include ansible-sample"
        include_role:
          name: "ansible_sample"
  ```
   
3) Run molecule test inside the role folder:
  ```bash
     molecule test
  ```

4) verify molecule the test results.


License
-------

[MIT](LICENSE.txt)

Author Information
------------------

* Adnan Kursun


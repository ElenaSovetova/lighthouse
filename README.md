Ansible-Role Lighthouse for CentOS 7
=========

Роль устанавливает Lighthouse на CentOS 7.

Requirements
------------
нет
Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

Переменные прописаны в  файле vars/main.yml и vars/main.yml
![image](https://user-images.githubusercontent.com/91061820/224510063-08fc6df4-7fb9-4775-b022-247cff963e49.png)
![image](https://user-images.githubusercontent.com/91061820/224510092-790bffdf-6a31-4a16-88c9-a6941b55b1f4.png)



Dependencies
------------

Потребуется так же установить Git и NGINX.

Example Playbook
----------------
```
---
- name: Install nginx
  hosts: lighthouse
  handlers:
    - name: start-nginx
      become: true
      command: nginx
    - name: reload-nginx
      become: true
      command: nginx -s reload
  tasks:
    - name: NGINX Install epel-release
      become: true
      ansible.builtin.yum:
        name: epel-release
        state: present
      tags: nginx
    - name: NGINX | Install NGINX
      become: true
      ansible.builtin.yum:
        name: nginx
        state: present
      notify: start-nginx
      tags: nginx
    - name: NGINX Create general config
      become: true
      template:
        src: templates/nginx.conf.j2
        dest: /etc/nginx/nginx.conf
        mode: 0644
      notify: reload-nginx
      tags: nginx
- name: Install lighthouse
  hosts: lighthouse
  pre_tasks:
    - name: Lighthouse | install dependencies
      become: true
      ansible.builtin.yum:
        name: git
        state: present
  roles: 
    - lighthouse
```


Author Information
------------------
Elena Sovetova :)

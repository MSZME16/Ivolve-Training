# Lab 5: Ansible Playbooks for Web Server Configuration

## Objective
Write an Ansible playbook to automate the configuration of a web server. This includes:
1. Installing and configuring Apache/Nginx.
2. Deploying a sample website.
3. Ensuring proper security settings.

## Prerequisites
- Ansible installed on your local machine.
- A remote server with SSH access.
- Basic knowledge of Ansible and YAML.

## Steps

### 1. Install and Configure Apache/Nginx
Create a playbook `webserver.yml` to install and configure Apache or Nginx.

#### Apache Example

```yaml
---
- name: Configure Apache Web Server
  hosts: webservers
  become: yes

  tasks:
    - name: Ensure Apache is installed
      apt:
        name: apache2
        state: present
        update_cache: yes

    - name: Ensure Apache is started and enabled
      service:
        name: apache2
        state: started
        enabled: yes

    - name: Deploy the sample website
      copy:
        src: /path/to/your/sample/index.html
        dest: /var/www/html/index.html

    - name: Set up proper permissions on the web root
      file:
        path: /var/www/html
        owner: www-data
        group: www-data
        recurse: yes

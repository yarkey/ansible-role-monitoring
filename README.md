
<!-- LOGO -->
<a name="readme-top"></a>
<br />

<div align="center">
  <a style="text-align: center" href="https://github.com/yarkey">
    <img align="center" src="https://github.com/yarkey/gitignore/blob/main/img/yarkey-logo-black.png?raw=true" alt="Logo" width="200" height="200">
  </a>
</div>

<div>

<h3 align="center">Simple All-In-One Ansible Monitoring Role</h3>

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about">About</a>
    </li>
    <li>
      <a href="#Usage">Usage</a>
    </li>
    <li><a href="#contact">Contact</a></li>
  </ol>
</details>



<!-- ABOUT -->
<a name="about"></a>
## About
<p align="center">
Simple Ansible Role for run monitoring system with dockerised services.

Included:
- Prometheus
- Grafana
- Alertmanager
- Blackbox

Configs can provided via j2 templates but i prefer to use my own configs for every monitoring host(we can specify it in host_vars)

Tasks:
- "main" - entry point
- "cent-docker-install" and "ubnt-docker-install" - docker installation on ubuntu or centos;
- "dirs" - prepare working directories on the host system
- "configs" - prepare configuration files
- "containers" - build and run dockerised services

</p>

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- Usage -->
<a name="Usage"></a>
## Usage

<details>
  <summary>Specify Services</summary>
  <ol>
  
  The first of all we need to specify services with host(or group) variables: 
  ```yaml
  prometheus: True
  grafana: True
  nodeexporter: True
  alertmanager: True
  blackbox: True
  ```
  
  </ol>
</details>

<details>
  <summary>Use Own Configs</summary>
  <ol>
  
  To use our own configs and avoid to build it from jinja templates, we need to specify paths(use your own values):

  ```yaml
  prometheus_config: "/home/ansible/configs/prometheus.yml"
  alertmanager_config: "/home/ansible/host_vars/observer/configs/config.yml"
  alert_rules_config: "/home/ansible/host_vars/observer/configs/alert.rules"
  blackbox_config: "/home/ansible/host_vars/observer/configs/blackbox.yml"
  alertmanager_message_template: "/home/ansible/host_vars/observer/configs/template.tmpl"
  ```

  Remember - when you use role, ansible searches file in its own folder called "files" if it persists. 
  Thats why you need specify absolutely path or delete/rename default file directory in role folder.

  </ol>
</details>

<details>
  <summary>Prometheus Config From Jinja Template</summary>
  <ol>
  
  For example purpose i added ability to build configs from jinja templates.
  Templates will be using if we will not specify path to service config.
  
  By default, there are only 3 options for prometheus:
  - node_exporter for monitoring linux servers;
  - windows_exporter for monitoring windows servers;
  - blackbox for monitoring websites

  To add hosts for node_exporter monitoring we need specify "prometheus_node_exporter_group" list of hosts:
  ```yaml
  prometheus_win_exporter_group:
    - host1
    - host2
  ```
  
  To add hosts for windows_exporter monitoring we need specify "prometheus_win_exporter_group" list of hosts:
  ```yaml
  prometheus_win_exporter_group:
    - host1
    - host2
  ```
  
  To add websites for blackbox_exporter monitoring we need specify "prometheus_blackbox_group" list of websites:
  ```yaml
  prometheus_blackbox_group:
    - https://google.com
    - https://amazon.com
  ```
  
  In alert.rules template for Prometheus exists only two example alerts:
  - For Instance Availability(both node_exporter and win_exporter);
  - For SSL certifiacate end-of-life monitoring(blackbox_exporter).

  </ol>
</details>

<details>
  <summary>Alertmanager Config From Jinja Template</summary>
  <ol>

By default there is only one Alertmanager reciever in you use template: Telegram.

To build Alertmanager Config with telegram reciever:
```
alertmanager_tg_admin: <channel_id without quotes>
alertmanager_tg_token: <telegram bot token without quotes>
```
 
  </ol>
</details>

<details>
  <summary>Summary</summary>
  <ol>
  
  All we need to run this role:
  - Git clone this repository;
  - Specify config pahts;
  - Create playbook to apply this role to hosts
  For example:
  ```yaml
  ---
  - name: Run Monitoring Role
  hosts: monitoring
  become: yes
  become_method: sudo
  roles:
    - ../roles/observer
  ```
  - Run playbook:
  ```shell
  ansible-playbook playbooks/monitoring.yaml -i inventory
  ```
  </ol>
</details>
<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- CONTACT -->
<a name="contact"></a>
## Contact

Be free to contact me for all questions!

Roman Yarkey - r.yarkey@gmail.com



<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/github_username/repo_name.svg?style=for-the-badge
[contributors-url]: https://github.com/github_username/repo_name/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/github_username/repo_name.svg?style=for-the-badge
[forks-url]: https://github.com/github_username/repo_name/network/members
[stars-shield]: https://img.shields.io/github/stars/github_username/repo_name.svg?style=for-the-badge
[stars-url]: https://github.com/github_username/repo_name/stargazers
[issues-shield]: https://img.shields.io/github/issues/github_username/repo_name.svg?style=for-the-badge
[issues-url]: https://github.com/github_username/repo_name/issues
[license-shield]: https://img.shields.io/github/license/github_username/repo_name.svg?style=for-the-badge
[license-url]: https://github.com/github_username/repo_name/blob/master/LICENSE.txt
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://linkedin.com/in/linkedin_username
[product-screenshot]: images/screenshot.png
[Next.js]: https://img.shields.io/badge/next.js-000000?style=for-the-badge&logo=nextdotjs&logoColor=white
[Next-url]: https://nextjs.org/
[React.js]: https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB
[React-url]: https://reactjs.org/
[Vue.js]: https://img.shields.io/badge/Vue.js-35495E?style=for-the-badge&logo=vuedotjs&logoColor=4FC08D
[Vue-url]: https://vuejs.org/
[Angular.io]: https://img.shields.io/badge/Angular-DD0031?style=for-the-badge&logo=angular&logoColor=white
[Angular-url]: https://angular.io/
[Svelte.dev]: https://img.shields.io/badge/Svelte-4A4A55?style=for-the-badge&logo=svelte&logoColor=FF3E00
[Svelte-url]: https://svelte.dev/
[Laravel.com]: https://img.shields.io/badge/Laravel-FF2D20?style=for-the-badge&logo=laravel&logoColor=white
[Laravel-url]: https://laravel.com
[Bootstrap.com]: https://img.shields.io/badge/Bootstrap-563D7C?style=for-the-badge&logo=bootstrap&logoColor=white
[Bootstrap-url]: https://getbootstrap.com
[JQuery.com]: https://img.shields.io/badge/jQuery-0769AD?style=for-the-badge&logo=jquery&logoColor=white
[JQuery-url]: https://jquery.com 

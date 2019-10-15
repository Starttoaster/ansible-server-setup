Required CI variables:
 - CREATE_USERNAME the username to be created by this playbook
 - HOST the IP address of the host to connect to
 - HOST_USER the username of the remote host
 - HOST_PASSWORD the password of the remote host's user account

Required CI File variables:
 - id_rsa the private key to install
 - id_rsa_pub the public key to install

TODO:
 - Make usable on other distros (currently works on CentOS 7)

 # ansible-server-setup
Ansible playbook for installing Docker, some basic packages, configuring a user account with your public keys, and basic SSH hardening. 
Just Works<sup>TM</sup> on CentOS, Red Hat, Debian, and Ubuntu servers.

### Use in GitLab CI

I use this in GitLab CI to configure remote hosts. All you need is a GitLab repo (you can fork this one and make changes as you see fit for you) and a few CI environment variables:

| Variable | Function |
| ---- | ---- |
| CREATE_USERNAME | The username to be created on the remote server by this playbook |
| HOST | The IP address of the host to connect to |
| HOST_PORT | The SSH port of the host to connect to | 
| HOST_USER | The username of the remote host to initially connect to |
| HOST_PASSWORD | The password of the remote host's user account |

You also need to define a couple of files in your GitLab CI variables. You can select file variables in the "type" dropdown.

| File | Function |
| ---- | ---- |
| id_rsa | This file should contain your private key |
| id_rsa_pub | This file should contain your public key |

To run on a new host, reset the environment variables as needed, and re-run the CI job!

### Manually Use

Run the playbook: `ansible-playbook playbook.yml --extra-vars "username=$YOUR_NEW_USERNAME"`

Store your private and public keys in the same directory as `playbook.yml`. This playbook installs keys for your new user account. 
They should be named `id_rsa` and `id_rsa.pub`. 

This playbook uses the `new_hosts` group that should be defined in your inventory file.

### Disclaimer

This playbook makes large changes to your server's sshd_config file. Do not run this playbook unless you understand what it does OR have some method besides SSH of logging into it.

Specifically, the playbook disables password based SSH logins, and root account SSH logins, which is important for securing any public facing Linux server. This playbook configures a sudo user of the name you specify at runtime. You set the user's password at first login.

### What does this install?

 - docker
 - docker-compose
 - git
 - fail2ban

Some various docker pre-requisite packages. Aptitude is also installed if ran on a Debian or Ubuntu system, as it is used by Ansible.

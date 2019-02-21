## Ansible playbook to configure Ubuntu droplets on Digital Ocean

Ansible playbook to configure Digital Ocean Ubuntu droplets. It implements most of the recommended steps [here](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-18-04).

### Tasks performed are
1. Install python (required for ansible)
2. Setup timezone
3. Create non root, sudo admin group and user
4. Update SSHD configuration to disallow root login and optionally change port number
5. Setup UFW to only enable SSH
6. Add authorized users SSH public key

### Steps

1. Create a droplet, select Ubuntu 16.04 or 18.04 for operating system. Note the IP address
1. Update ansible hosts file
- change **<HOSTNAME>** with hostname for droplet: eg: www1
- change **<IP_ADDRESS>** with IP of droplet. You can find this from Digital Ocean control panel for droplet
2. Copy your SSH public key (usually id_rsa.pub) to roles/common/files
3. Update the defaults file (roles/common/defaults/main.yml)
- Change **ADMIN\_GROUP** to desired group name
- Change **ADMIN\_USER** to desired username
- Change **ADMIN\_PASSWORD** to desired password. Right now hardcoded to **changemenow**. Linux machines require crypted password, to generate one please follow instructions [here](https://docs.ansible.com/ansible/latest/reference_appendices/faq.html#how-do-i-generate-crypted-passwords-for-the-user-module). Alternatively you can leave the default which is **changemenow** and immediately change it after you first login 
- **PUBLIC\_KEYS** If using a different public key name other than id\_rsa.pub, please change it here
- **SSHD\_PORT** I prefer to have a non standard SSHD port, so please change it if desired
- Change **TIMEZONE** if desired

4. The Ubuntu droplets don't have python installed, so install it first
```BASH
ansible-playbook -i hosts install_python_minimal.yml
```
5. Run the common playbook
```
ansible-playbook -i new common.yml
```
6. Login to server
```
ssh <ADMIN_USER>@<IP_ADDRESS>
```
**Note** If you didn't change default password (**changemenow**), now is a good time to change it




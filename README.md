create_ssh_users
=================

This role creates ssh users on the desired host/s and secures the ssh server
configuration.

Role Variables
--------------
All these variables are defined in defaults/main.yml

   configure_sshd: [yes|no]  If true ssh server will be secured with the options defined in the tasks file configure_sshd.yml
   permit_only_ssh_key: [yes|no] If true the ssh to access to the host/s will only be possible using public key.
   disable_root_login: [yes|no] If true access to the host/s using root won't be possible. So make sure you have created another user before disabling root access.
   sshd_config_file: The ssh server configuration file location , default : /etc/ssh/sshd_config 
   allowed_ssh_users: User/s that will be allowed to have ssh access to the host/s.
   ssh_port: The port on which the ssh server will be listening, default 22

   ssh_users:
     - user_name: user name to create
       user_pass: user password
       group_name: user group name
       sudo: [yes|no] create sudo group and add newly created user into this group
       bash: [yes|no] to allow shell access
       sudo_without_pass: [yes|no] allow newly created user to sudo without password
       comment: an description for the created

To create several users at once just copy and paste the following block personalizing the variables under ssh_users:

   - user_name: 
     user_pass:
     group_name: 
     sudo: [yes|no]
     bash: [yes|no]
     sudo_without_pass: [yes|no]
     comment: ""

Tags
-----

You can use following tags to limit actions/configurations to perform on the host/s.

   update_keys : to update authorized public keys for the user/s
   sshd_config : to configure ssh server
   allow_ssh_user : to update ssh allowed users


Requirements
------------

- ansible 2.7.7


Example Playbook
----------------

You can an playbook as described below :

    - hosts: all
      roles:
        - { role: create_ssh_users }
      
If ansible is being executed by non root user with privileged access :
      
      - hosts: all
        roles:
          - { role: create_ssh_users }
        become: true

For the direct one line command execution on localhost use :
   
   ansible-playbook --connection=local --inventory 127.0.0.1, create_ssh_users.yaml
   
To run on the remote hosts using inventory :
   
   ansible-playbook -i remote_hosts create_ssh_users.yaml
   
To run the role using tags :
   
   ansible-playbook -i remote_hosts create_ssh_users.yaml --tags "sshd_config"


License
-------

BSD

Author Information
------------------

	Raman Deep
	31th March 2020
	deep.raman85@gmail.com

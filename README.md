# Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults
Mastering-Ansible-Section-6----SSH Key Based Authentication and Vaults


**** 
SSH Key Based Authentication and Vaults
****

Make a video on this showing the legacy way - hardcoded into the inventory file
The cleaner way to do it - 


![image](https://github.com/bowlercbtlabs/Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults/assets/120626722/c4fed499-6b59-40a3-9c6c-a97747560ccc)



Order of ansible operations for user authentication:

1) It will take the host variables first if defined:

![image](https://github.com/bowlercbtlabs/Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults/assets/120626722/25803e1e-75c0-4804-8e3e-00f2d5e5fe77)

2) Then it looks at the group variable next:

![image](https://github.com/bowlercbtlabs/Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults/assets/120626722/8968e2da-3b04-4966-b786-b7bddc233cf7)


3) Lastly, it takes the common group variable:

![image](https://github.com/bowlercbtlabs/Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults/assets/120626722/f53e71d2-b150-42cb-952d-daa7df3ede8d)

- Add the ansible_user to the playbook so you can demonstrate which user is trying to access the network device as shown below:

![image](https://github.com/bowlercbtlabs/Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults/assets/120626722/14f69d6e-faef-44bf-ade6-25d71f76e20e)


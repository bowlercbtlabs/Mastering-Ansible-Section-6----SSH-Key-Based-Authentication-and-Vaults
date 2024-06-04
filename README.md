# Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults
Mastering-Ansible-Section-6----SSH Key Based Authentication and Vaults


**** 
SSH Key Based Authentication and Vaults
****

- The legacy way - user credentials hardcoded into the inventory file

- The cleaner more secure way to do it - RSA Key Generation from Ansible Server and Import the key onto the target devices



- First we will show you the legacy way to store user credentials as shown below in the sample inventory file:

![image](https://github.com/bowlercbtlabs/Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults/assets/120626722/c4fed499-6b59-40a3-9c6c-a97747560ccc)

User Credentials can be stored in multiple locations in the inventory file, here is the order of ansible operations for user authentication:

1) It will take the host variables first if defined:

![image](https://github.com/bowlercbtlabs/Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults/assets/120626722/25803e1e-75c0-4804-8e3e-00f2d5e5fe77)

2) Then it looks at the group variable next:

![image](https://github.com/bowlercbtlabs/Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults/assets/120626722/8968e2da-3b04-4966-b786-b7bddc233cf7)


3) Lastly, it takes the common group variable:

![image](https://github.com/bowlercbtlabs/Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults/assets/120626722/f53e71d2-b150-42cb-952d-daa7df3ede8d)

- Add the ansible_user to the playbook so you can demonstrate which user is trying to access the network device as shown below:

![image](https://github.com/bowlercbtlabs/Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults/assets/120626722/14f69d6e-faef-44bf-ade6-25d71f76e20e)

- as you can see from the below output, admin2 is being used to authenticate to R1 and admin1 is being used to authenticate to R2

![image](https://github.com/bowlercbtlabs/Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults/assets/120626722/2fffba23-39f8-42a5-8bec-ea221eb19a1f)


****
Generate RSA Key and Import it to your target devices (Cisco routers in this example) 
****

- On the Ubuntu/Ansible device generate the RSA key pair:

![image](https://github.com/bowlercbtlabs/Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults/assets/120626722/6cb28d32-5d60-4b12-adbe-902a2d782ef8)

- We now see the public key file has been created (id_rsa.pub), this is what we import to the target devices:

![image](https://github.com/bowlercbtlabs/Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults/assets/120626722/817f0454-9932-4340-9091-2d5c1647d22e)

![image](https://github.com/bowlercbtlabs/Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults/assets/120626722/8e2daff3-fc70-471d-852e-449fa6d7833f)

- Login to the target device and run the following commands:

![image](https://github.com/bowlercbtlabs/Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults/assets/120626722/f7d1cfdc-9bfe-4249-934c-7414fa80d63f)

- In order to copy/paste the RSA key into the Cisco device we need to reformat the file using the 'fold' command as shown:

![image](https://github.com/bowlercbtlabs/Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults/assets/120626722/247d5de3-1fe6-4d3e-8524-f4eca9ab07ab)

- Now copy the content into the target device:

![image](https://github.com/bowlercbtlabs/Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults/assets/120626722/c8418195-9d76-42ed-9fb9-1eb1298c8287)

![image](https://github.com/bowlercbtlabs/Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults/assets/120626722/3b940c86-dc77-4b01-883f-b3dfbbdda2b7)

- Then give the user permissions:

![image](https://github.com/bowlercbtlabs/Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults/assets/120626722/20cb60ab-d20e-4ad1-b132-49b73cf56503)

- If you try to ssh now into the router it is still asking for the password:

![image](https://github.com/bowlercbtlabs/Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults/assets/120626722/b88f7b86-970a-4d83-b892-467f8d2e4a8e)

- This is because the Cisco device is running an older version of SSH, so we need to update the Ubuntu server to allow the connection:

![image](https://github.com/bowlercbtlabs/Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults/assets/120626722/57c0ba29-da26-4783-a225-50043c9a9310)

- Uncomment out this line and save the file:

![image](https://github.com/bowlercbtlabs/Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults/assets/120626722/027ebf81-248a-47d7-bf79-e18b71660c1e)

- Now try to ssh again to the Cisco device:

![image](https://github.com/bowlercbtlabs/Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults/assets/120626722/d0d607c3-50f4-4165-a47d-9b213fff6c3b)

- It is working, you have sucessfully created a RSA key based authentication connection to the Cisco device using key pair hosted from the Ubuntu/Ansible server

- Now change your ansible inventory file to tell ansible to use the new username you created on the Cisco device 'ansible_admin':

![image](https://github.com/bowlercbtlabs/Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults/assets/120626722/883e8018-a667-488a-a12e-2f2e88bbeac7)

![image](https://github.com/bowlercbtlabs/Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults/assets/120626722/cd27f808-f3e0-4743-99b3-62afce5b87b5)

- By default ansible is able to find the default location of the RSA key pair, but you can tell ansible a custom RSA key pair location on your server by just adding the location of where to find the key such as the following example:

![image](https://github.com/bowlercbtlabs/Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults/assets/120626722/588aad41-6eae-4e8e-92db-fe3f7d495a61)

![image](https://github.com/bowlercbtlabs/Mastering-Ansible-Section-6----SSH-Key-Based-Authentication-and-Vaults/assets/120626722/bf304347-8908-46e5-9413-69b446a25867)








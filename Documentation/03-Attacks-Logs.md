# Attacker Machine

- Go to Virtual machines > Create > > Create a new Ressource Group as 'RG-Attacker' and name the machine as 'attacker-vm'
- On network tab > create a new vnet for windows > eg : attacker-vm-vnet > Review & Create
     
# RDP Attacker Windows machine using its Public IP

![attacker_RDP_Win](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/attacker_RDP_Win.png)

## Attacker Logs 

- let's RDP from Attacker_VM to Windows_VM and put wrong credentials so we can generate some logs

- Download SSMS on the attacker machine and try to connect with 2 wrong password and 1 correct password to generate logs.

![attacker-ssms-fail](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/attacker-ssms-fail.png)

--- 

## Linux logs 

- now let's generate some logs from our linux, go to powershell and connect ssh with wrong passwords.

![attacker-ssh-connect-fail](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/attacker-ssh-connect-fail.png)

----

# Let's go back to our Windows-vm and look for the logs 
- go to event viewer, and we can see some logs from our previous exercice 

![Event_view_sql_fail](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/Event_view_sql_fail.png)

![event_attacker-login_fail](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/event_attacker-login_fail.png)


---

## now let's connect on linux machine and look for logs

type: cd /var/log 

type: cat auth.log | grep password to view logs

![cat_auth_log](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/cat_auth_log.png)

![cat_auth_log_fail_success](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/cat_auth_log_fail_success.png)



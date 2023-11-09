<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube Coming Soon](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell/ PowerShell ise

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Create 2 vms, one with server 2022 (server), one with windows 10 (client).
- Configure the server with a static IP address, and allow ipv4 ICMP traffic from the client to the server
- Add Active Directory Domain Services to the server, create a new forest, and promote as a Domain Controller
- Use Active Directory users and computers to create an admins and employees organizational unit
- Configure the client's dns settings to use the server vm as the DNS server.
- Use the admin account to join the client to the domain, and enable normal domain users to use Remote Desktop Protocol (RDP) to access the client.
- Use a script in Powershell ise to create 1000+ users.
- Create a new security group to assign a some users to

<h2>Deployment and Configuration Steps</h2>

1\. Create 2 VM's. One with Windows server (called server in this lab), and one with windows 10 (called client in this lab). create the server vm first and make sure they are both on the same network.


2\. Connect with rdp to the server and use aother rdp to connect to the client using the usernames and passwords 2you used when creating the VMs.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/a1c1e13d-d850-4d35-a2d3-e4e599fbc943/screenshot.jpeg?tl_px=920,283&br_px=1780,764&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


3\. ping yoyur server's private ip address. observe you cannot ping it.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-09/2b5aec3a-8e92-4f7b-a782-384634700a2c/user_cropped_screenshot.jpeg?tl_px=0,0&br_px=1228,772&force_format=png&width=1120.0)



4\. In the server, search and open wf.msc.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/6249ee2d-433d-43aa-b123-c866bb8a9a9c/screenshot.jpeg?tl_px=0,670&br_px=1376,1440&force_format=png&width=1120.0)


5\. Click on inbound rules

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/c8110193-f6b7-46b8-9d0f-49aaf9242821/screenshot.jpeg?tl_px=105,15&br_px=964,496&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


6\. Expand the window and sort by protocol.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/8b779fa2-4844-404a-b521-20a9b6b389a9/screenshot.jpeg?tl_px=744,0&br_px=1604,480&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,87)


7\. Right click and enable the 'Core Networking Diagnostics - ICMP echo request' in the ipv4 section.  apply the setting.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/4bd736da-4f68-4992-bd3a-f64c55ae74fd/screenshot.jpeg?tl_px=43,182&br_px=902,663&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


8\. Restart the server vm

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/ceaa4d01-9b00-4ee2-8d44-9cfa1c726ee3/screenshot.jpeg?tl_px=358,0&br_px=1218,480&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,190)


9\. Reconnect to the server with RDP

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/69de8858-ea69-403a-aa6c-6b46cb0f9856/screenshot.jpeg?tl_px=915,282&br_px=1775,763&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


10\. In the client, try to ping the server again. to make sure the changes took effect.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/7c84e6fa-af55-4ad0-b4ce-0f4a3bdbaf6c/screenshot.jpeg?tl_px=343,478&br_px=2063,1440&force_format=png&width=1120.0)


11\. Back to the server, go in the server manager, click add roles and features.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/11ab1d6f-5323-46ae-ade0-9da97b8256c3/screenshot.jpeg?tl_px=62,7&br_px=921,488&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


12\. Find Active Directory Domain Services

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/5afff1b7-504f-454b-8d7f-869b99dae4fb/screenshot.jpeg?tl_px=1,137&br_px=860,618&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


13\. Click add features and click next and continue with the standard install unless you need the options.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/ec6578b4-08bf-486c-85c5-90211126d230/screenshot.jpeg?tl_px=200,392&br_px=1060,873&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


14\. Click install.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/df211a79-038c-400b-8326-eeeb1fcb8536/screenshot.jpeg?tl_px=388,476&br_px=1248,957&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


15\. Once its done installing, click on the flag in the top right side

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/1d271d32-98ce-43bf-8c19-41383757a7f0/screenshot.jpeg?tl_px=1700,0&br_px=2560,480&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=566,26)


16\. Promote this server to a domain controller.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/c67f9c32-1091-4f4e-847f-476fb11a0210/screenshot.jpeg?tl_px=1700,0&br_px=2560,480&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=452,152)


17\. Add new forest, unless you need to do the other options.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/2f5f9472-ec02-433d-af17-47946307bf19/screenshot.jpeg?tl_px=117,219&br_px=976,700&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


18\. Name your domain

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/fc82f2ae-c5d6-45df-9c79-f68a9d38ee5d/screenshot.jpeg?tl_px=252,252&br_px=1235,801&force_format=png&width=983)


19\. Continue through the installer and create a password.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/201ee022-713a-4c8b-9100-62072bbbad3d/screenshot.jpeg?tl_px=360,364&br_px=1220,845&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


20\. Click next through the standard installation unless you need the other options.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/f2e4d845-4454-457f-98e3-7a469fec8474/screenshot.jpeg?tl_px=336,549&br_px=1196,1030&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


21\. Click install.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/9ff1447f-a401-4100-bca6-8d7ddf624b4e/screenshot.jpeg?tl_px=442,552&br_px=1302,1033&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


22\. Your server will restart.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/c5b88125-5354-4ba2-a791-1811b1a8a630/screenshot.jpeg?tl_px=1107,523&br_px=1967,1004&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


23\. Reconnect to the server.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/e4d4c3db-6274-4e10-b514-576edea82650/screenshot.jpeg?tl_px=939,278&br_px=1799,759&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


24\. After the server is promoted, you will need to click on use a different account.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/5fa156c6-e447-43d3-8a5b-a4adcc4584d8/screenshot.jpeg?tl_px=741,381&br_px=1601,862&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


25\. The username changed to  'yourdomain\username'. the password shoul be the same as when you created the VM

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/8e336738-2b22-41f2-9a08-4c65519c0cae/screenshot.jpeg?tl_px=311,141&br_px=2031,1102&force_format=png&width=1120.0)


26\. Search and open 'Active Directory Users and Computers'

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/7e4958fa-57a7-4469-af7a-c8f4c04207db/screenshot.jpeg?tl_px=0,780&br_px=859,1261&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=230,212)


27\. Click on your domain name and expand the arrow.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/db69529e-f83f-4efa-85c8-c4a7b703d48a/screenshot.jpeg?tl_px=154,242&br_px=1014,723&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


28\. Click on your domain name

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-09/7037fa82-930d-4f98-b0ae-4f37cf62070d/screenshot.jpeg?tl_px=214,13&br_px=1074,494&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


29\. Click on this to create a new organizational unit

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-09/69b1d7aa-8fb1-40f4-9368-0eb0a2151040/screenshot.jpeg?tl_px=443,0&br_px=1303,480&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,166)


30\. Name it '\_ADMINS', or whatever you want.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-09/f192c675-795e-46e7-ab3b-5784f9c0ae35/screenshot.jpeg?tl_px=0,0&br_px=1719,961&force_format=png&width=1120.0)


31\. Click on your domain and create another organizational unit  named '\_EMPLOYEES'

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-09/5a513a3b-65bd-429b-b071-f9e2bfc46e6b/screenshot.jpeg?tl_px=15,0&br_px=1734,961&force_format=png&width=1120.0)


32\. Click on your '\_ADMINS' folder and create a user

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-09/f75a1117-c73f-4969-87c3-0a0e232a03f5/screenshot.jpeg?tl_px=445,0&br_px=1305,480&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,164)


33\. Give them a name

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-09/b4ce175f-0e8b-4ff1-bc1b-f7e23d6845d9/screenshot.jpeg?tl_px=446,261&br_px=1306,742&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


34\. Give them a password ( best practices would be to give them a default password, have them change it at login, and make it expire)

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-09/f8c2c52c-15af-4b8c-87f4-26459d6c617a/screenshot.jpeg?tl_px=436,257&br_px=1296,738&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


35\. Click finish

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-09/72b965ba-6f7e-44de-bc39-a6da9737bd73/screenshot.jpeg?tl_px=436,256&br_px=1296,737&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


36\. Right click on your admin and click add to group

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-09/c60e12ef-f59d-444b-808a-162de0bbe2b6/screenshot.jpeg?tl_px=425,66&br_px=1285,547&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


37\. Add them to type 'Domain Admins', click check name, then click ok.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-09/e0be4216-cc32-43a1-b1da-d036783a65ab/screenshot.jpeg?tl_px=443,149&br_px=1303,630&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


38\. close out of RDP

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-09/28b5955a-989b-48e7-ad02-d158d1b630be/screenshot.jpeg?tl_px=1090,0&br_px=1950,480&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,-20)


39\. Go back into RDP and log into your admin account.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-09/417682aa-d3fd-4d00-84c6-47b26c9b3036/screenshot.jpeg?tl_px=396,0&br_px=2116,961&force_format=png&width=1120.0)

<h2>Connecting client to domain and enable RDP for domain users</h2>

1\. Go to your client in the azure portal and click on networking.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/24a114bb-9a86-45c7-bc60-0ecce2590e46/screenshot.jpeg?tl_px=0,221&br_px=859,702&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=347,212)


2\. Click on your vnet

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/e185249d-5096-4ee3-bff2-f01478193adb/screenshot.jpeg?tl_px=403,165&br_px=1263,646&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


3\. Click "DNS servers"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/486388f9-f1b3-4afa-85dd-6c09339760a5/screenshot.jpeg?tl_px=0,477&br_px=859,958&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=20,212)


4\. Click custom and put the ip address of your server. Click save

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/1c1b82f9-d496-4873-a0cd-cb7b55f2b0a0/screenshot.jpeg?tl_px=0,135&br_px=1146,776&force_format=png&width=1120.0)


5\. Restart the client

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/00c7703e-b749-4912-960e-e2c66d6a5887/screenshot.jpeg?tl_px=222,97&br_px=1082,578&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


6\. Reconnect to the client in RDP

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/b0d09a59-3bea-41b9-80d1-6570895a5307/screenshot.jpeg?tl_px=899,272&br_px=1759,753&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


7\. Open command prompt and type 'ipconfig -all' and make sure the DNS server's ip address is the same as your server's

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/e73d656c-9571-4a15-8270-c6d40539cfab/screenshot.jpeg?tl_px=116,364&br_px=976,845&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


8\. Go to settings-&gt; system

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/b57f2207-b3e1-4586-841d-ef3aa779df59/screenshot.jpeg?tl_px=0,107&br_px=859,588&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=172,212)


9\. Click on about

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/d9d54ea2-ebcd-4edf-87d5-c27ba80af2c1/screenshot.jpeg?tl_px=0,597&br_px=859,1078&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=65,212)


10\. Click advanced system settings

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/3f519754-75da-4713-9690-fbc3ffab86a2/screenshot.jpeg?tl_px=605,216&br_px=1465,697&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


11\. Click to change the domain

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/1ff9deb1-8ff7-4225-b7f9-309cc24dd6cf/screenshot.jpeg?tl_px=647,374&br_px=1507,855&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


12\. Type your domain name

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/5450db1e-be3e-4ede-9a21-a2edffac018f/screenshot.jpeg?tl_px=1,137&br_px=1721,1098&force_format=png&width=1120.0)


13\. Login with your admin's credentials

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/0a813f7f-428a-4781-bd55-7334532bb258/screenshot.jpeg?tl_px=95,225&br_px=1815,1186&force_format=png&width=1120.0)


14\. Restart the client and reconnect with RDP

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/e271a56f-ce06-4264-8f33-507899655c61/screenshot.jpeg?tl_px=917,271&br_px=1777,752&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


15\. Back to the server

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/60404aa7-aa41-428b-9980-b28c22d00811/screenshot.jpeg?tl_px=709,959&br_px=1569,1440&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,375)


16\. In the server, go to Active Directory users and computers

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/88235c9b-8955-4c40-81ec-a3e2d09d26be/screenshot.jpeg?tl_px=0,820&br_px=859,1301&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=201,212)


17\. Expand your domain

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/a638788c-9acf-4014-b4fc-34e3643a6dc6/screenshot.jpeg?tl_px=0,0&br_px=859,480&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=83,190)


18\. Create an organizational unit called '_CLIENTS'

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/799714dc-aaf0-43c2-9944-9b928fd8c223/screenshot.jpeg?tl_px=0,45&br_px=859,526&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=101,212)


19\. Go into 'computers' and drag the client computer into the '_CLIENTS' folder

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/e986729f-751a-458e-903e-6694f1cfa0ca/screenshot.jpeg?tl_px=0,0&br_px=859,480&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=303,181)


20\. Log out of the client computer and reconnect in RDP. Click use a different account

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/f7c12ae2-517d-455d-abe2-28eda96cfbcd/screenshot.jpeg?tl_px=729,364&br_px=1589,845&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


21\. Log in with your admin account

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/9b2e978e-953c-4b5b-9af8-1f127f2ffaaa/screenshot.jpeg?tl_px=299,124&br_px=2018,1085&force_format=png&width=1120.0)


22\. Go to settings-&gt; system

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/d2d1f6fd-edd6-4794-a429-1bf4827e0138/screenshot.jpeg?tl_px=0,30&br_px=859,511&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=234,212)


23\. Click remote desktop

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/fd3c2be2-b1af-4d9c-bcaf-f00edf54b81b/screenshot.jpeg?tl_px=0,542&br_px=859,1023&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=199,212)


24\. Click 'Select users that can remotely access this PC' and click add

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/ac3206ff-3405-4fc3-9b32-9a1b5f7b939e/screenshot.jpeg?tl_px=88,313&br_px=948,794&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


25\. Type ' Domain users' and click check names

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/ac7ab18c-7285-49bf-ac49-c11a69f0ac0c/screenshot.jpeg?tl_px=429,286&br_px=1289,767&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


26\. Click ok

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/1e333f4b-d274-4334-933f-5f0704c355ee/screenshot.jpeg?tl_px=272,402&br_px=1132,883&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)

<h2>Creating domain users and logging into the client</h2>
1. Go into the server and search for and run Powershell ISE as administrator

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/c0d931fb-5500-4dec-a148-b9de166d82c8/screenshot.jpeg?tl_px=0,731&br_px=859,1212&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=266,212)


2\. Click new script

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/04891a3e-3491-4608-a750-fdbb5ce2a6cb/screenshot.jpeg?tl_px=0,0&br_px=859,480&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=116,101)


3\. paste the script from <https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1>

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/f30eb920-8288-4738-bf24-7a3b5a53ea6f/screenshot.jpeg?tl_px=0,0&br_px=859,480&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=209,171)


4\. Adjust the number of accounts to create less or more, if you need (i did 1000). Double check you have an organizational unit named exactly '\_EMPLOYEES' or the script won't work (you can edit the script to look for a different OU if you know what you are doing).

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/f43a79ba-1cdc-4640-a54c-cb6a3f4e0dfd/screenshot.jpeg?tl_px=17,0&br_px=877,480&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,175)


5\. Note: default Password for all accounts will be 'Password1'

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/ae55ebae-d0b6-4701-92ec-49b6a5f3e38e/screenshot.jpeg?tl_px=52,0&br_px=912,480&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,168)


6\. Click run script

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/2ece0222-fe55-46c0-a1d5-87211643934e/screenshot.jpeg?tl_px=16,0&br_px=875,480&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,92)


7\. Once done, go into AD: users and computers.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/25f26a4c-f7f9-4e34-a74e-56f6c1f2dce9/screenshot.jpeg?tl_px=0,12&br_px=859,493&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=165,212)


8\. In the employee folder, look at all the random names it created. select one

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/d7f1bc7b-3141-4fcb-92a0-c5ac3e436e5e/screenshot.jpeg?tl_px=0,0&br_px=859,480&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=322,195)


9\. Note the display name,  because it will be used to login to the client

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/5dc8ff83-b131-44cc-8b72-e7eef9669f43/screenshot.jpeg?tl_px=227,0&br_px=1087,480&force_format=png&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,58)


10\. Log out of the client and log in as your new user.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2023-11-06/9be75825-3c24-49c0-8bca-7ecd747e21e8/screenshot.jpeg?tl_px=616,293&br_px=1763,934&force_format=png&width=1120.0)








#
<h1>Using and Understanding DNS</h1>

This is a tutorial explaining the steps our computer takes when interacting with host names on a network using DNS. 
<p>
<h2>Observing/Inspecting Local DNS Cache</h2>

Log in to Client 1 as a Domain Admin and run Windows Powershell as an administrator. We will try to ping & nslookup an arbitrary name, "maintenance". Powershell will return something along the lines of, "maintenance does not exist", the host name maintenance doesn't exist in the local DNS Cache, host file, or DNS server. We can observe our Local DNS Cache by using the commmand "ipconfig /displaydns > dns.txt" and following it up with "notepad dns.txt". 

![1](https://github.com/user-attachments/assets/0d0cbc54-f0e2-487f-bd0a-46146169a9c2)
![2](https://github.com/user-attachments/assets/193adfb7-79b3-496e-afc1-2ce7cab087bc)
</p>
<p>
<h2>Observing & Interacting with DNS Host file</h2>

To observe our client computer's DNS Host file, run Notepad as an administrator. Click File -> click Open -> select "All Files" instead of "Text Documents" -> open folder "drivers" -> open folder "etc" -> open "hosts". We can add a new host "files" and apply a loopback address to it. Save the file and attempt to ping the host "files", our computer now recognizes the host "files" from the client's DNS Host file and can receieve/send pings to "files" successfully. 

![3](https://github.com/user-attachments/assets/053f9a02-fcc2-404b-aa71-f50763e3f35f)
![4](https://github.com/user-attachments/assets/5976392d-ced0-4be7-b6f7-ee02bab2bb93)
![5](https://github.com/user-attachments/assets/871a250b-21fb-4bd9-a899-d2ed5031de40)
</p>
<p>
<h2>Creating a DNS A-Record for "maintenance" on Domain Controller</h2>

Log in to DC 1 as a Domain Admin and navigate to the Windows Search Bar, expand -> Windows Administrative Tools -> click DNS to open up DNS Manager. In DNS Manager, expand -> DC-1 -> expand Forward Lookup Zones -> click mydomain.com -> observe Host(s) & IP Addresses. Right click and select "New Host (A or AAAA...)", we will have the host "maintenance" point to our Domain Controller's Private IP Address (10.0.0.4), finally check the box next to "Create associated pointer (PTR) record" and click Add Host. Note that it's okay to have multiple records pointing to the same IP Address.

![6](https://github.com/user-attachments/assets/ef60e139-fb12-4b81-a615-354ea931b315)
![7](https://github.com/user-attachments/assets/190100d9-4bae-4617-8233-07c094bec759)
![8](https://github.com/user-attachments/assets/5d921761-bdc8-4510-8680-4c931d35946c)
![9](https://github.com/user-attachments/assets/60850951-148c-4335-bc1a-f92c579c8154)

</p>
<p>
<h2>Testing connectivity for new host on Client side</h2>

To test the connectivity of our newly created host go back to Client 1 and ping "maintenance" in Windows Powershell. 

![10](https://github.com/user-attachments/assets/ce652b9c-2df1-4971-b309-785dc1e25d5e)
</p>
<p>
<h2>Changing A-Record's IP Address & Observing changes on Client side</h2>

In our Domain Controller, change the IP Address of "maintenance" to (8.8.8.8). Right click on "maintenance" -> click Properties -> change IP Address to 8.8.8.8 -> click Apply -> click OK. Then go back to Client 1 and ping maintenance again, notice how the IP Address is still 10.0.0.4 instead of 8.8.8.8. This is because our computer will always search for the host in the network with the following priorities, 1. Local DNS Cache, 2. Host File, 3. DNS Server. If we observe our Local DNS Cache again, we can see that our host "maintenance" is still pointing to the IP Address 10.0.0.4. To see the changes made from our DC, we will need to flush out our client's Local DNS Cache so that it will recognize 8.8.8.8 as the host's new IP Address.

![11](https://github.com/user-attachments/assets/d6b2958e-3538-4202-b0c7-598d201d0d85)
![12](https://github.com/user-attachments/assets/877f3cf9-5347-49f0-af6a-f5eb919644a1)
![13](https://github.com/user-attachments/assets/dca51e27-6823-4d26-a88d-0f400b8dae49)

</p>
<p>
<h2>Flushing Client 1's Local DNS Cache</h2>

Run the command "ipconfig /flushdns" in PowerShell, running this command will clear our entire Local DNS Cache and force our computer to do a fresh DNS lookup the next time we try pinging our host. Run "ipconfig /displaydns" to observe our flushed out Local DNS cache. Next, attempt to ping "maintenance" once more to see if our new IP Address has been implemented.

![14](https://github.com/user-attachments/assets/6317cccc-4828-47a5-8335-a4796514dc9c)
![15](https://github.com/user-attachments/assets/efb68242-a438-4f7d-b151-37142faec4b9)
</p>
<p>
<h2>Creating a CNAME record</h2>

Create a new CNAME record in our DNS Manager by right clicking and select "New Alias (CNAME)...". Make the alias name "search" and set "www.google.com" as our FQDN, click OK. When we ping the alias name "search", it will ping Google's IP Address. Back on Client 1, ping "search" and observe.

![16](https://github.com/user-attachments/assets/833fdb2b-bf4a-44f3-8fea-01de27893542)
![17](https://github.com/user-attachments/assets/d7bdcabd-ffc3-4213-b28d-edbefea9cb82)
</p>

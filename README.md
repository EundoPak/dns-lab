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

<h2>Creating a DNS A-Record for "maintenance" on Domain Controller</h2>

Log in to DC 1 as a Domain Admin and navigate to the Windows Search Bar, expand -> Windows Administrative Tools -> click DNS to open up DNS Manager. In DNS Manager, expand -> DC-1 -> expand Forward Lookup Zones -> click mydomain.com -> observe Host(s) & IP Addresses. Right click and select "New Host (A or AAAA...)", we will have the host "maintenance" point to our Domain Controller's Private IP Address (10.0.0.4), finally check the box next to "Create associated pointer (PTR) record" and click Add Host. 

![6](https://github.com/user-attachments/assets/ef60e139-fb12-4b81-a615-354ea931b315)
![7](https://github.com/user-attachments/assets/190100d9-4bae-4617-8233-07c094bec759)
![8](https://github.com/user-attachments/assets/5d921761-bdc8-4510-8680-4c931d35946c)
![9](https://github.com/user-attachments/assets/60850951-148c-4335-bc1a-f92c579c8154)




</p>

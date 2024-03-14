# PI-Hole-Tutorial
## Install
Update Package Repos

```bash
sudo apt update
```

Start PI Hole Install

```bash
curl -sSL https://install.pi-hole.net | sudo PIHOLE_SKIP_OS_CHECK=true bash
```

`Select: OK`

`Select: OK`

This page and the next ones deal with static IP addresses. If setting this up permanently I highly recommend setting a static IP. For this demo though we will not need one.

`Select: Continue`

`Select: Yes Set static IP using current values -> Continue` 

`Select: OK`
 
This page is asking for the forwarder server. If setting this up anywhere but the UNF network select the default. For this demo we need to select custom.
	
`Select: Custom`

Then enter the IP of the UNF DNS
		
`Enter: 139.62.200.185`

`Select: OK`

`Select: Yes`

This page is asking if you want to include the default block lists.

`Select: Yes`

The next page is asking if you want the web portal installed. This is highly recommended since it allows for easy management of the PI-Hole software.
	
`Select: Yes`

`Select: Yes`

This page is asking if the DNS queries sent to PI-Hole should be logged. For this demo I recommend enabling this but if you want more privacy select no.

`Select: Yes`

Leave defaults
	
 `Select: Continue`

## Save Web Interface Password
After the installation is completed the password for the web interface is shown. This needs to be saved to a file for later reference

```bash
nano /home/admins/Desktop/pi-hole-info
```

Paste the login into the file. 

Then hit ctrl+x. 

Then hit y and enter to save.

## Set DNS Server for PI-Hole
For DNS request to be sent to our PI-Hole the PI-Hole's IP address must be set as the primary DNS. Raspbian sets this in the file below.

```bash
sudo nano /etc/dhcpcd.conf
```

Go to the bottom of the file and modify the line below to match

`static domain_name_servers=127.0.0.1`

The address 127.0.0.1 always points to the localhost/computer.

Then hit ctrl+x

Then hit y and enter to save.

Next we need to reset the ethernet adapter so it uses the new DNS server.

```bash
sudo ip link set down eth0
```

```bash
sudo ip link set up eth0
```

Then print the resolv.conf file to the console to verify the setting.

```bash
cat /etc/resolv.conf
```

Print out should contain

`nameserver 127.0.0.1`

## PI-Hole Interface
Open web browser and go to

`localhost/admin`

Provide password saved previously. 
This is the web interface that comes with PI-Hole.

Note the Domains on Adlist count.

Note the number of queries and queries blocked.

Open a new tab and go to www.cnn.com.

Then go back to web interface and notice number of queries blocked is higher.

Open queries blocked - if needed they can be whitelisted from this page.

## Block List
Currently PI-Hole is only using the default block lists.

More block list can be added by going to the Adlists menu in the web interface.

At the top of this page is a text box you can place URLs to a block list.

### Firebog Block List
Lots of block list exist on the internet but firebog has then in a format that imports directly into PI-Hole.

Go to the site below and pick the desired list. For this demo select all list - this may block more than what we want though.

`https://v.firebog.net/hosts/lists.php`

Once the desired list is selected a list of files will be presented. Copy the entire list and then paste it into the Address text box in the web interface.

Then click on "Add"

This does not add the block list to the Database automatically. Open a new terminal and run.

```bash
pihole -g
```

Once that completes go back to the homepage of the web interface and note how many domains are now blocked




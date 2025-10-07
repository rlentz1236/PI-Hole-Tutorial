# PI-Hole-Tutorial
## Install
Update Package Repos

```bash
sudo apt update
```
![update](https://raw.githubusercontent.com/rlentz1236/PI-Hole-Tutorial/main/Capture1.PNG "sudo apt update sample output")

Start PI Hole Install

```bash
curl -sSL https://install.pi-hole.net | sudo PIHOLE_SKIP_OS_CHECK=true bash
```
![update](https://raw.githubusercontent.com/rlentz1236/PI-Hole-Tutorial/main/Capture2.PNG "PI-Hole Install")
`Select: OK`

![update](https://raw.githubusercontent.com/rlentz1236/PI-Hole-Tutorial/main/Capture3.PNG "PI-Hole Install")
`Select: OK`

This page and the next ones deal with static IP addresses. If setting this up permanently I highly recommend setting a static IP. For this demo though we will not need one.
![update](https://raw.githubusercontent.com/rlentz1236/PI-Hole-Tutorial/main/Capture4.PNG "PI-Hole Install")
`Select: Continue`
 
This page is asking for the forwarder server. If setting this up anywhere but the UNF network select the default. For this demo we need to select custom.

![update](https://raw.githubusercontent.com/rlentz1236/PI-Hole-Tutorial/main/Capture7.PNG "PI-Hole Install")	
`Select: Custom`

Then enter the IP of the UNF DNS

![update](https://raw.githubusercontent.com/rlentz1236/PI-Hole-Tutorial/main/Capture8.PNG "PI-Hole Install")		
`Enter: 139.62.200.185`

`Select: OK`

![update](https://raw.githubusercontent.com/rlentz1236/PI-Hole-Tutorial/main/Capture9.PNG "PI-Hole Install")
`Select: Yes`

This page is asking if you want to include the default block lists.

![update](https://raw.githubusercontent.com/rlentz1236/PI-Hole-Tutorial/main/Capture10.PNG "PI-Hole Install")
`Select: Yes`

This page is asking if the DNS queries sent to PI-Hole should be logged. For this demo I recommend enabling this but if you want more privacy select no.
![update](https://raw.githubusercontent.com/rlentz1236/PI-Hole-Tutorial/main/Capture13.PNG "PI-Hole Install")
`Select: Yes`

Leave defaults
![update](https://raw.githubusercontent.com/rlentz1236/PI-Hole-Tutorial/main/Capture14.PNG "PI-Hole Install")
 `Select: Continue`

## Save Web Interface Password
After the installation is completed the password for the web interface is shown. This needs to be saved for later reference. I recommend taking a picture of the output with your phone or saving the password to a text file on the PI.
![update](https://raw.githubusercontent.com/rlentz1236/PI-Hole-Tutorial/main/Capture15.PNG "PI-Hole Password")
`Select: OK`

Take a picture of the password or copy the password from the screen below and save it to a file.
![update](https://raw.githubusercontent.com/rlentz1236/PI-Hole-Tutorial/main/Capture16.PNG "PI-Hole Password")

## Update PI-Hole Database
For PI-Hole to work the database needs to be updated. Which can be done by running the command below.

```bash
sudo pihole -g
```

## Set DNS Server for PI-Hole
For DNS request to be sent to our PI-Hole the PI-Hole's IP address must be set as the primary DNS. Raspbian sets this using the commands below.

The address 127.0.0.1 always points to the localhost/computer. This forces the PI to use itself as the primary DNS server.

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dns "127.0.0.1"
```

```bash
sudo nmcli connection up "Wired connection 1"
```

Then print the resolv.conf file to the console to verify the setting.

```bash
cat /etc/resolv.conf
```
![update](https://raw.githubusercontent.com/rlentz1236/PI-Hole-Tutorial/main/Capture21-1.png "PI-Hole DNS")

Print out should contain

`nameserver 127.0.0.1`

## PI-Hole Interface
Open web browser and go to

`localhost/admin`

![update](https://raw.githubusercontent.com/rlentz1236/PI-Hole-Tutorial/main/Capture22.PNG "PI-Hole Interface")

Provide password saved previously. 
This is the web interface that comes with PI-Hole.

Note the "Domains on Adlist" count.

Note the number of queries and queries blocked.
![update](https://raw.githubusercontent.com/rlentz1236/PI-Hole-Tutorial/main/Capture23.PNG "PI-Hole Interface")

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

![update](https://raw.githubusercontent.com/rlentz1236/PI-Hole-Tutorial/main/Capture24.PNG "PI-Hole")


Once the desired list is selected a list of files will be presented. Copy the entire list and then paste it into the Address text box in the web interface.

![update](https://raw.githubusercontent.com/rlentz1236/PI-Hole-Tutorial/main/Capture25.PNG "PI-Hole")

![update](https://raw.githubusercontent.com/rlentz1236/PI-Hole-Tutorial/main/Capture26-1.PNG "PI-Hole")

Then click on "Add"

![update](https://raw.githubusercontent.com/rlentz1236/PI-Hole-Tutorial/main/Capture27.PNG "PI-Hole")

This does not add the block list to the Database automatically. Open a new terminal and run.

```bash
sudo pihole -g
```

![update](https://raw.githubusercontent.com/rlentz1236/PI-Hole-Tutorial/main/Capture28.PNG "PI-Hole")

Once that completes go back to the homepage of the web interface and note how many domains are now blocked

![update](https://raw.githubusercontent.com/rlentz1236/PI-Hole-Tutorial/main/Capture29.PNG "PI-Hole")



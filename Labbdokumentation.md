# Labbmiljö, Git, CLI och AI
Anders Schölin - 2026-09-15  
Kurs: Introduktion till yrkesrollen och grunderna i IT-infrastruktur (MYH 2025/4008)  

Uppgiften syftar till att demonstrera kunskap och praktiskta färdigheter i att sätta upp en virtuell labbmiljö, navigera CLI i Linux och Windows, dokumentera och spåra arbetet i Git samt att reflektera kritiskt kring AI-användning.

Dokumentationen är uppdelad i följande delar:
- **Labbmiljö och nätverk:** En beskrivning av uppsättningen och en översiktstabell
- **Kommandoradsgenomförande:** En beskrivning av genomförda kommandon i CLI
- **Git och Versionshantering:** Demonstration av ändringshistoriken i Git
- **AI-logg och Utvärdering:** Kritisk granskning av en AI-prompt med reslutat


## Labbmiljö och Nätverk

Labbmiljön är uppsatt i **Virtualbox**.   
En maskin kör Ubuntu Server 24.04 och en kör Windows 11 Pro (tredje och fjärde maskinen i bilden).  
Båda maskinernas nätverksinställningar är satta till **Host-Only** för att isolera trafiken och möjliggöra intern kommunikation. Maskinerna ska tilldelas IP-adresser enligt tabellen nedan.   

<img src="images/Virtualbox_settings.png" width="350" alt="Virtualbox settings">

### Översiktstabell
| Hostname | Operativsystem | IP-adress | Subnätmask | Standard Gateway |
| :--- | :--- | :--- | :--- | :--- |
| ubuntuserver | Ubuntu Server 24.04 | 192.168.1.50 | 255.255.255.0 | 192.168.1.1 |
| Lab-win | Windows 11 Pro | 192.168.1.51 | 255.255.255.0 | 192.168.1.1 |

### Konfiguration av maskinerna

På Linux-servern konstateras att nätverksenheten heter "enp0s3" med kommandot,  
`ip a`  
IP-adressen konfigureras sedan med kommandot,  
`sudo ip addr add 192.168.1.50/24 dev enop0s3`

<img src="images/Bash_IP_config.png" width="450" alt="Bash IP config">


För motsvarande i Windows behöver man först öppna Powershell som administratör, sedan sätts IP adressen med kommandot:  
```New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress 192.168.1.51 -PrefixLength 24 -DefaultGateway 192.168.1.1```

<img src="images/Powershell_IP_config.png" width="350" alt="Powershell IP config">



## Kommandoradsgenomförande

Här följer steg-för-steg dokumentation över de moment som ingick i uppgiften. 

### Linux, bash-kommandon

1. En arbetsmapp skapas med kommandot mkdir,  
`sudo mkdir -p /var/systementor/konsultdata`  
Därefter skapas en tom fil i mappen med kommandot touch,  
`sudo touch /var/systementor/konsultdata/anteckningar.txt`  
<img src="images/Bash_1_2_groupadd.png" width="450" alt="Bash groupadd">

1. I bilden syns även kommandot groupadd, som används för att skapa en användargrupp med namnet konsulter.  
`sudo groupadd konsulter`

1. Mappen konsultdata och allt i den ska tilldelas gruppen konsulter, för det används kommantot chown,  
`sudo chown -R root:konsulter /var/systementor/konsultdata`  
och sedan ställs behörigheter för mappen och filen in med kommandot chmod,  
`sudo chmod 750 /var/systementor/konsultdata` och  
`sudo chmod 640 /var/systementor/konsultdata/anteckningar.txt`
<img src="images/Bash_3_chmod.png" width="550" alt="Bash chmod">

1. Behörigheterna inspekteras därefter med kommandot ls -la,  
`sudo ls -la /var/systementor/konsultdata/`  
<img src="images/Bash_4_ls.png" width="350" alt="Bash ls">

1. Till sist verifieras nätverksanslutningen genom att skicka en ping till Windows-VM:en och undersöka nätverkskortets detaljer med kommandot ip sddr show,  
`ip addr show`och `ping -c 5 192.168.1.51`  
<img src="images/Bash_5_ping.png" width="550" alt="Bash ping">  


### Windows, powershell-kommandon

1. Först skapas mappen C:\Systementor\KonsultData med kommandot mkdir,  
`mkdir C:\Systementor\KonsultData`  
<img src="images/Powershell_1_mkdir.png" width="450" alt="Powershell mkdir">

1. Behörighetsstrukturen ska inspekteras med kommandot Get-Acl.  
I bilden ovan syns att kommandot ger ett svar som slutar med "...", d.v.s. det finns mer att visa.  
Med ett utökat kommando kan man se att rättigheter har ärvts vid skapandet av mappen och att t.ex. alla användare har "ReadAndExecute" rättigheter.  
`(Get-Acl).Access | Format-Table IdentityReference,FileSystemRights,AccessControlType`  
<img src="images/Powershell_2_get-acl.png" width="450" alt="Powershell get-acl">

1. Till sist verifieras nätverksanslutningen genom att skicka en ping till Linux-VM:en och undersöka nätverkskortets detaljer med kommandot ipconfig /all,  
<img src="images/Powershell_3_ping.png" width="450" alt="Powershell ping"><img src="images/Powershell_3_ipconfig.png" width="450" alt="Powershell ipconfig">  

### Felsökning

Det visade sig att Windows inbyggda brandvägg blockerade ping-anrop från Linux-maskinen. Frågar man AI om detta problem så föreslår den snabbt att man stänger av brandväggen (tillfälligt), men jag valde istället att öppna endast den regel som behövdes med kommandot,  
`Enable-NetFirewallrule -Name "FPS-ICMP4-ERQ-In"`  
<img src="images/Windows_firewall.png" width="450" alt="Windows firewall">



## Git och Versionshantering


## AI-logg och Utvärdering
*Prompt, AI-utdata och din kritiska granskning.*


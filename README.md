# brute-force-lab
Varje grupp kommer ha tillgång till en egen EC2 instans och en tillhörande IP-adress. 
Labbet går ut på att utföra en brute-force attack mot en gömd inloggningssida, och är uppdelad i tre steg:

1. Verifiera att instansen kör en webbserver
2. Hitta den dolda inloggningssidan
3. Hitta en användares inloggningsuppgifter genom att utföra en brute force-attack på inloggningssidan.


##nmap
Verifiera att instansen kör en webbserver genom att utföra en portskanning på instansen. 

Om man inte redan har nmap installerat kan man enkelt installera det genom:

```sh
sudo apt-get install nmap
```
för att utföra portskanningen, kör följande kommando:

```sh
nmap <IP>
```
Ersätt <IP> med din grupps ip-adress. Som standard utför Nmap endast tunga portskanningar, mot datorer som går att pinga. Jag har märkt att aws ibland inte svarar på ping paket, 

```sh
cd gobuster
gobuster dir --url http://<IP> -w common.txt
```

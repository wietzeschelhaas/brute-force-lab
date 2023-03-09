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
Ersätt <IP> med din grupps ip-adress. Som standard utför Nmap endast portskanningar, mot datorer som går att pinga. Jag har märkt att aws ibland inte svarar på ping paket, i så fall kan **-Pn** (no ping) flaggan användas

## gobuster
Hela instansen är egentligen en känd sårbar virtuell maskin som har använts vid CTF ("Capture the flag") tävlingar. Vi utnyttjar i den här labben dock bara en av flera sårbarheter som finns tillgängliga på maskinen. Det finns inget relevant att se på webbplatsens startsida.
  
Nu när vi vet att instansen hostar en webbserver så kan vi använda gobuster för att "brute forca" mappar och filer. 

Om man inte redan har nmap installerat kan man enkelt installera det genom:

```sh
sudo apt-get install gobuster
```

Utför följande kommandon för att starta gobuster.
```sh
cd gobuster
gobuster -u http://<IP> -w common.txt
```
Analysera resultatet och hitta inloggningssidan!
  
## hydra
När inloggningssidan har hitatts så är det dags att se om vi kan få tag på någons inloggningsuppgifter. Vi kan använda hydra för detta, men vi måste ange för hydra hur en innlogging sker, vi måste alltså ta reda på:
  * Vilken endpoint används inloggning?
  * Vilka parametrar skickas till endpointen?
  * Hur görs HTTP anropet, GET eller POST?
  
Om vi t.ex. har samlat in följande information:
  
Att hitta rätt anvädnarnamn borde inte ta mer än 3-4 minuter

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
för att utföra port skanningen, kör följande kommando:

```sh
nmap <IP>
```
Ersätt <IP> med din grupps ip-adress. Som standard utför Nmap endast port skanningar, mot datorer som går att pinga. Jag har märkt att aws ibland inte svarar på ping paket, i så fall kan **-Pn** (no ping) flaggan användas

## gobuster
Hela instansen är egentligen en känd sårbar virtuell maskin som har använts vid CTF ("Capture the flag") tävlingar. Vi utnyttjar i den här labben dock bara en av flera sårbarheter som finns tillgängliga på maskinen. Det finns inget relevant att se på webbplatsens startsida.
  
Nu när vi vet att instansen hostar en webbserver så kan vi använda gobuster för att "brute forca" mappar och filer. 

Om man inte redan har gobuster installerat kan man enkelt installera det genom:

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
När inloggningssidan har hittats så är det dags att se om vi kan få tag på någons inloggningsuppgifter. Vi kan använda hydra för detta, men vi måste ange för hydra hur en inloggning sker, vi måste alltså ta reda på:
  * Vilken endpoint används inloggning?
  * Vilka parametrar skickas till endpointen?
  * Hur görs HTTP anropet, GET eller POST?
  * Vad händer när ett inloggningsförsök misslyckas?
  
Om vi t.ex. har samlat in följande information:
  * Endpoint : exempel.com/login.php
  * Parametrar : username och password
  * HTTP metod : POST
  * Texten "Invalid username or password." dyker upp i HTTP responsen.

Då kan vi konstruera följande hydra kommando:
```sh  
sudo hydra -L usernames.txt -P password.txt exempel.com http-post-form "/login.php:username=^USER^&password=^PASS^:Invalid username or password."
```
### Förklarning av kommandot
Först måste vi ange vilka användarnamn och lösenord hydra ska använda i attacken. **-L** används för att ange en lista med användarnamn, **-P** används för att ange en lista med lösenord. Hade vi istället angett **-l username** så kommer endast användaren "username" att användas vid inloggning försöken. På samma sätt, hade vi angett **-p password** så kommer endast lösenordet "password" att användas vid inloggning försöken.  
  
 Efter det måste vi ange en url, i det här exemplet är det **exempel.com**.
  
 Sen måste vi ange HTTP metoden, i det här exemplet är det POST så då anger man **http-post-form**. Hade det istället varit GET så hade man skrivit **http-get-form**.
  
Nu måste man en sträng som innehåller resterande information.
  
Först i strängen så skriver man endpointen, här alltså **login.php** följt av ett **:** 
Sen anger man parametrarna som hittades, alltså: **username=^USER^&password=^PASS^**, när hydra körs sen så kommer **^USER^** och **^PASS^** ersättas med dom användarnamnen och lösenorden som man angett, här spelar det ingen roll om man använt **-L**, **-l**, **-P** eller **-P**. 
  
Till sist så måste man ange för hydra vad som händer vid misslyckade inloggningsförsök, vi anger texten **"Invalid username or password."**, då vet hydra att om den texten INTE förekommer i HTTP-responsen så betyder det att hydra har lyckats hitta en korrekt inloggningsuppgift!

### Uppgiften
Er uppgift är att nu hitta informationen som nämns ovan, så ni kan kontruera hydra kommandot på samma sätt. Listan med användarnamnen och listan med lösenorden finner man under hydra katalogen. Det finns en användarnamn/lösenord kombination som är korrekt. 

usernames.txt innehåller ca 2000 användarnamn och passwords.txt innehåller ca 1000 lösenord. Om man försöker med alla  användarnamn/lösenord kombinationer så skulle det ta ca 30 timmar för hydra att lista ut rätt kombination. Tricket här är att först ta reda på ett korrekt användarnamn, sen använda det för att ta reda på lösenordet.

Har ni gjort allt korrekt så bör hydra inte ta mer än 3-4 minuter att köras.

Om man inte redan har hydra installerat kan man enkelt installera det genom:

```sh
sudo apt-get install hydra
```

Lycka till!





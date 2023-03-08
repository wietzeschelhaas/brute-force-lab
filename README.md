# brute-force-lab
Varje grupp kommer ha tillgång till en egen EC2 instans IP-adress som ledar till en egen 
Labbet går ut på att utföra en brute-force attack på en gömd inloggningssida.
Labbet är uppdelad i tre steg:

1. Bekräfta att 

```sh
cd gobuster
gobuster dir --url http://<IP> -w common.txt
```

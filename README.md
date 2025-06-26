# AUTOMATIZAREA PROCESULUI DE CONFIGURARE A ECHIPAMENTELOR DE REȚEA

## Proiect Licență - Cojiță Filip-Iosia - UPT 2025

## Descriere generală

Acest proiect automatizează procesul de configurare a unei topologii GNS3, utilizând scripturi Python și framework-ul PyATS. Soluția suportă mai multe tipuri de echipamente:

- Routere Cisco IOU, IOSv, IOS-XE (CSR1000v)
- Dispozitive de tip firewall Cisco Firepower Threat Defense (FTD)
- Dispozitive Ubuntu 24.10 (server, dockerguest, dockerdesktop)
- Server DNS

Configurarea se face în două etape:

- **1. Prin Telnet** – configurare inițială (IP, hostname, rută default, activare SSH)
- **2. Prin SSH** – configurare avansată (interfețe, rutare statică sau OSPF, DHCP)

Sau, pentru FTD:
- **2. Prin REST API (Swagger)** – configurare avansată pentru FTD (zone, reguli de acces, obiecte de rețea, rute)

Framework-ul PyATS este folosit pentru definirea topologiei și orchestrarea pașilor de execuție.

## Structura proiectului:

```
.
├── main_autofill.py             # Script principal – configurare completă
├── main_1dev.py                 # Configurare individuală a unui dispozitiv
├── autofill_engine.py           # Completează automat atribute lipsă în testbed.yaml
├── telnet_connector2.py         # Conectare și configurare Telnet
├── ssh_connector_paramiko.py    # Conectare și configurare SSH
├── swagger_connector.py         # Interacțiune Swagger REST API pentru FTD
├── ubuntu_setup.py              # Configurare locală a serverelor Ubuntu
├── verify_ubuntu_ping.py        # Verificare conectivitate IP în rețea
├── configure_fdm_via_rest.py    # Configurare completă FTD prin Swagger
├── mocktests.py                 # Teste unitare pentru componentele principale
├── mytopo.yaml                  # Fișier de definire a topologiei rețelei
├── link_proiect_licenta_gns3    # Link download proiect portabil GNS3
```

## Cerințe preliminare:

- Python 3.10+  
- GNS3 cu următoarele imagini:
  - Cisco IOSv
  - Cisco CSR1000v
  - Cisco FTD
  - Ubuntu/DNS (mașini virtuale sau containere)
  
Un proiect GNS3 portabil este atașat în acest sens

- Acces la terminal Bash / PowerShell
- Conectivitate între sistemul local și nodurile GNS3

## Instalare

1. Clonează proiectul sau descarcă fișierele sursă. (nu uita de proiectul GNS3)

2. Creează și activează un mediu virtual:

```bash
python3 -m venv venv
source venv/bin/activate    # sau venv\Scripts\activate pe Windows
```

3. Instalează bibliotecile necesare:

```bash
pip install pyats paramiko requests bravado
```

## Configurare

Asigură-te că fișierul `mytopo.yaml` reflectă topologia reală GNS3 și include:

- OS: `ios`, `iosxe`, `ftd`, `linux`
- Interfețe: cu IP, subnet, alias (`initial`)
- Conexiuni: `telnet`, `ssh`, `rest` (cu IP și port)
- Atribute `custom`: `hostname`, `gateway`, `static_routes`, `dhcp`, `ip_helper`, etc.

## Rulare aplicație

### Configurare completă a topologiei

Întâi verifică că proiectul GNS3 este funcțional
- dispozitivele sunt deschise fără erori, și sunt gata să se aplice config pe ele
- verifică dacă porturile din GNS3 corespund cu cele din topologie!
- verifică că rulezi scriptul Python pe serverul ubuntu remote.

Apoi, se poate începe executarea scripturilor 
```bash
python3 main_autofill.py
```
### Pas intermediar config FTD - vezi documentație sau video

### Verificare ping între toate dispozitivele

```bash
python3 verify_ubuntu_ping.py
```

## Testare unitară

```bash
python3 -m unittest mocktests.py
```

## Autor

**Filip-Iosia Cojiță**  
Lucrare de licență – Universitatea Politehnica Timișoara  
Sesiunea: Iunie 2025

Coordonator: Conf. dr. ing. Octavian Ștefan

# Home-Assistant-Backup auf den vServer

Samba-Freigabe `habackup` auf dem vServer, nur ueber das VPN erreichbar (10.8.0.1, tcp/445 nur auf wg0).
Home Assistant OS bindet sie als Netzwerkspeicher ein und legt dort seine Backups ab.

## Voraussetzung
Home Assistant muss 10.8.0.1 erreichen. Der Pi (10.8.0.5) ist das Gateway ins VPN, daher in der
FRITZ!Box eine statische Route anlegen: Netz 10.8.0.0/24 -> Gateway <LAN-IP des Pi>
(Heimnetz > Netzwerk > Netzwerkeinstellungen > Statische Routingtabelle).
Test im Terminal-Add-on von Home Assistant: `ping 10.8.0.1`

## vServer
    ansible-playbook -i ../wireguard/inventory.ini backup.yml

## Home Assistant
1. Einstellungen > System > Speicher > Netzwerkspeicher hinzufuegen
   - Name: vserver, Verwendung: Backup, Protokoll: Samba/Windows (CIFS)
   - Server: 10.8.0.1, Freigabe: habackup, Benutzer: habackup, Passwort: (aus dem Playbook)
2. Einstellungen > System > Backups > Automatische Backups konfigurieren
   - Speicherort "vserver" waehlen, Zeitplan (z.B. taeglich) und Anzahl (z.B. 7) festlegen
3. Verschluesselungsschluessel (Notfall-Kit) herunterladen und NICHT nur auf dem vServer ablegen.
   Ohne diesen Schluessel ist kein Backup wiederherstellbar.

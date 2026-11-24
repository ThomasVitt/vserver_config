# Home-Assistant-Backup per SFTP auf den vServer

Integration "SFTP Storage" in Home Assistant legt Backups auf dem vServer ab.
Benutzer `habackup`: nur SFTP, Chroot nach /srv/backup/homeassistant, nur Schluessel-Login,
Schluessel gilt nur aus 10.8.0.0/24 und 192.168.170.0/24.

## Voraussetzung
Home Assistant muss 10.8.0.1 erreichen. Der Pi (10.8.0.5) ist das Gateway ins VPN, daher in der
FRITZ!Box eine statische Route: Netz 10.8.0.0/24 -> Gateway <LAN-IP des Pi>.
Test im Terminal-Add-on von Home Assistant: `ping 10.8.0.1`

## vServer
    ansible-playbook -i ../wireguard/inventory.ini backup.yml

Das Playbook erzeugt /root/ha-backup-key/id_ed25519 (ohne Passphrase, das verlangt HA).
Datei auf den Rechner holen, in HA hochladen, danach auf dem vServer loeschen:
    scp root@vpn.tvitt.de:/root/ha-backup-key/id_ed25519 .
    ssh root@vpn.tvitt.de 'rm /root/ha-backup-key/id_ed25519'

## Home Assistant
1. Einstellungen > Geraete & Dienste > Integration hinzufuegen > "SFTP Storage"
   Host 10.8.0.1, Port 22, Benutzer habackup, Private-Key-Datei hochladen, Verzeichnis /backups
2. Einstellungen > System > Backups: Speicherort "SFTP Storage" fuer automatische Backups waehlen,
   Zeitplan (z.B. taeglich) und Anzahl (z.B. 7) festlegen.
3. Verschluesselungsschluessel (Notfall-Kit) herunterladen und NICHT nur auf dem vServer ablegen.

## Test vom Rechner im VPN
    sftp -i id_ed25519 habackup@10.8.0.1    # klappt, landet in "/", schreiben nur in /backups
    ssh  -i id_ed25519 habackup@10.8.0.1    # wird abgewiesen (nur SFTP)

# WireGuard: MacBook/iPhone -> vServer -> FRITZ!Box 5G -> Home Assistant

VPN-Netz 10.8.0.0/24: vServer 10.8.0.1, MacBook 10.8.0.2, iPhone 10.8.0.3, FRITZ!Box 10.8.0.4.
Heimnetz im Stall: 192.168.178.0/24 (Home Assistant: http://<HA-IP>:8123).
Die FRITZ!Box baut den Tunnel nach aussen auf (kein Port-Forwarding, CGNAT-tauglich).

## Schritte
1. Schluessel erzeugen (Client): `umask 077; wg genkey | tee privatekey | wg pubkey > publickey`
2. `wg_peers` in `wireguard.yml` pflegen; fuer die FRITZ!Box `allowed_ips: 10.8.0.4/32, 192.168.178.0/24`.
3. Playbook ausfuehren (auf dem vServer): `ansible-playbook -i inventory.ini wireguard.yml`
4. `clients/*.conf.example` kopieren, Platzhalter ersetzen:
   - FRITZ!Box: Datei in der Box importieren (Internet > Freigaben > VPN (WireGuard))
   - MacBook/iPhone: in der WireGuard-App als Tunnel eintragen.
5. Test: Tunnel am MacBook aktivieren, `ping 10.8.0.4`, `ping <HA-IP>`, dann http://<HA-IP>:8123

Private Schluessel nie ins Repo committen.

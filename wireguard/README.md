# WireGuard: MacBook -> vServer -> Home Assistant

Netz: vServer 10.8.0.1, MacBook 10.8.0.2, iPhone 10.8.0.3 (VPN 10.8.0.0/24).
Home Assistant: http://10.8.0.3:8123 (Pi baut die Verbindung nach aussen auf, kein Port-Forwarding noetig).

## Schritte
1. Schluessel auf Pi und MacBook erzeugen (je Geraet):
       umask 077; wg genkey | tee privatekey | wg pubkey > publickey
2. `peers.example.yml` nach `peers.yml` kopieren, Public Keys eintragen.
3. Playbook ausfuehren: `ansible-playbook -i inventory.ini wireguard.yml -e @peers.yml`
   Am Ende wird der Server Public Key angezeigt.
4. `clients/*.conf.example` kopieren, Platzhalter ersetzen:
   - Pi: nach /etc/wireguard/wg0.conf, dann `sudo systemctl enable --now wg-quick@wg0`
   - MacBook: in der WireGuard-App als Tunnel importieren.
5. Test: Tunnel am MacBook aktivieren, `ping 10.8.0.3`, dann http://10.8.0.3:8123 .

Private Schluessel nie ins Repo committen.

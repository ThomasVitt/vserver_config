# vserver_config

Ansible-Konfiguration fuer den vServer `vpn.tvitt.de` (Ubuntu 24.04).

```
ansible.cfg              # zeigt auf inventory.ini und roles/
inventory.ini            # Host vpn.tvitt.de in Gruppe "vserver" (laeuft lokal auf dem vServer)
group_vars/vserver.yml   # eigene Werte, z.B. WireGuard-Peers
requirements.yml         # benoetigte Collections
site.yml                 # alle Playbooks
wireguard.yml            # WireGuard-Server + iptables
homeassistant_backup.yml # SFTP-Backup-Ziel fuer Home Assistant
roles/
  wireguard/             # tasks, handlers, templates, defaults
  homeassistant_backup/  # tasks, handlers, defaults
docs/                    # Anleitungen und Client-Beispielkonfigurationen
```

## Benutzung (im Repo-Root auf dem vServer)

    ansible-galaxy collection install -r requirements.yml
    ansible-playbook site.yml                  # alles
    ansible-playbook wireguard.yml             # nur WireGuard
    ansible-playbook homeassistant_backup.yml  # nur Backup-Ziel

Anleitungen: [docs/wireguard.md](docs/wireguard.md), [docs/homeassistant-backup.md](docs/homeassistant-backup.md)

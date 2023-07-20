# Infra

An Ansible playbook that sets up an homeserver with:
  - Create not root user
  - SSH secure settings
  - Samba settings
  - SnapRAID confuration
  - Docker with some containers:
    - Traefik - reverse proxy with DuckDNS for local network and auto ssl cetificates
    - Authelia - authentication middleware to protect naked ui sevices
    - Portainer - docker containers manager
    - Homer - simple page with links to all services
    - Jellyfin - video streaming to any device from your server
    - Navidrome - music listening and organisations tool
    - Filebrowser - web ui for all files
    - Deluge - torrent client with web ui
    - Photoprism - photo/video organisations tool with sync app from phone and AI under the hood
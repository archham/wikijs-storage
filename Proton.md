---
title: Proton
description: Infos about Proton.me services
published: true
date: 2026-04-05T09:04:52.337Z
tags: docker, proton, mail
editor: markdown
dateCreated: 2026-04-05T08:42:00.076Z
---

# Proton Mail Bridge
Used to handle applications sending mail, because of enryption of Proton Mail.

Code: https://github.com/dancwilliams/protonmail-bridge-docker

## Docker
We will configure this with only SMTP and no IMAP so we get more security. Only sending is allowed

**Big picture**
``Application (Docker) → SMTP → Proton Bridge (Docker) → Proton Mail``

### Step-by-step setup
```bash
# Clone project & copy config
cd /opt && sudo git clone https://github.com/dancwilliams/protonmail-bridge-docker.git
sudo mkdir prod-protonmail-bridge-docker && sudo cp protonmail-bridge-docker/docker-compose.yml prod-protonmail-bridge-docker/

vi prod-protonmail-bridge-docker/docker-compose.yml
```
Edit ``docker-compose.yml``
```YAML
services:
  protonmail-bridge:
    image: dancwilliams/protonmail-bridge:latest
    ports:
      - 1025:25/tcp # SMTP only (no IMAP) 
      # - 1143:143/tcp
    restart: unless-stopped
    volumes:
      - protonmail:/root

volumes:
  protonmail:
    name: protonmail

```
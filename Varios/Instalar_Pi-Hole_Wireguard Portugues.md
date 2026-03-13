# VPN con Bloqueo de Publicidade
Para conectarnos a casa de forma segura e navegar sen anuncios, usaremos WireGuard (WG-Easy), Pi-hole e DuckDNS.

## 1.1 Código de Instalación (Compose)
Iremos a Servizos - Compose - Arquivos e crearemos un arquivo chamado vpn-pihole. Pegamos este código, que xa inclúe a automatización para que a túa VPN funcione sempre:

```YAML

services:
  duckdns:
    image: lscr.io/linuxserver/duckdns:latest
    container_name: duckdns
    environment:
      - SUBDOMAINS=anxovc
      - TOKEN=0ae68c74-3957-4fbb-929c-bc66815ed9c0
      - TZ=Europe/Madrid
    volumes:
      - ${PATH_TO_APPDATA}/config/duckdns:/config
    restart: unless-stopped

  wg-easy:
    image: ghcr.io/wg-easy/wg-easy
    container_name: wg-easy
    environment:
      - WG_HOST=anxovc.duckdns.org
      # Hash para "mi_contraseña"
      - PASSWORD_HASH=$$2a$$12$$X6vH/xP/Lp.v7X6z8rG7fO7L8Xh6F9r6v8uP6j5n8w6x8y6z7A9B.
      - WG_DEFAULT_DNS=10.8.1.3
      - WG_DEFAULT_ADDRESS=10.8.0.x
    volumes:
      - ${PATH_TO_APPDATA}/etc/wireguard
    ports:
      - "51820:51820/udp"
      - "51821:51821/tcp"
    restart: unless-stopped
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    sysctls:
      - net.ipv4.ip_forward=1
      - net.ipv4.conf.all.src_valid_mark=1
    networks:
      wg-easy:
        ipv4_address: 10.8.1.2

  pihole:
    image: pihole/pihole
    container_name: pihole
    environment:
      - WEBPASSWORD=mi_contraseña
    volumes:
      - ${PATH_TO_APPDATA}/etc/pihole
      - ${PATH_TO_APPDATA}/etc/dnsmasq.d
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "5353:80/tcp"
    restart: unless-stopped
    networks:
      wg-easy:
        ipv4_address: 10.8.1.3

networks:
  wg-easy:
    ipam:
      config:
        - subnet: 10.8.1.0/24
```
## 1.2 Configuración e Probas
Unha vez que os contedores estean funcionando (en verde), segue estes pasos:

* Conectar o Móbil (WireGuard):
  * Entra no panel web de WireGuard en: http://naspi:51821.
  * Crea un novo cliente premendo en "+ New Client" e ponlle o teu nome.
  * Pulsa na icona do Código QR.
  * Abre a app oficial de WireGuard no teu móbil e elixe "Escanear código QR".
  * Ponlle un nome (ex: "Casa VPN") e activa o interruptor.
* Entrar ao Panel de Control (Pi-hole):
  * Abre o navegador e entra en: http://naspi:5353/admin.
  * Usa o teu contrasinal: mi_contraseña.
  * Moi importante: Vai a Settings > DNS, baixa ata "Interface settings" e marca "Permit all origins". Sen isto, a VPN non terá internet.
* Comandos Útiles:
  * Xerar Hash de WireGuard: sudo docker run --rm ghcr.io/wg-easy/wg-easy wgpw **mi_contraseña**.
  * Resetear contrasinal Pi-hole: sudo docker exec -it pihole 

* **Vídeo de Referencia:** [Como instalar WireGuard y Pi hole usando DuckDNS](https://youtu.be/u9VKhBioclI?si=tUw-jEPZ2uiCrDJS).
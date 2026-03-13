# VPN com Bloqueio de Publicidade
Para nos conectarmos a casa de forma segura e navegar sem anúncios, usaremos WireGuard (WG-Easy), Pi-hole e DuckDNS.

## 1.1 Código de Instalação (Compose)
Iremos a Serviços - Compose - Arquivos e criaremos um arquivo chamado vpn-pihole. Colamos este código, que já inclui a automatização para que a sua VPN funcione sempre:

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
## 1.2 Configuração e Testes
Uma vez que os contêineres estejam a funcionar (em verde), siga estes passos:

* Conectar o Telemóvel (WireGuard):
  * Entre no painel web do WireGuard em: http://naspi:51821.
  * Crie um novo cliente clicando em "+ New Client" e dê-lhe o seu nome.
  * Clique no ícone do Código QR.
  * Abra a app oficial do WireGuard no seu telemóvel e escolha "Escanear código QR".
  * Dê-lhe um nome (ex: "Casa VPN") e ative o interruptor.
* Entrar no Painel de Controlo (Pi-hole):
  * Abra o navegador e entre em: http://naspi:5353/admin.
  * Use a sua palavra-passe: mi_contraseña.
  * Muito importante: Vá a Settings > DNS, desça até "Interface settings" e marque "Permit all origins". Sem isto, a VPN não terá internet.
* Comandos Úteis:
  * Gerar Hash do WireGuard: sudo docker run --rm ghcr.io/wg-easy/wg-easy wgpw **mi_contraseña**.
  * Redefinir palavra-passe Pi-hole: sudo docker exec -it pihole 

* **Vídeo de Referência:** [Como instalar WireGuard y Pi hole usando DuckDNS](https://youtu.be/u9VKhBioclI?si=tUw-jEPZ2uiCrDJS).
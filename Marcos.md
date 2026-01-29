# Proxecto de Fin de Curso Anxo Vigo Canosa: Servidor NAS con Raspberry Pi 5


## 1. Introdución

Este proxecto ten como obxectivo a creación dun servidor NAS completamente funcional empregando unha Raspberry Pi 5. A idea nace da necesidade de dispor dun sistema de almacenamento en rede accesible, eficiente e económico. Este traballo combina hardware e software é permite comprender de maneira práctica o funcionamento dun servidor real nun entorno doméstico.


## 2. Xustificación do proxecto

A motivacion principal para facer este proxecto, e que xa desde fai un tempo queria facer o meu propio servidor. Enton foi cando dixen, pois este vai ser o meu proxecto, ademais de evitar pagar un servicio de almacenamiento na nube como Google Drive e Almacenar os meus datos nun sitio mais seguros, no meu propio servidor privado.


## 3. Obxectivos

Os obexectiovs de este projecto son: 

* Crear un servidor NAS funcional empregando unha Raspberry Pi 5.
* Configurar un sistema de almacenamento en rede seguro e accesible Utilizando NextClaud.
* Instalar unha VPN, para poder acceder ao Nas desde calquera lugar.
* Aprender conceptos de administración de sistemas, redes e hardware.
* Poñer en práctica coñecementos adquiridos durante o ciclo formativo.

## 4. Hardware empregado

A continuacion vou detallar o hardware que utilizei para o meu proxecto.

### 4.1 Raspberry Pi 5 

Esta Raspberry Pi 5 8GB é o núcleo do proxecto. As súas características principais inclúen:

* Procesador ARM Cortex-A76
* Portas USB 3.0 de alta velocidade
* Melloras significativas respecto a xeracións anteriores
* Compatibilidade con almacenamento externo M.2 mediante adaptador

### 4.2  SanDisk Ultra 

Tarjeta microsd de 128GB con una velocidad de 150 MB/s

Aqui e onde vou a instalar o sistema operativo para a Rasberry Pi 5

### 4.3 Discos de almacenamento Utilizados 

Para o meu Proxecto mentres non consigo discos de gran capacidade vou a reutilizar discos que teño na casa.

O almacenamento que tou usando:

* HDD 2.5" de 250GB a 5400rpm da marca Samsung 
* HDD 2.5" de 500GB a 5400rpm da marca Samsung
* HDD 2.5" de 320GB a 5400rpm da marca WDigital
* SSD 2.5" de 240GB da marca Kingston
  
Con esto vou a armar un **RAID 5**. O que me permite a tolenrancia de fallos nun so disco, e un espacio util total de **658GB**. 


### 4.4 Radxa Penta Sata Hat Para Rasberry Pi 5 

Esto e unha tarjeta de expansion para conectarse coa rasberry para poder conertar discos, soporta hasta 5 discos tanto HDD/SSD de 2.5" e discos HDD de 3.5".

* [Radxa Penta-Sata-Hat](https://radxa.com/products/accessories/penta-sata-hat)
* [Fonte de alimentación 12V 3A 5525](https://radxa.com/products/accessories/power-dc12-36w)


## 5. Instalación do sistema operativo

Optei por instalar **OpenMediaVault** na MicroSD.
Pasos principais:

1. Descargar Raspberry Pi Imager.
2. Seleccionar Raspberry Pi OS lite.
3. Configurar usuario.
4. Configurar a wifi si fora necesario.
5. Acivar o SSH.
6. Gravar o sistema na tarxeta SD.
7. Arrancar a Raspberry Pi desde a SD.

## 6. Configuración do NAS

Unha vez instalado o sistema operativo na Sd con `rasberry pi imager`, insertaremos a SD na rasberry e encenteremola, e pasado uns minutos, imos proceder a conectarnos a ela, mediante ssh co seginte comando desde a nosa terminal.
```
ssh usuario@direccion ip / nombre do dispositivo
```
Para averiguar a direccion ip, poderemos acceder ao noso router e nos mostrara a direccion ip do noso dispositivo.

No seguinte paso imos actualizar o sistema co seguinte comando `sudo apt update && sudo apt upgrade -y` , asi teremos o sistema actualizado, e procederemos a instalar `OpenMediaVault` co seguinte comando, que e uns script de instalacion.
```
wget -O - https://raw.githubusercontent.com/OpenMediaVault-Plugin-Developers/installScript/master/install | sudo bash
```
Este proceso vai tardar uns minutos, cando termine teremos que meter a IP da nosa RasberryPI no navegador web, e tera que mostrar unha pagina asi:

![alt text](Imagenes/omv_login.png)

O usuario predefinido e **admin** e a contraseña **openmediavault**, que procederemos a cambiar tan pronto entremos a paxina inicial, clicando no engranaje arriba a dereita

## 6.1 Configuración de rede

Realizouse unha configuración básica para garantir estabilidade no acceso:

* Asignación de IP fixa para o NAS
* Activación de SSH para administración remota
* Comprobación de conectividade na rede local
* Imos crear un certificado SSl
* Habilitar o HTTPS

### 6.1.1 Certificado SSL e HTTPS
Para mellorar a seguridade e que o navegador non se queixe tanto, imos crear un certificado SSL propio e activar o modo seguro.

Para crealo imos a Sistema - Certificados - SSL, seleccionamos o botón de Máis (+) e dámoslle a Crear.

Na ventá que se abre, o máis importante é o **Nombre Propio**, onde poñeremos a IP do servidor ou o noso dominio, e ten que quedar asi:

![alt text](Imagenes/ssl.png)

Clicaremos en Salvar.

Agora para activalo, imos a Sistema - Área de trabajo, buscamos o apartado de **Conexión Segura**.

Temos que activar o interruptor de SSL/TLS e en **Certificado** seleccionamos o que acabamos de crear no paso anterior.

Ten que quedar asi:

![alt text](Imagenes/https.png)

Clicaremos en Salvar e arriba aparecerá a barra amarela de configuración, Confirmaremos os cambios.

Ao finalizar, o navegador recargarase e perderemos a conexión. Agora teremos que entrar poñendo https:// diante da IP:
```
https://192.168.1.128
```
### 6.2 Como Crear un Raid 

Para instalar o Plugin para permitir facer Raids, debemos ir a barra lateral buscar **Sistema** e entrar no apartado **Plugins**, xa dentro debemos buscar **Raid** clicar e instalar clicando no icono de Descarga, xusto na parte superior.

Debemos acutalizar a paxina premendo F5 e ir a **Almacenamiento**, debemos ir a discos e localirzar os que vas a usar, e debemos premer no icono da goma de borrar para limpar os discos que formaran parte do Raid.

Con esto xa feito imos ir a Raid, e clicar en crear, no meu caso vou crear un **Raid 5** e logo seleccionaremos os discos que imos a utilizar, e comfirmaremos os cambios en OpenMediaVault, este proceso tardara un rato.

Ahora imos Formatear e montar o disco, e imos clicar en Sistema de archivos, clicar en crear e selecionaremos o tipo de sistema de ficheiro que queremos, eu utilizare **EXT4** e logo selccionaremos o disco e clicaremos en salvar, e xa pasara a opcion de montar, onde seleccionaremos o sistema e o umbral de uso e xa estaria todo listo para clicar en salvar e ter o noso Raid en funcionamento 


## 7. Servizos instalados

* Docker
* NextCloud
* TailScale

### 7.1 Docker

Instalar docker permite empaquetar aplicaciones en en unidades chamadas contenedores, esto e necesario para instalar o servicio de nextcloud entre outros.
Para instalalo necesitaremos, ir a barra lateral buscar o apartado **Plugins** e xa dentro de ese apartado buscar **OMV-Extras** e **Compose**
Con paquete OMV-Extras xa instalado necesitaremos confirmar o cambio e recargar a paxina, e ir a  **Sistema** localizado na barra lateral e clicar en omv-extras, activar Docker repo e clicar en Salvar.

A continuacion imos crear as seguintes carpetas no apartado de **almacenamiento** - **Carpetas Compartidas**:

* appdata
* data
* docker
* backup_compose

Xa feito esto imos ir a **Servicios** - **Compouse** - **Configuracion** e teremos que meter o seguinte:

![alt text](Imagenes/compouse.png)

Con esto listo damoslle a salvar e daremoslle a reinstalar docker.

Por ultimo crearemos o usuario **appdata** cos seguientes permisos.

![alt text](Imagenes/USRappdata.png)

E daremoslle acceso de **lectura/escritura** en **appdata** e de **lectura** en data, ademais de en Control de Acceso so permitirlle a lectura.

#### 7.1.1 Variables de entorno

Imos crear unha variables de entorno que nos facilitaran o traballo ao instalar contenedores con compouse, para esto temos que escribir en **compouse** dentro de **archivos** clicaremos no icono cunha estrellita, e escribiremos o seguinte.

```
APPUSER_PUID=1002
APPUSER_PGID=100

TIME_ZONE_VAULE=Europe/Madrid

PATH_TO_APPDATA=/srv/dev-disk-by-uuid-dee6e7ad-abb0-483d-93f4-3052adb442d4/appdata/
PATH_TO_DATA=/srv/dev-disk-by-uuid-dee6e7ad-abb0-483d-93f4-3052adb442d4/data/
```


### 7.2 NextCloud 

Para instalar NextCloud, necesitaremos unha base de datos, por eso instalaremos **Mysql** e **PHPMyAdmin**.

Para instalalos imos a utilizar o seguinte codigo:

```
services:
  db:
    image: lscr.io/linuxserver/mariadb:latest
    container_name: db
    environment:
      - PUID=${APPUSER_PUID}
      - PGID=${APPUSER_PGID}
      - TZ=${TIME_ZONE_VALUE}
      - MYSQL_ROOT_PASSWORD=${PASSWORD_MYSQL_ROOT}
    volumes:
      - ${PATH_TO_APPDATA}/mysql/database:/config
    ports:
      - "3306:3306"
    restart: always

  phpmyadmin:
    image: lscr.io/linuxserver/phpmyadmin:latest
    container_name: phpmyadmin
    environment:
      - PUID=${APPUSER_PUID}
      - PGID=${APPUSER_PGID}
      - TZ=${TIME_ZONE_VALUE}
      - PMA_ARBITRARY=1
      - PMA_HOST=db
    volumes:
      - ${PATH_TO_APPDATA}/phpmyadmin/config:/config
    ports:
      - 8081:80
    restart: unless-stopped
    depends_on:
      - db

```

Con esto copiado vamos a Servicios - Compouse - Archivos, seleccionamos Mas e añadir, e ten que quedar asi.
![alt text](Imagenes/PHP-Mysql.png)

Clicaremos en  salvar e Volveremos a o apartado de variables de entorno para añadir: `PASSWORD_MYSQL_ROOT=(contraseña)`, salvaremos o archivo e comfirmaremos os cambios, por ultimo clicaremos sobre o archivo e daremoslle a up o icono ca flecha para arriba.

Ao finalizar en Comouse podemos ir a servicios e comprobar que estan funcionando.

Para acceder a phpmyadmimin teremos que colocar o siguiente no navegador `http://192.168.1.128:8081` e colocaremos o usuario e contraseña
![alt text](Imagenes/phpmyadmin.png)

Dentro de PhpMyAdmin vamos crear un usuario ca seguinte configuracion:

![alt text](Imagenes/UsrNexcloud.png)

E clicamos en continuar, quedando asi:

* Nonbre-Base de datos = nextcloud
* Nonbre-User = nextcloud

Para finalizar imos instalar Nextcloud, clicando en Compouse-Archivos-Crear-Crear desde los ejemplo.

E seleccionaremos Nextcloud,clicaremos no cuadrado de mais, meteremos unha descripcion e daremoslle a engadir,confirmaremos os cambios, e editaremos o archivo, e ten que quedar asi:

```
 nextcloud:
    image: lscr.io/linuxserver/nextcloud:latest
    container_name: nextcloud
    environment:
      - PUID=${APPUSER_PUID}
      - PGID=${APPUSER_PGID}
      - TZ=${TIME_ZONE_VALUE}
    volumes:
      - ${PATH_TO_APPDATA}/nextcloud/config:/config
      - ${PATH_TO_DATA}/nextcloud/data:/data
    ports:
      - 4443:443
    restart: unless-stopped
    depends_on:
      - db

```
O copiaremos e o meteremos co contenedor de Mysql.

Xa feito esto, no navegador poñeremos `https://ip`, e meteremos as seguintes configuracions:

![alt text](Imagenes/nextcloud.png)

### 7.3 Tailscale

Para poder acceder ao noso servidor desde fóra da casa de forma segura sen abrir portos, necesitaremos unha VPN, por eso instalaremos Tailscale.

Se non queremos usar Docker, podemos instalar Tailscale directamente no servidor. Esto é útil para ter sempre acceso ao sistema, incluso se Docker falla.

Para instalalo, primeiro teremos que abrir a terminal ou conectarnos por SSH ao servidor.

Imos a utilizar o seguinte comando para descargar e instalar o script oficial:

```
curl -fsSL https://tailscale.com/install.sh | sh
```
Unha vez remate a instalación (tarda uns segundos), teremos que inicialo. Para iso executamos:
```
sudo tailscale up
```
Ao darlle a enter, a terminal devolveranos unha dirección web (un link) parecida a esta:
```
To authenticate, visit:
https://login.tailscale.com/a/1234567890
```
Copiaremos ese link e pegarémolo no noso navegador do PC ou móbil. Aí teremos que loguearnos coa nosa conta (Google, Microsoft, etc.) para autorizar o dispositivo.

Unha vez feito, na terminal aparecerá "Success".

Agora podemos ir á web de Tailscale (Admin Console) e veremos o noso servidor conectado e coa IP asignada.


## 8. Seguridade

A seguridade reforzouse mediante:

* Permisos axeitados en carpetas
* Limitación do acceso remoto: deshabilitaremos o ssh 
* Creouse un certificado SSL propio e activouse o HTTPS
* Actualizacións periódicas do sistema

## 9. Probas de funcionamento

Realizáronse diversas probas:

* Acceso ás carpetas compartidas dende outro dispositivo
* Funcionamento correcto do servidor DNS
* Monitorización do rendemento da Raspberry Pi

## 10. Problemas encontrados e solucións

Durante o desenvolvemento apareceron algunhas dificultades:

## 10.1 A tarjeda de expansion non detectaba os discos
 Solucionado: modificar archivo, co comando `sudo nano /boot/fimware/config.txt` e engadimos na ultima linea do archivo:
```
[all] 
dtparam=pciex1=on 
dtparam=pciex1_gen=3
``` 

## 10.2 Solución Erro: Dominio non confiable en NextCloud
Se ao intentar entrar en Nextcloud dende unha IP de Tailscale aparece unha pantalla azul coa mensaxe "Acceso a través de un dominio en el que no se confía", é porque Nextcloud ten unha lista de seguridade e bloquea todo o que non estea nela.

Para autorizar a nova IP, temos que editar o archivo config.php. Como é un arquivo protexido, non podemos facelo cun usuario normal, así que seguiremos estes pasos na terminal.

Primeiro, temos que converternos en usuario Root.

 Para iso executamos:

```
sudo -i
```

Unha vez sexamos root, navegaremos ata a ruta onde está a configuración.

```
cd /srv/dev-disk-by-uuid-dee6e7ad-abb0-483d-93f4-3052adb442d4/appdata/nextcloud/config/www/nextcloud/config
```
Agora abriremos o arquivo co editor de texto nano:

```Bash

nano config.php
```
Dentro do arquivo, temos que buscar o bloque que pon **trusted_domains**. Temos que engadir unha liña nova coa nosa IP seguindo a orde dos números.

Ten que quedar asi:

```PHP

  'trusted_domains' => 
  array (
    0 => 'localhost',
    1 => '100.114.185.22',
  ),
```

Para rematar:

* Pulsamos Ctrl + O e logo Enter para gardar.
* Pulsamos Ctrl + X para saír do editor.
* Escribimos exit para pechar a sesión de root.

Agora, se recargamos a páxina web, o acceso estará permitido e xa veremos o login de Nextcloud.




## 11. Conclusión

Este proxecto permitiu crear un servidor NAS funcional, seguro e totalmente personalizado utilizando unha Raspberry Pi 5. Ademais de ofrecer unha solución práctica para almacenar datos persoais, o proceso serviu para adquirir coñecementos en administración de sistemas, servizos de rede, e configuración de hardware.

E aprendin cousas que non se dan no ciclo, porque non nos aprenden a crear unha vpn propia ou un servidor de almacenamiento que pode ser mui util. 


## 12. Anexos

Inclúense:

* Imaxes da montaxe
* Capturas de comandos
* Links de compra
* SoftWare de instalacion



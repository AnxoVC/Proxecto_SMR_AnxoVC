# Links de compra utilizados

### 1. Placa Base e Almacenamento
* **Raspberry Pi 5 (8GB RAM):**
  * [Páxina Oficial (Distribuidores Autorizados)](https://www.raspberrypi.com/products/raspberry-pi-5/)
  * *Nota: Escolleuse o modelo de 8GB para garantir fluidez ao executar múltiples contedores Docker simultaneamente.*

* **Tarxeta MicroSD (Sistema Operativo):**
  * **Modelo:** SanDisk Ultra microSDXC 128GB (Clase 10, A1)
  * [Ligazón de compra (Western Digital / SanDisk Store)](https://shop.sandisk.com/es-es/products/memory-cards/microsd-cards/sandisk-ultra-uhs-i-microsd-120-mbps)
  * *Nota: Recoméndase este modelo pola súa fiabilidade e velocidade de lectura (ata 120 MB/s), vital para o rendemento do sistema operativo.*

### 2. Expansión SATA (Radxa)
Para a conexión dos discos duros e a creación do RAID 5 utilizouse o HAT específico de Radxa, que aproveita o porto PCIe da Raspberry Pi 5.

* **Radxa Penta SATA HAT:**
  * [Páxina do Produto (Radxa)](https://radxa.com/products/accessories/penta-sata-hat)
  * *Características: Soporta ata 5 discos SATA e inclúe xestión de enerxía.*

* **Fonte de Alimentación (Para os Discos):**
  * **Modelo:** 12V / 3A (Conector 5.5x2.5mm)
  * [Páxina do Produto (Radxa)](https://radxa.com/products/accessories/power-dc12-36w)
  * *Importante: Esta fonte alimenta tanto aos discos duros como á propia Raspberry Pi a través do HAT, eliminando a necesidade dun cargador USB-C adicional.*
### 3. Refrixeración e Carcasa
Para garantir o silencio e a durabilidade do sistema dentro da carcasa impresa en 3D.

* **Ventilador Silencioso (40x10mm):**
  * **Modelo:** Noctua NF-A4x10 5V PWM
  * [Ligazón de compra (Amazon / Noctua Oficial)](https://www.amazon.es/dp/B00NEMGCIA/)
  * *Xustificación: Escolleuse a versión de **5V** para poder conectalo directamente aos pines GPIO da Raspberry Pi.*
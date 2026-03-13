# Links de compra utilizados

### 1. Placa Mãe e Armazenamento
* **Raspberry Pi 5 (8GB RAM):**
  * [Página Oficial (Distribuidores Autorizados)](https://www.raspberrypi.com/products/raspberry-pi-5/)
  * *Nota: Escolheu-se o modelo de 8GB para garantir fluidez ao executar múltiplos contêineres Docker simultaneamente.*

* **Cartão MicroSD (Sistema Operativo):**
  * **Modelo:** SanDisk Ultra microSDXC 128GB (Classe 10, A1)
  * [Link de compra (Western Digital / SanDisk Store)](https://shop.sandisk.com/es-es/products/memory-cards/microsd-cards/sandisk-ultra-uhs-i-microsd-120-mbps)
  * *Nota: Recomenda-se este modelo pela sua fiabilidade e velocidade de leitura (até 120 MB/s), vital para o desempenho do sistema operativo.*

### 2. Expansão SATA (Radxa)
Para a conexão dos discos rígidos e a criação do RAID 5 utilizou-se o HAT específico da Radxa, que aproveita a porta PCIe do Raspberry Pi 5.

* **Radxa Penta SATA HAT:**
  * [Página do Produto (Radxa)](https://radxa.com/products/accessories/penta-sata-hat)
  * *Características: Suporta até 5 discos SATA e inclui gestão de energia.*

* **Fonte de Alimentação (Para os Discos):**
  * **Modelo:** 12V / 3A (Conector 5.5x2.5mm)
  * [Página do Produto (Radxa)](https://radxa.com/products/accessories/power-dc12-36w)
  * *Importante: Esta fonte alimenta tanto os discos rígidos como o próprio Raspberry Pi através do HAT, eliminando a necessidade de um carregador USB-C adicional.*

### 3. Refrigeração e Carcaça
Para garantir o silêncio e a durabilidade do sistema dentro da carcaça impressa em 3D.

* **Ventilador Silencioso (40x10mm):**
  * **Modelo:** Noctua NF-A4x10 5V PWM
  * [Link de compra (Amazon / Noctua Oficial)](https://www.amazon.es/dp/B00NEMGCIA/)
  * *Justificação: Escolheu-se a versão de **5V** para poder ligá-lo diretamente aos pinos GPIO do Raspberry Pi.*
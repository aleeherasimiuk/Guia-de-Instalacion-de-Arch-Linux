<div align="center">
  <div style="display: flex; align-items: flex-start;">
    <img alt="arch logo" src="img/archlogo.png" width="100" height="100" />
    <h1>Guía de Instalación de Arch Linux en Español</h1>
  </div>
</div>

Arch Linux es una distrubución de GNU/LINUX que sigue un modelo de lanzamiento contínuo <<*Rolling Release*>>, esto quiere decir que el sistema se actualiza continua, sin interrupciones, con pequeñas actualizaciones. Todo esto implica que no existen versiones de Arch como de Ubuntu por ejemplo, que tiene lanzamientos completos cada tanto.

Además la instalación deja únicamente lo estrictamente necesario para funcionar. Y este proceso puede ser complejo, por eso elaboré esta guía de la forma más sencilla posible intentando explicar qué hace cada cosa.

Para el seguimiento de esta guía se asume que se conocen los comandos básicos de Linux así como también un conocimiento básico sobre grupos, usuarios, permisos, manejo de particiones, file system, archivos e inglés básico.

<!-- Index -->

# Índice

- [Índice](#índice)
- [Disclaimer](#disclaimer)
- [Guía Oficial](#guía-oficial)
- [Antes de Empezar](#antes-de-empezar)
  - [Instalación en Hardware](#instalación-en-hardware)
  - [Instalación sobre una Máquina Virtual](#instalación-sobre-una-máquina-virtual)
- [Preparación del medio de instalación](#preparación-del-medio-de-instalación)
- [Preparando la instalación](#preparando-la-instalación)
  - [Configuración de Teclado](#configuración-de-teclado)
    - [Para los curiosos](#para-los-curiosos)
  - [Conectándonos a internet](#conectándonos-a-internet)
  - [Configurando la fecha y hora](#configurando-la-fecha-y-hora)
- [Creando las Particiones](#creando-las-particiones)
  - [Explicación breve](#explicación-breve)
  - [Preparando las particiones](#preparando-las-particiones)
  - [(Opción 1) Root en EXT4](#opción-1-root-en-ext4)
    - [Ahora sí, creamos las particiones](#ahora-sí-creamos-las-particiones)
    - [Formateando y Montando las particiones](#formateando-y-montando-las-particiones)
  - [(Opción 2) Root en ZFS](#opción-2-root-en-zfs)
    - [Instalar ZFS en el Live System](#instalar-zfs-en-el-live-system)
    - [Formateado de las particiones](#formateado-de-las-particiones)
    - [Crear y montar los datasets](#crear-y-montar-los-datasets)
  - [(Opción 3) Root en BTRFS](#opción-3-root-en-btrfs)
    - [Formateando las particiones](#formateando-las-particiones)
    - [Crear y montar los subvolúmenes](#crear-y-montar-los-subvolúmenes)
- [Configurando los mirrors](#configurando-los-mirrors)
- [Ahora sí. Instalamos el sistema](#ahora-sí-instalamos-el-sistema)
- [Generando el archivo fstab](#generando-el-archivo-fstab)
- [Configurando el sistema](#configurando-el-sistema)
- [Instalando Programas](#instalando-programas)
- [Creando el usuario](#creando-el-usuario)
- [Instalando Network Manager](#instalando-network-manager)
- [(Opcional) Instalando ZFS](#opcional-instalando-zfs)
- [(Opcional) Instalando BTRFS](#opcional-instalando-btrfs)
- [Instalando GRUB](#instalando-grub)
- [Finalizando y primer reinicio](#finalizando-y-primer-reinicio)
- [Conectándonos a Internet (de nuevo)](#conectándonos-a-internet-de-nuevo)
- [Comandos útiles](#comandos-útiles)
- [Instalando Yay](#instalando-yay)
- [Instalando un Entorno de Escitorio](#instalando-un-entorno-de-escitorio)
  - [KDE Plasma](#kde-plasma)
- [Optimizando del sistema](#optimizando-del-sistema)
  - [Configuración de Pacman](#configuración-de-pacman)
  - [Ananicy (Nice daemon)](#ananicy-nice-daemon)
  - [SSD TRIM](#ssd-trim)

# Disclaimer

La siguiente guía no tiene garantía alguna. Sos completamente responsable de las acciones que harás a continuación y corrés el riesgo de perder todos los datos si no procedés con precaución. Leé 2 veces cada comando antes de ejecutarlo y prestá atención a todo lo que el Sistema Operativo devuelve ante cada comando. Si pasa que la instalación toma otro rumbo desconocido, ya sea por algun error o cambio en la instalación,
lo mejor es googlear en busca de la solución.

# Guía Oficial

Es altamente recomendable tener siempre a mano la [guía oficial de instalación de Arch Linux en inglés](https://wiki.archlinux.org/title/installation_guide) que tendrá todos los últimos cambios que se hayan hecho sobre la instalación así como también todas las cosas que haya que tener en cuenta ante situaciones específicas.

# Antes de Empezar

## Instalación en Hardware

Si estás en Windows y querés mantener el sistema operativo para poder entrar tanto a Windows como a Arch Linux, es conveniente que dejes una parte del disco sin asignar para poder instalar ahí Arch Linux. Si bien no es necesario mucho espacio para instalarlo es conveniente pensar de antemano cuánto espacio vas a necesitar.

Unos 80GB estarían bien, aunque todo depende de para que vayas a utilizar el sistema. Este particionado te recomendamos que lo hagas desde Windows, porque Linux y Windows suelen llevarse como perro y gato.

Agarrá un lapiz y un papel y anotá el tamaño de cada partición de tu disco duro. Es probable que lo necesites más adelante.

## Instalación sobre una Máquina Virtual

Si vas a realizar esta instalación sobre una máquina virtual, lo conveniente es que averigues la forma en la que se puede iniciar la máquina en modo UEFI.

En caso de que utilices una máquina VMWare, esto se logra añadiendo la siguiente línea al archivo .vmx de la vm: `firmware = "efi"`

Si usas Virt Manager, antes de terminar de configurar la máquina virtual tienes que elegir el sistema UEFI, como se ve en la captura de pantalla. Es posible que al hacerlo te cambie el orden de los discos de arranque, que hay que volver a ajustarlos:

![virt manager](img/virt-manager.png)

Otros softwares de máquinas virtuales tienen otras formas muy distintas de entrar en modo UEFI y algunos incluso no pueden hacerlo, por lo que dejamos esto en tus manos.
Dicho esto, empecemos.

# Preparación del medio de instalación

Bien, lo primero y principal es la preparación del dispositivo booteable con la imagen del sistema que vamos a instalar. Para eso hay que descargarlo desde [la página oficial de Arch Linux](https://www.archlinux.org/download/) y mediante un software como [Rufus](https://rufus.ie/en/) o [Etcher](https://www.balena.io/etcher/) y grabarlo en modo GPT para sistemas UEFI en una USB.

Luego hay que reiniciar la PC y seleccionar como medio de arranque el USB accediendo a la BIOS.

# Preparando la instalación

Si todo fue bien, habrás llegado a una pantalla como la siguiente:

![first screen](img/firstScreen.png)

Ahora mismo nos encontramos en un mini Sistema Operativo cargado desde el USB en la Memoria RAM de nuestra computadora que nos servirá para poder realizar la instalación y será tu ángel guardián, nunca lo borres porque es desde donde podés solucionar gran parte de los problemas que no te permitan acceder a Linux.

## Configuración de Teclado

Si te ganó la curiosidad y empezaste a escribir algo, te habrás dado cuenta que el teclado está distinto, y si no probá escribir una `ñ` o un `-` y vas a ver que no te hace caso y pone lo que se le da la gana.

Esto es porque por defecto el teclado viene con el layout estadounidense y tenemos que cambiarlo para poder estar cómodos con nuestro teclado. Para eso necesitamos saber en dónde vivimos (?) y qué layout corresponde.

Esta configuración tendrá efecto sólamente durante la instalación, luego veremos cómo hacerlo en el sistema instalado.

**TL;DR**

Si estás en latinoamérica el comando que tenés que ejecutar es el siguiente:

```sh
loadkeys la-latin1
```

Y para ahorrarte el trabajo, el `-` está en el lugar del `?`.

Para España (tu teclado probablemente tenga alguna tecla como esta: `Ç`) el comando será:

```sh
loadkeys es
```

Y así se puede volver al teclado estadounidense:

```sh
loadkeys us
```

### Para los curiosos

Las distribuciones disponibles para setear las podemos listar con el comando:

```sh
ls /usr/share/kbd/keymaps/**/*.map.gz
```

y veremos algo como lo siguiente:

![keyboard layout](img/keyboardLayout.png)

Como evidentemente no entran en la pantalla, podemos copiarlos todos en un archivo y verlos más tranquilamente con:

```sh
ls /usr/share/kbd/keymaps/**/*.map.gz > /tmp/keymaps.txt
nano /tmp/keymaps.txt
```

O también podemos pasarle el comando mediante una pipe a less, que formatea el output de un comando de forma en que podamos desplazarnos más cómodamente:

```sh
ls /usr/share/kbd/keymaps/**/*.map.gz | less
```

Teniendo ya el teclado configurado podemos continuar con la instalación.

## Conectándonos a internet

Si estás conectado por cable esto no te va a tomar mucho tiempo, podés probar hacer:

```sh
ping -c 5 archlinux.org
```

Y habrás obtenido algo similar a esto:

![ping archlinux.org](img/ping.png)

En ese caso podés continuar a la siguiente sección. Sino quedate que vamos a configurar el WiFi.

El instalador de Arch Linux viene con una herramienta que nos va a servir para conectarnos al WiFi durante la instalación que se llama `iwd` que se ejecuta como daemon (proceso en segundo plano) y que se configura con el comando:

```sh
iwctl
```

Así habrás entrado a una consola interactiva en donde podemos escribir los comandos para conectarnos al WiFi. Para ver los adaptadores de red conectados ejecutamos:

```sh
device list
```

Por ejemplo, podría aparecer como dispositivo `wlp1s0` o `wlan0` o similar. Y para ver las redes disponibles ejecutamos:

```sh
station wlan0 scan
station wlan0 get-networks
```

Reemplazá `wlan0` por el nombre del dispositivo que vas a usar. Luego de ver las redes disponibles vamos a conectarnos.

Supongamos que la red se llama `Batman` y la contraseña es `B4timovil`. Entonces nos conectamos con el siguiente comando:

```sh
station wlan0 connect Batman
## Nos va a pedir la contraseña y pondremos
B4timovil

## Para salir escribimos el
## siguiente comando o apretamos Ctrl + C.
exit
```

Ahora probá hacer un `ping` como mostré antes y podemos continuar con la instalación.

Si tu placa de red no encontró ninguna red para conectarse, o tuviste problemas para conectarte te recomiendo que te conectes al modem por cable y sigas así con la instalación. Es probable que luego de la instalación del sistema, los paquetes que instalemos te permitan conectarte al WiFi.

## Configurando la fecha y hora

Ahora que tenemos nuestro instalador conectado a internet, vamos a configurar la fecha y hora.

Para eso tenemos una herramienta que nos permite consultar o modificar la fecha y hora del sistema que es `timedatectl` y lo vamos a hacer así:

```shbr>
timedatectl set-ntp true
```

Con este comando ya le decimos que use internet para sincronizar la fecha y la hora. Para comprobarlo podemos usar el comando:

```sh
timedatectl status
```

# Creando las Particiones

*Ahora sí se viene lo chido...* diria Luisito Comunica.

Ya que tenemos la instalación del sistema preparada, vamos a crear las particiones que vamos a necesitar.

Si ya tenés Windows instalado, entonces ya hay particiones hechas que **NO TENES QUE TOCAR**. Entre ellas la EFI Partition y la partición de Windows (El disco C digamos...).

En mi caso no tengo Windows instalado y crearé las particiones de cero, pero vos ya tendrás 2 o más particiones asignadas.

Para listar los discos y particiones que tenemos usamos el siguiente comando:

```sh
lsblk
```

Si estás en una Máquina virtual o haciendo una instalación limpia habrás visto algo como lo siguiente:

![lsblk](img/lsblk1.png)

En cambio si estás instalando en una PC con windows instalado habrás visto algo más similar a lo siguiente:

![lsblk](img/lsblk3.png)

O a lo siguiente:

![lsblk](img/lsblk2.png)

## Explicación breve

Los discos instalados los vamos identificar por verse como `sda`, `sdb`, etc. O en el caso de nvme (que es una unidad de almacenamiento ultra rápida) se ven como `nvme0n1`. Si estás en una máquina virtual puede que se vean como `vda`.

El primer disco lo vamos a ver como sda, el segundo como sdb, y así sucesivamente.

Al mismo tiempo, cada disco puede tener sus particiones, por ejemplo, `sda1`, `sda2`, `sdb1`, `sdb2`, etc.

Cada partición puede organizarse internamente de distintas formas, que son los distintos File Systems. Por ejemplo el File System por defecto de Windows es NTFS, el de un PenDrive es Fat32 y el de Linux ext4. Y cada File System puede montarse en la ubicación que deseemos. En el ejemplo vemos que mi partición `nvme0n1p6` está montada en `/home` o en el caso de la Máquina Virtual el único volumen `sda` no está montado en ningún lugar.

## Preparando las particiones

Vamos a crear algunas particiones para instalar Arch Linux que van a ser las siguientes:

- EFI Partition
  - En esta partición están los archivos necesarios para que el Sistema Operativo arranque.

    Si tenés Windows instalado, esta partición ya existe y no tenés que tocarla porque sino puede que Windows no arranque.

    Si vas a instalar Arch Linux solo, o en una Máquina Virtual, vamos a asignarle unos 550MiB de espacio. Aquí estarán nuestras imágenes del kernel, el initramfs y el cargador de arranque.

- Swap
  - Es una partición que sirve para que el Sistema Operativo envíe los procesos que no se están ejecutando actualmente sobre todo en equipos con poca Memoria RAM.

    La partición SWAP no tiene punto de montaje ni File System.

    Por regla general si tenés hasta 2GB de Ram es recomendable usar 4GB de Swap. A partir de 4GB de Ram es conveniente tener no más de 4GB de Swap.

    Más swap puede ser útil si tenemos un portátil y queremos que hiberne a la Swap en vez de en Ram (teniendo alrededor de la misma cantidad de Swap que de Ram), o si tenemos que compilar proyectos grandes en los que nos podamos quedar sin Ram.

- Root
  - La partición de root la vamos a montar en `/`. Aquí es donde vivirán todos los archivos del sistema. A diferencia de la partición EFI, aquí tenemos varias opciones para el formato que darle, y también el número de particiones que se usarán o como se separarán los archivos.

## (Opción 1) Root en EXT4

Aquí explicamos cómo usar el formato EXT4 para la partición principal. Es uno de los formatos más usados y más antiguos: es fácil de configurar y ha sido muy testado en linux. Por otra parte, tiene menos características que otros sistemas como ZFS, que se detalla en la sección siguiente.

- Root
  - Si estás instalando sobre una PC real necesita tener por lo menos unos 25Gb de espacio (aunque puede ser más, en mi caso he necesitado hasta 50Gb).

- Home
  - La partición HOME la vamos a montar en `/home` y la vamos a formatear como `ext4` también.

    Acá van a estar todos los archivos personales (por ejemplo todo lo que está en el escritorio, la carpeta de descargas, fotos, videos, configuraciones, etc).

    Vamos a asignarle todo el resto del espacio que nos sobre.

### Ahora sí, creamos las particiones

Después de tanto blabla vamos a ejecutar el siguiente comando para crear las particiones. Una vez tengamos reconocido el disco al que instalar, en nuestro caso sda:

```sh
cfdisk /dev/sda
```

Si nos pregunta qué tipo de tabla de particiones queremos le decimos que vamos a usar GPT. Hasta este punto tendremos algo similar a esto:

![cfdisk](img/cfdisk.png)

Si tenés windows instalado es **CLAVE** que no toques las particiones que ya están hechas y nos vamos a enfocar en aquellas que dicen `Free Space`.

Acá te podés mover con las flechas hacia arriba y abajo para seleccionar la partición, o izquierda y derecha para seleccionar una opción.

Si nos paramos en `Free Space` y seleccionamos `[New]` nos preguntará el espacio que le queremos asignar y vamos poner lo que ya anotamos antes, en mi caso voy a instalarlo en un disco de 120GB y lo voy a hacer de la siguiente manera:

- 550MiB para EFI Partition
- 4G para Swap
- 25G para Root
- 90G para Home

Prestá atención a cómo escribí las dimensiones.

Y con si seleccionamos `[Type]` vamos a poder seleccionar el tipo.

- EFI System para la EFI Partition
- Linux Root (x86-64) para Root
- Linux Home para Home
- Linux Swap para la Swap

Sacale foto a como quedó, lo vas a necesitar. A mí me quedó algo así:

![cfdisk2](img/cfdisk2.png)

Entonces nos queda:

- `/dev/sda1` para EFI Partition
- `/dev/sda2` para Swap
- `/dev/sda3` para Root
- `/dev/sda4` para Home

Finalmente le damos a `[Write]`, confirmamos la operación y salimos con `[Quit]`.

### Formateando y Montando las particiones

Vamos a formatear y montar las particiones que acabamos de crear.

Como dijimos, vamos a usar ext4 como File System y vamos a formatear las particiones de Root y de Home con ese File System:

```sh
mkfs.ext4 /dev/sda3
mkfs.ext4 /dev/sda4
```

Ahora toca formatear la partición EFI. **SOLAMENTE SI LA ACABÁS DE CREAR SINO NO EJECUTES ESTE COMANDO**.

```sh
mkfs.fat -F32 /dev/sda1
```

Por último queda formatear la partición de Swap.

```sh
mkswap /dev/sda2
```

Ahora queda montar las particiones que creamos sobre el File System del instalador para poder instalar sobre ellas el Sistema Operativo.

Lo haremos de la siguiente manera:

```sh
mount /dev/sda3 /mnt # Montamos Root sobre /mnt

mkdir /mnt/home # Creamos la carpeta Home
mount /dev/sda4 /mnt/home # Montamos Home sobre /mnt/home

mkdir /mnt/boot # Creamos la carpeta Boot
mount /dev/sda1 /mnt/boot # Montamos la partición EFI sobre /mnt/boot

swapon /dev/sda2 # Activamos Swap
```

Y con esto ya tenemos creadas las particiones y montadas sobre el File System del USB para poder instalar el sistema.

Luego si querés saber cómo quedaron las particiones podés usar este comando:

```sh
fdisk -l
```

![particiones](img/fdisk.png)

## (Opción 2) Root en ZFS

Como alternativa a usar EXT4, podemos usar el formato ZFS. Este formato tiene una serie de características muy interesantes, incluyendo pero no limitado a:

- **Compresión transparente del sistema:**

  Todos los archivos automáticamente se comprimen y descomprimen al escribirlos al disco, reduciendo el espacio ocupado y el número de lecturas/escrituras que desgastan el disco. El algoritmo de compresión es muy rápido y no supone un gran coste de computación.

- **Encriptado nativo:**

  El sistema nos preguntará por la contraseña del disco al arrancar. Esto hace que si nos roban el disco, el atacante no pueda acceder a los datos del interior sin la contraseña.

- **Subvolúmenes:**

  Se puede entender como "particiones" dentro de nuestro disco que almacenan distintos tipos de datos. Sin embargo no tienen un tamaño fijo y se pueden crear, destruir, copiar, en cualquier momento.

- **Snapshots:**

  Copias de seguridad de volúmenes, que debido a cómo funcionan el sistema (Copy on Write), no ocupan espacio al crearlas, sino que empiezan a ocupar cuando hay diferencias entre el estado del volumen y la copia. Por ejemplo: si borramos un archivo de 1MB del volumen, la copia pasará a ocupar 1MB.

- **Integridad de archivos:**

  ZFS desconfía de los discos, y hace todo lo posible para comprobar que los archivos no se corrompan, como crear una checksum al modificarlos.

- **Otras funciones:**

  - RAID nativo: usar varios discos como un disco grande en el que se puede recuperar información perdida cuando uno o algunos de los discos se rompe.

  - Deduplicación de archivos: eliminación las copias de archivos que están en dos ubicaciones distintas para ahorrar espacio.

  - ZFS send/receive (enviar volúmenes por internet)

  - Y más...

El punto negativo es que por problemas de licencia, los drivers no se instalan con el kernel y se tendrán que instalar por separado.

Empezamos igual que en EXT4 creando las particiones:

```sh
cfdisk /dev/sda
```

Actuamos muy similarmente a como se haría con ext4, nos paramos en `Free Space` y seleccionamos `[New]` nos preguntará el espacio que le queremos asignar y vamos poner lo siguiente:

- 550MiB para EFI Partition
- 4G para Swap
- El resto para el Root

Y con si seleccionamos `[Type]` vamos a poder seleccionar el tipo.

- EFI System para la EFI Partition
- Linux Swap para la Swap
- Linux Filesystem para el Root

![zfs_cfdisk](img/zfs_cfdisk_0.png)

Finalmente le damos a `[Write]`, confirmamos la operación y salimos con `[Quit]`.

### Instalar ZFS en el Live System

A diferencia de EXT4, los drivers de ZFS no vienen incluidos en el live system de Arch, por lo que para poder formatear nuestra partición primero tendremos que instalarlos.

Por suerte un miembro de la comunidad ha puesto a disposición un script que lo hace de forma automática:

```sh
curl -s https://eoli3n.github.io/archzfs/init | bash
```

![zfs_archzfs](img/zfs_archzfs.png)

### Formateado de las particiones

Creamos las particiones EFI y SWAP:

```sh
mkfs.fat -F32 /dev/sda1
mkswap /dev/sda2
```

Para crear el volumen ZFS, la documentación de ZFS recomienda elegir el disco según su id, en vez de `/dev/sd[letra]`. Para conocer su id, ejecutamos:

```sh
ls -l /dev/disk/by-id
```

![zfs_diskbyid](img/zfs_diskbyid.png)

En nuestro caso, el que apunta a `/dev/sda3` es `/dev/disk/by-id/ata-QEMU_HARDDISK_QM00003-part3`.

Para crear el volumen ZFS, habrá que pasar varias opciones. Se listan una debajo de otra para poder leerse mejor, aunque se pueden pasar seguidas:

```sh
zpool create -f -o ashift=12           \
             -O acltype=posixacl       \
             -O xattr=sa               \
             -O relatime=on            \
             -O atime=off              \
             -O dnodesize=auto         \
             -O normalization=formD    \
             -O mountpoint=none        \
             -O canmount=off           \
             -O devices=off            \
             -R /mnt                   \
             zroot /dev/disk/by-id/ata-QEMU_HARDDISK_QM00003-part3
```

Si queremos compresión, añadimos:

```sh
             -O compression=lz4       \
```

Si queremos encriptado con contraseña, añadimos:

```sh
             -O encryption=aes-256-gcm \
             -O keyformat=passphrase   \
             -O keylocation=prompt     \
```

![zfs_format](img/zfs_format.png)

### Crear y montar los datasets

Como hemos comentado antes, en ZFS se puede crear unos "volúmenes" o datasets, con los que podemos separar partes del disco sin tener que hacer una partición para cada una.

Vamos a crear algunos datasets:

- **zroot/arch/default:** aquí estarán los archivos del sistema. Antes de realizar una actualización, podemos hacerle una snapshot (copia) y si algo va mal, podemos volver a la versión anterior. También podremos instalar otros sistemas operativos bajo zroot/ubuntu, zroot/fedora, etc.

- **zroot/var/log:** se montará en /var/log, y será independiente de zroot/arch, asi si algo ha ido mal y tenemos que reiniciar desde un snapshot, podremos acceder a los logs del sistema.
- **zroot/data/home:** se montará en /home y tendrá los archivos de los usuarios, independiente del sistema operativo.

Los comandos serán:

```sh
zfs create -o mountpoint=none                   zroot/arch
zfs create -o mountpoint=/ -o canmount=noauto   zroot/arch/default
zfs create -o mountpoint=none                   zroot/var
zfs create -o mountpoint=/var/log               zroot/var/log
zfs create -o mountpoint=none                   zroot/data
zfs create -o mountpoint=/home                  zroot/data/home
```

Desmontamos y volvemos a montar todo para comprobar que no haya errores:

```sh
zpool export zroot
zpool import -d /dev/disk/by-id/ata-QEMU_HARDDISK_QM00003-part3 -R /mnt zroot -N
zfs load-key zroot # Sólo si hemos usado encriptado
zfs mount zroot/arch/default
zfs mount -a
mkdir /mnt/boot
mount /dev/sda1 /mnt/boot
swapon /dev/sda2
```

Comprobamos que esté todo correcto:

![particiones](img/zfs_check.png)

## (Opción 3) Root en BTRFS

Finalmente como otra alternativa podemos usar el sistema btrfs. Se trata de un sistema que es un compromiso entre la cantidad de características de ZFS, pero fácil de instalar e incluido por defecto en el kernel, como EXT4. En Fedora 33 pasó a ser el formato de almacenamiento por defecto. Entre sus características:

- **Compresión transparente del sistema:**

  Todos los archivos automáticamente se comprimen y descomprimen al escribirlos al disco, reduciendo el espacio ocupado y el número de lecturas/escrituras que desgastan el disco. El algoritmo de compresión es muy rápido y no supone un gran coste de computación.


- **Subvolúmenes:**

  Se puede entender como "particiones" dentro de nuestro disco que almacenan distintos tipos de datos. Sin embargo no tienen un tamaño fijo y se pueden crear, destruir, copiar, en cualquier momento.

- **Snapshots:**

  Copias de seguridad de volúmenes, que debido a cómo funcionan el sistema (Copy on Write), no ocupan espacio al crearlas, sino que empiezan a ocupar cuando hay diferencias entre el estado del volumen y la copia. Por ejemplo: si borramos un archivo de 1MB del volumen, la copia pasará a ocupar 1MB.

Igual que en las demás opciones, empezamos creando las particiones y después les daremos el formato.


```sh
cfdisk /dev/sda
```

Actuamos muy similarmente a como se haría con ext4, nos paramos en `Free Space` y seleccionamos `[New]` nos preguntará el espacio que le queremos asignar y vamos poner lo siguiente:

- 550MiB para EFI Partition
- 4G para Swap
- El resto para el Root

Y con si seleccionamos `[Type]` vamos a poder seleccionar el tipo.

- EFI System para la EFI Partition
- Linux Swap para la Swap
- Linux Filesystem para el Root

![btrfs_cfdisk](img/zfs_cfdisk_0.png)

Finalmente le damos a `[Write]`, confirmamos la operación y salimos con `[Quit]`.


### Formateando las particiones

Creamos las particiones EFI y SWAP:

```sh
mkfs.fat -F32 /dev/sda1
mkswap /dev/sda2
```

Para crear el sistema btrfs sólo necesitamos:

```sh
mkfs.btrfs /dev/sda3
```

### Crear y montar los subvolúmenes

Podemos entender los subvolúmenes en btrfs como unas "carpetas" especiales. Por una parte, las podemos montar y asignar disntintos parámetros como si fuesen particiones, pero no ocupan un espacio fijo. La convención es llamar a los subvolúmenes empezando con la letra '@' para saber que es un subvol.

Igual que con ZFS, podemos organizarlos como queramos. En este tutorial proponemos la siguiente organización:


- **@arch:** aquí estarán los archivos del sistema. Antes de realizar una actualización, podemos hacerle una snapshot (copia) y si algo va mal, podemos volver a la versión anterior. También podremos instalar otros sistemas operativos bajo @ubuntu, @fedora, etc.

- **@var_log:** se montará en /var/log, y será independiente de @arch, asi si algo ha ido mal y tenemos que reiniciar desde un snapshot, podremos acceder a los logs del sistema.
- **@home:** se montará en /home y tendrá los archivos de los usuarios, independiente del sistema operativo.


Primero montamos el sistema base de forma provisional:

```sh
mount /dev/sda3 /mnt
cd /mnt
```

Creamos los subvolúmenes:

```sh
btrfs subvolume create @arch
btrfs subvolume create @var_log
btrfs subvolume create @home
```

Si hacemos un simple `ls`, podemos ver que se han creado unas carpetas con los mismos nombres. Sin embargo con `btrfs subvolume list /mnt`, el comando nos dice que están registradas esas carpetas como volúmenes.

Ahora mismo el volumen por defecto de `/dev/sda3` es `/`, es decir, la raíz. Para evitar problemas en el arranque vamos a registrar nuestro volumen `@arch` como el volumen por defecto.

```sh
btrfs subvolume set-default @arch
```

Ahora desmontamos el sistema y volvemos a montar todo con sus opciones. En este caso usaremos compresión transparente `lzo`, que es la que ofrece un mejor rendimiento. Si queremos un sistema más comprimido podemos usar `zstd`, o `no` para no usar compresión (el valor por defecto si borramos la opción es `ztsd`).

```sh
cd /
umount /mnt
mount -o noatime,space_cache=v2,compress=lzo /dev/sda3 /mnt
mkdir -p /mnt/{home,var/log,boot}
mount -o noatime,space_cache=v2,compress=lzo,subvol=@home /dev/sda3 /mnt/home
mount -o noatime,space_cache=v2,compress=lzo,subvol=@var_log /dev/sda3 /mnt/var/log
mount /dev/sda1 /mnt/boot
swapon /dev/sda2
```

Si todo ha salido bien, deberíamos ver un montaje así:

![Btrfs btrfs_check](img/btrfs_check.png)


# Configurando los mirrors

Los paquetes que instalamos se descargan de servidores que están repartidos por todo el mundo, hay un archivo que ahora vamos a revisar, que tiene en orden de prioridad descendente cada uno de los servidores. Normalmente ya viene configurado con los mirrors más rápidos para nuestra ubicación. Lo podemos ver con el siguiente comando:

```sh
cat /etc/pacman.d/mirrorlist
```

![mirrorlist](img/mirrorlist.png)

Si ves algo como lo de la imagen, entonces ya podés continuar a la próxima sección, si no, quedate que los vamos a configurar.

Vamos a usar `reflector` que es una herramienta que se va a encargar de probar todos los servidores actualizados y meterlos en el archivo mirrorlist ordenados según la velocidad. Lo haremos con el siguiente comando:

```sh
cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.bak ## Creamos un Backup por las dudas
reflector --verbose --latest 5 --save /etc/pacman.d/mirrorlist ## Hacemos la magia
cat /etc/pacman.d/mirrorlist ## Comprobamos que se han añadido los servidores

## Y si algo salió mal:
cp /etc/pacman.d/mirrorlist.bak /etc/pacman.d/mirrorlist ## Levantamos el backup.

## Sino ya podemos borrar el backup
rm /etc/pacman.d/mirrorlist.bak

## Copiamos nuestros mirrors desde el Live a nuestro sistema para no tener que recrearlo
mkdir -p /mnt/etc/pacman.d
cp /etc/pacman.d/mirrorlist /mnt/etc/pacman.d/mirrorlist
```

# Ahora sí. Instalamos el sistema

Para instalar el sistema, el instalador de Arch Linux nos provee de una herramienta llamada `pacstrap` y lo vamos a usar de la siguiente manera:

```sh
pacstrap /mnt base base-devel linux linux-firmware
```

Puede que tarde un rato, mientras se instala te cuento que esta herramienta va a usar los servidores que configuramos hace un ratito y va a descargar el sistema e instalarlo sobre `/`, te acordás que lo montamos en `/mnt` ¿no?.

Luego usaremos otra herramienta llamada `pacman` para instalar los paquetes.

Bien. Ya tenemos Arch Linux instalado, pero esto todavía no termina. Nos vemos en la próxima sección.

# Generando el archivo fstab

¿Te acordás que hace un rato montamos las particiones a mano con el comando `mount`?

Bien, hacer eso todo el tiempo es insano, por eso el Sistema Operativo tiene un archivo con la tabla de File Systems y se encarga de automatizar el proceso. Para generar ese archivo hacemos:

```sh
genfstab -U /mnt >> /mnt/etc/fstab
```

**IMPORTANTE**, sólo en ZFS: tenemos que eliminar todas las lineas del fstab que hagan referencia a nuestro zroot, dejando las demás.

Y si por curiosidad queremos verlo podemos hacer:

```sh
cat /mnt/etc/fstab
```

Se vería algo así:

![fstab](img/fstab.png)

# Configurando el sistema

Lo primero es movernos al Root de nuestra máquina que creamos hace un rato y donde instalamos el sistema operativo:

```sh
arch-chroot /mnt
```

Para definir la zona horaria vamos a crear un Soft Link de nuestra región+ciudad.

Para ver qué regiones hay disponibles lo hacecmos con:

```sh
ls /usr/share/zoneinfo/
```

Y si queremos ver qué ciudad hay para una región tenemos que hacer:

```sh
ls /usr/share/zoneinfo/America/
```

Finalmente lo seteamos:

```sh
ln -s /usr/share/zoneinfo/America/Buenos_Aires /etc/localtime
```

Para configurar la hora usamos hwclock que va a generar el archivo `/etc/adjtime`:

```sh
hwclock --systohc
```

Ahora toca configurar el idioma.
Para ver los idiomas disponibles podemos ver el archivo haciendo:

```sh
cat /etc/locale.gen
```

Sí, ya se, no entran en la pantalla. Para ver los que nos interesan podemos usar:

```sh
cat /etc/locale.gen | grep -E "en|es" | grep UTF
```

Elegimos uno (o alguno) de esos. Y hacemos lo siguiente. En mi caso voy a seleccionar inglés de Estados Unidos y español de Argentina, pero vos podes seleccionar el que más te guste.

```sh
echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen
echo "es_AR.UTF-8 UTF-8" >> /etc/locale.gen
```

Y generamos los archivos de localización con:

```sh
locale-gen
```

Si querés ver el idioma seteado podés hacer:

```sh
locale
```

Y si querés cambiar el idioma podés hacer:

```sh
localectl set-locale LANG=en_US.UTF-8
```

Ahora seteamos el layout del teclado (si, de nuevo, pero ahora en el Sistema Operativo instalado)

```sh
echo "KEYMAP=la-latin1" > /etc/vconsole.conf
```

Tenemos que definir un nombre para el equipo. El mío se va a llamar `archvm` y lo hacemos de la siguiente manera:

```sh
echo "archvm" > /etc/hostname
```

Bien, ahora tenemos que definir el archivo `/etc/hosts`.

Nos va a permitir mapear algunos alias a algunas IP's específicas.

De esta manera, por ejemplo, vamos a poder usar `localhost` para referirnos a nuestro equipo.

Reemplazá `archvm` por el nombre de tu equipo. Revisá que esté todo bien copiado

```sh
echo -e "127.0.0.1\tlocalhost" >> /etc/hosts
echo -e "::1\t\tlocalhost" >> /etc/hosts
echo -e "127.0.1.1\tarchvm.localhost archvm" >> /etc/hosts
```

Tuvo que haber quedado algo así:

```sh
cat /etc/hosts
```

![hosts](img/hosts.png)

Con esto ya terminamos la parte más pesada de la instalación. Continuamos con la configuración del usuario, pero antes vemos cómo instalar paquetes.

# Instalando Programas

El gestor de paquetes por defecto de Arch Linux es `Pacman`, como luego vamos a tener que usarlo paso a explicar ahora como funciona. Memorizate estos comandos porque los vas a necesitar:

```sh
pacman -S <nombre_paquete> ## Instala un paquete
pacman -Rns <nombre_paquete> ## Desinstalar un paquete
pacman -Sy ## Actualiza la base de datos de los paquetes
pacman -Syu ## Actualizar todos los paquetes y el Sistema Operativo
pacman -h ## Listar las posibles opciones que te acabo de listar
```

Entonces vamos a instalar algunas cosas como por ejemplo un editor de texto. Yo uso vim, pero si preferís usar nano, neovim, o algun otro podés instalarlo de igual manera:

```sh
pacman -S vim
```

Podemos instalar varios paquetes de una también:

```sh
pacman -S vim nano htop neofetch
```

*pss*: Cuando estemos en el sistema ya instalado, logueados con nuestro usuario, vamos a tener que ejecutar este comando con `sudo`

# Creando el usuario

Lo primero es definir la contraseña del root.

```sh
passwd
```

Y ahora creamos el usuario, en mi caso lo voy a llamar batman, vos llamalo como quieras. Luego le asignamos una contraseña.

```sh
useradd -m batman

passwd batman
```

Vamos a instalar `sudo` para poder ejecutar comandos con privilegios de root.

```sh
pacman -S sudo
```

Luego, nos agregaremos el grupo wheel a los grupos que tienen permiso para usar sudo.

```sh
## Usamos este comando para editar el archivo
EDITOR=vim visudo

## Y descomentamos esta línea
%wheel ALL=(ALL) ALL

## Salimos de vim con ESC y escribimos :wq!
```

Si leíste lo que estamos haciendo, dice que hay que descomentar la línea para que todos aquellos usuarios que estén en el grupo `wheel` puedan ejecutar comandos de sudo, con lo cual ahora toca asignarnos ese grupo. Aparte de eso, vamos también a asignarnos otros grupos que podrían llegar a ser necesarios:

```sh
usermod -aG wheel,audio,video,storage batman
```

# Instalando Network Manager

Para poder conectarnos a internet en nuestro sistema ya instalado vamos a tener que usar una herramienta llamada `Network Manager`, pero claro, tenemos que instalarla antes de reiniciar porque sino no vamos a tener internet para instalarlo luego.

```sh
pacman -S networkmanager
```

Ya tenemos instalado el servicio, pero aún tenemos que habilitarlo para que se inicie junto con el sistema operativo y quede ejecutándose:

```sh
systemctl enable NetworkManager
```

Todavía no nos vamos a conectar, porque seguimos con la conexión que teníamos en el instalador. Luego de reiniciar nos conectamos

# (Opcional) Instalando ZFS

Si optamos por usar ZFS en el sistema, tenemos que terminar de configurarlo antes.

Como ya se ha comentado, por los problemas de licencia se deben de instalar los drivers a parte del kernel. Para EXT4 los drivers ya vienen incluidos en el kernel y no hace falta más configuración.

Lo primero que debemos de hacer es habilitar unos repositorios en los que se distribuyen los módulos de kernel ya compilados para el kernel que usar Arch.

Con nuestro editor de texto favorito, editamros `/etc/pacman.conf` y añadimos al final:

```txt
[archzfs]
Server = https://archzfs.com/$repo/x86_64
```

Sincronizamos la base de datos de paquetes:

```sh
pacman -Syu
```

Para instalar los módulos del kernel deberemos instalar la versión del paquete que se corresponda con nuestro kernel. Anteriormente instalamos `linux`, así que tenemos que usar el paquete `zfs-linux`. Si más adelante instalamos otro kernel como `linux-lts`, habrá que instalar también `zfs-linux-lts`.

```sh
pacman -S zfs-linux
```

Después, ajustamos nuestro sistema para que pueda arrancar correctamente desde ZFS, generando el `hostid` y el caché de discos:

```sh
zgenhostid
zpool set bootfs=zroot/arch/default zroot
zpool set cachefile=/etc/zfs/zpool.cache zroot
systemctl enable zfs.target
systemctl enable zfs-import-cache.service
systemctl enable zfs-mount.service
systemctl enable zfs-import.target
```

Editando `/etc/mkinitcpio.conf` para que se incluyan los drivers de ZFS en el initramfs, en la parte "HOOKS", borramos `fsck` y `filesystems` y añadimos `zfs filesystems` al final, quedando:

```txt
HOOKS=(base udev autodetect modconf block keyboard zfs filesystems)
```

Recreamos el initramfs con:

```sh
mkinitcpio -P
```

Finalmente nos adelantamos un paso: en la sección siguiente instala GRUB, y antes de usar `grub-mkconfig` vuelve aquí.

Tenemos que editar `/etc/default/grub`, para añadir los parámetros:

```txt
GRUB_CMDLINE_LINUX="dozfs root=ZFS=zroot/arch/default"
```

Generamos la configuración de grub con:

```sh
ZPOOL_VDEV_NAME_PATH=1 grub-mkconfig -o /boot/grub/grub.cfg
```

# (Opcional) Instalando BTRFS

Si hemos usado btrfs como formato para nuestra partición, antes de terminar tenemos que instalar los componentes necesarios para que funcione.

```sh
pacman -S btrfs-progs
```

También tenemos que asegurarnos que nuestro initramfs contenga los módulos de btrfs. Editamos nuestro `/etc/mkinitcpio.conf` y editamos:

```sh
# Por defecto viene vacío
MODULES=(btrfs)
```

Guardamos, y regeneramos el initramfs:

```sh
mkinitcpio -P
```


# Instalando GRUB

Ahora toca instalar Grub, que es el gestor de arranque de nuestro sistema operativo. Nos va a permitir elegir el sistema operativo con el que vamos a arrancar cada vez que iniciemos la PC.

Presta atención y lee 2 veces antes de ejecutar cada comando, si lo hacés mal puede ser que no arranque el sistema operativo. Vamos a ejecutar los siguientes comandos:

```sh
## Lee bien cada comando antes de ejecutarlo
pacman -S grub efibootmgr dosfstools
grub-install --target=x86_64-efi --efi-directory=/boot
grub-mkconfig -o /boot/grub/grub.cfg
```

# Finalizando y primer reinicio

Luego hay que cruzar los dedos, reiniciar y sacar el pen drive

```sh
exit
umount -R /mnt
```

```sh
# Solo si usamos ZFS
zfs unmount -a
zpool export zroot
```

```sh
reboot
```

Si todo salió bien, al reiniciar se habrá iniciado GRUB y te dió a elegir la posibilidad de entrar a Arch Linux habiendo llegado a algo como esto:

![listo](img/listo.png)

Ponemos nuestro usuario y contraseña y ya hemos terminado con la instalación del Sistema Operativo.

# Conectándonos a Internet (de nuevo)

Sí, ya sé que nos habíamos conectado a internet antes con `iwctl`, pero eso fue en el instalador, ahora si ya tenes el PC conectado por cable, podés continuar. Sino quedate que vamos a conectarnos al WiFI.

Network Manager nos ofrece `nmcli` para conectarnos al WiFi. Si por ejemplo nuestra Red Wifi se llama `Batman` y la contraseña es `B4timovil`:

```sh
nmcli device wifi list ## Listar las redes disponibles
nmcli device wifi connect Batman pasword B4timovil
```

Si tu Red WiFi o contraseña tiene espacios, ponela entre comillas.

# Comandos útiles

Ya tenemos una bella, hermosa, y muy poco intuitiva terminal.

Por eso te voy a dejar acá algunos comandos útiles. No te olvides que de todas formas google existe!.

```sh
ping archlinux.org ## Comprobar si tenemos conexión a internet
reboot ## Reiniciar
shutdown now ## Apagar
ls -l ## Listar todos los archivos del directorio actual
cd <nombreDirectorio> ## Cambiar de directorio
cd .. ## Volver hacia atrás
cd ~ ## Ir a la carpeta home
pwd ## Saber en qué directorio estás
cat <nombreArchivo> ## Leer un archivo de texto


CTRL + C ## Para parar una ejecución
```

# Instalando Yay

A veces los repositorios oficiales de Arch Linux (esos que usa `pacman` para descargar cosas) no tienen todo lo que queremos (Y sinó hace un `sudo pacman -S google-chrome` y fijate que pasa).

Por eso hay repositorios de la comunidad (AUR) que nos van a facilitar la vida. En nuestro caso vamos a usar [Yay](https://github.com/Jguer/yay). Para poder instalarlo tenemos que ejecutar esta secuencia de comandos:

```sh
sudo pacman -Syu git
cd /tmp
git clone https://aur.archlinux.org/yay-git.git
cd yay-git
makepkg -sric ## Se compilará e instalará
cd ~ ## Volvemos a nuestro home
rm -rf /tmp/yay-git # Ya no necesitamos esto, yay es capaz de actualizarse a él mismo
```

Y con esto ya podemos instalar paquetes con `yay`. Lo bueno es que tiene casi las mismas opciones que `pacman`. Entonces, lógicamente para instalar un paquete con `yay` tenemos que ejecutar:

```sh
yay -S <nombrePaquete> ## Mirá que no puse sudo
```

Y para actualizar todos los paquetes... sí, adivinaste:

```sh
yay -Syu
```

# Instalando un Entorno de Escitorio

Arch Linux viene pelado, le tenemos que instalar un entorno de escritorio para poder usarlo.

Te dejo algunos que seguro ya conocés de otras distribuciones, pero tené en cuenta que hay más y que hay todo un mundo y una vez que entrás ya no podés salir (googleá `qtile`, `xmonad`, `i3`, etc)

## KDE Plasma

Al principio es feo como golpearse el dedo chiquito del pie con la pata de la cama, pero con un poco de amor puede quedar muy bien.

```sh
sudo pacman -S xorg ## Gestiona la comunicación con los distintos periféricos y nuestro SO
sudo pacman -S plasma-meta kde-applications-meta ## Instalamos KDE Plasma
sudo systemctl enable sddm ## Habilitamos el servicio que nos permitirá loguearnos en el siguiente inicio
reboot
```

Paciencia, esto va a tardar un rato.

Si por algun motivo, ves que aparece algo como esto:

```sh
WARNING: Possible missing firmware for moudule: wd719x
WARNING: Possible missing firmware for moudule: aic94xx
WARNING: Possible missing firmware for moudule: xhci_pci
```

No te hagas problema que lo solucionamos más tarde.

# Optimizando del sistema

## Configuración de Pacman

Usando nuestro editor favorito, vamos a cambiar unas opciones de configuración de pacman.

Para empezar, abrimos `/etc/pacman.conf`, y cambiaremos:

```sh
Color # Por defecto viene comentado, activa los colorines en los comandos
ParallelDownloads = 5 # Descargar varios paquetes a la vez, en vez de uno a uno
```

Ahora abriremos `/etc/makepkg.conf` y editamos las líneas en las que se encuentran...

```sh
# Usar todos los nucleos disponibles cuando nos encontremos con paquetes que haya que compilar.
MAKEFLAGS="-j$(nproc)"

# Compilar los paquetes necesarios desde la RAM en vez del disco duro. Reducimos el desgaste del disco y aceleramos la compilación. No usar si tenemos <4GB RAM.
BUILDDIR=/tmp/makepkg
```

Cuando hagamos una actualización al sistema, puede que haya archivos de configuración en `/etc` que también haya que actualizarlos. Para ello podemos usar la herramienta:

```sh
yay -S etc-update
# Después de hacer yay -Syu, ejecutamos
sudo etc-update
```

Durante la instalación, ya configuramos los mirrors. Si en el futuro alguno de estos mirrors se cae o se se vuelve lento, seguirá en nuestra configuración, pero no queremos eso. Instalamos `reflector` en nuestro sistema y lo configuramos:

```sh
yay -S reflector
```

Configuramos reflector para que use los mirrors más rápidos y recientes, editando el archivo `/etc/xdg/reflector/reflector.conf`:

```
--save /etc/pacman.d/mirrrorlist
--protocol https
--latest 30
--sort rate
```

```sh
sudo systemctl enable --now reflector.timer
```

## Ananicy (Nice daemon)

Una herramienta que podemos instalar se llama "Ananicy", que automaticamente establece las prioridades de los procesos para que el sistema responda mejor.

```sh
yay -S ananicy
sudo systemctl enable --now ananicy
```

## SSD TRIM


Para reducir el desgaste en nuestro SSD, tenemos que hacer un TRIM cada cierto tiempo. Se puede activar con:

```sh
sudo systemctl enable --now fstrim.timer
```

En ZFS podemos activarlo con:

```sh
sudo zpool set autotrim=on zroot
```

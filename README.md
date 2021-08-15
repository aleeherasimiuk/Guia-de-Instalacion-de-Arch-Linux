<div align="center">
  <div style="display: flex; align-items: flex-start;">
    <img alt="arch logo" src="img/archlogo.png" width="100" height="100" />
    <h1>Guía de Instalación de Arch Linux en Español</h1>
  </div>
</div>

<p align="justify">Arch Linux es una distrubución de GNU/LINUX que sigue un modelo de lanzamiento
  contínuo <<<i>Rolling Release</i>>>, o sea que cada vez que se lanza una nueva versión no es necesario reinstalar
    todo el sistema. Además la instalación deja únicamente lo estrictamente necesario para funcionar. El proceso de
    instalación puede ser complejo y por eso elaboré esta guía de la forma más sencilla posible intentando explicar
    qué hace cada cosa.
    Para el seguimiento de esta guía se asume que se conocen los comandos básicos de Linux tales como
    <code>ls, cd, cat, vim, nano, mkdir</code>, etc así como también un conocimiento básico sobre grupos, usuarios y
    permisos; manejo de particiones, file system, archivos e inglés básico.
</p>
<br>
<br>
<!-- Index -->

<div class="index">
  <h2>Índice</h2>
  <ul>
    <li><a href="#disclaimer">Disclaimer</a></li>
    <li><a href="#guiaoficial">Guía oficial de Instalación</a></li>
    <li><a href="#antesDeEmpezar">Antes de empezar</a></li>
    <li><a href="#boot">Preparación del medio de instalación</a></li>
    <li><a href="#preparacion">Preparándonos para la instalación</a></li>
    <li><a href="#configurandoTeclado">Configuración del Teclado</a></li>
    <li><a href="#internet">Conectándonos a Internet</a></li>
    <li><a href="#fechaYHora">Configurando la Fecha y la Hora</a></li>
    <li><a href="#particiones">Creando, formateando y montando las Particiones</a></li>
  </ul>
</div>



<div class="disclaimer">
  <h3>Disclaimer</h3>
  <p>La siguiente guía no tiene garantía alguna. Sos completamente responsable de las acciones que harás a continuación
    y corres el riesgo de perder todos los datos si no procedés con precaución.
    Lee 2 veces cada comando antes de ejecutarlo y presta atención a todo lo que el Sistema Operativo devuelve ante cada
    comando. Si pasa que la instalación toma otro rumbo desconocido, ya sea por algun error o cambio en la instalación,
    lo mejor es googlear en busca de la solución.
  </p>
</div>

<div class"guiaoficial">
  <h3>Guía Oficial</h3>
  <p>
    Es altamente recomendable tener siempre a mano la <a href=https://wiki.archlinux.org/title/installation_guide>guía
      oficial de instalación de Arch Linux en inglés</a>, que tendrá todos los últimos cambios que se hayan hecho sobre
    la instalación así como también todas las cosas que haya que tener en cuenta ante situaciones específicas.
  </p>
</div>

<div class="antesDeEmpezar">
  <h3>Antes de empezar</h3>

  <h4>Instalación en Hardware</h4>
  <p>
    Si estás en Windows y querés mantener el sistema operativo para poder entrar tanto a Windows como a Arch Linux, es
    conveniente que dejes una parte del disco sin asignar para poder instalar ahí Arch Linux. Si bien no es necesario
    mucho espacio para instalarlo es conveniente pensar de antemano cuánto espacio vas a necesitar.<br>
    Unos 80GB estarían bien.<br>
    Agarrá un lapiz y un papel y anotá el tamaño de cada partición de tu disco duro. Es probable que lo necesites más
    adelante <br>

  </p>
  <h4>Instalación sobre una Máquina Virtual</h4>
  <p>
    Si vas a realizar esta instalación sobre una máquina virtual de VMWare, es necesario configurarla para que arranque
    en modo UEFI; esto se logra añadiendo la siguiente línea al archivo .vmx de la vm:<br>
    <code>
      firmware = "efi"
    </code>
    <br><br>
    Dicho esto, empecemos.
  </p>
</div>

<div class="boot">
  <h3>Preparación del medio de instalación</h3>
  <p>
    Bien, lo primero y principal es la preparación del dispositivo booteable con la imagen del sistema que vamos a
    instalar. Para eso hay que descargarlo desde <a href="https://www.archlinux.org/download/">la página oficial de Arch
      Linux</a> y mediante un software como <a href="https://rufus.ie/en/">Rufus</a> y grabarlo en modo GPT para
    sistemas UEFI en una USB.
    <br>
    Luego hay que reiniciar la PC y seleccionar como medio de arranque el USB.
  </p>
</div>

<div class="preparacion">

  <h3>Preparación de la instalación</h3>
  <p>
    Si todo fue bien, habrás llegado a una pantalla como la siguiente:
  </p>
  <div align="center">
    <img alt="first screen" src="img/firstScreen.png" />
  </div>

  <p>
    Ahora mismo nos encontramos en un mini Sistema Operativo cargado desde el USB en la Memoria Ram de nuestra
    computadora que nos servirá para poder realizar la instalación.
  </p>

</div>
<div class="configurandoTeclado">
  <h3>Configuración de Teclado</h3>
  <p>
    Si te ganó la curiosidad y empezaste a escribir algo, te habrás dado cuenta que el teclado está distinto, y sinó
    probá escribir una <code>ñ</code> o un <code>-</code> y vas a ver que no te hace caso y pone lo que se le da la
    gana.
    <br>
    Esto es porque por defecto el teclado viene con el layout estadounidense y tenemos que cambiarlo para poder estar
    cómodos con nuestro teclado. Para eso necesitamos saber en donde vivimos (?) y qué layout corresponde.
    Esta configuración tendrá efecto sólamente durante la instalación, luego veremos cómo hacerlo en el sistema
    instalado
    <br><br>
    <b>TL;DR</b>
    <br>
    Si estás en latinoamérica el comando que tenés que ejecutar es el siguiente:
  </p>

  ```sh
  loadkeys la-latin1
  ```
  <p>
    Y para ahorrarte el trabajo, el <code>-</code> está en el lugar del <code>?</code>.
    <br>
    Para españa (tu teclado probablemente tenga alguna tecla como esta: <code>Ç</code>) el comando será:
  </p>

  ```sh
  loadkeys es
  ```

  <p>
    <br>
    Para volver al teclado estadounidense:
    <br>
  </p>

  ```sh
  loadkeys us
  ```
  <p>
    <b>Para los curiosos<b>
        <br>
        <br>
        Las distribuciones disponibles para setear las podemos listar con el comando:
  </p>

  ```sh
  ls /usr/share/kbd/keymaps/**/*.map.gz
  ```

  <p>
    y veremos algo como lo siguiente:
  </p>
  <div align="center">
    <img alt="keyboard layout" src="img/keyboardLayout.png" />
  </div>

  <p>
    Como evidentemente no entran en la pantalla, podemos copiarlos todos en un archivo y verlos más tranquilamente con:
  </p>


  ```sh
  ls /usr/share/kbd/keymaps/**/*.map.gz > /tmp/keymaps.txt
  vim /tmp/keymaps.txt
  ```

  <p>
    Te recuerdo que para salir de vim, tenés que escribir <code>:q</code> y presionar enter.
    <br><br>
    Teniendo ya el teclado configurado podemos continuar con la instalación
  </p>

</div>

<div class="internet">
  <h3>Conectándonos a internet</h3>
  <p>
    Si estás conectado por cable esto no te va a tomar mucho tiempo, podés probar hacer:
  </p>

  ```sh
  ping -c 5 archlinux.org
  ```

  <p>
    Y habrás obtenido algo similar a esto:
  </p>

  <div align="center">
    <img alt="ping archlinux.org" src="img/ping.png" />
  </div>

  <p>
    <br>
    En ese caso podés continuar a la siguiente sección. Sino quedate que vamos a configurar el WiFi.
    <br>
    El instalador de Arch Linux viene con una herramienta que nos va a servir para conectarnos al WiFi durante la
    instalación que se llama <code>iwctl</code>.
    Para acceder tenés que escribir:
  </p>

  ```sh
  iwctl
  ```
  <p>
    Y habrás entrado a una consola interactiva de iwctl en donde podemos escribir los comandos para conectarnos al WiFi.
    Para ver los adaptadores de red conectados:
  </p>

  ```sh
  device list
  ```
  <p>
    Por ejemplo, podría aparecer como dispositivo <code>wlp1s0</code> o <code>wlan0</code> o similar.
    Y para ver las redes disponibles:
  </p>

  ```sh
  station wlan0 scan
  station wlan0 get-networks
  ```
  <p>
    Reemplazá wlan0 por el nombre del dispositivo que vas a usar.<br>
    Luego de ver las redes disponibles vamos a conectarnos.<br>
    Supongamos que la red se llama <code>Batman</code> y la contraseña es <code>B4timovil</code><br>
    Entonces nos conectamos con el siguiente comando:
  </p>

  ```sh
  station wlan0 connect Batman
  ## Nos va a pedir la contraseña y pondremos
  B4timovil

  ## Para salir escribimos
  exit
  ```

  <p>
    Ahora probá hacer un <code>ping</code> como mostré antes y podemos continuar con la instalación.<br>
    Si tu placa de red no encontró ninguna red para conectarse, o tuviste problemas para conectarte te recomiendo que te
    conectes al modem por cable y sigas así con la instalación. Es probable que luego de la instalación del sistema, los
    paquetes que instalemos te permitan conectarte al WiFi.
  </p>

</div>

<div class="fechaYHora">
  <h3>Configurando la fecha y hora</h3>
  <p>
    Ahora que tenemos nuestro instalador conectado a internet, vamos a configurar la fecha y hora.<br>
    Para eso tenemos una herramienta que nos permite consultar o modificar la fecha y hora del sistema que es
    <code>timedatectl</code> y lo vamos a hacer así:
  </p>

  ```sh
  timedatectl set-ntp true
  ```
  <p>
    Con este comando ya le decimos que use internet para sincronizar la fecha y la hora.<br>
    Para comprobarlo podemos usar el comando:
  </p>

  ```sh
  timedatectl status
  ```
</div>

<div class="particiones">
  <h3>Creando las Particiones</h3>
  <p>
    <i>Ahora sí se viene lo chido...</i> diria Luisito Comunica. <br>
    Ya que tenemos la instalación del sistema preparada, vamos a crear las particiones que vamos a necesitar.
    <br>
    Si ya tenés Windows instalado, entonces ya hay particiones hechas que <b>NO TENES QUE TOCAR</b>. Entre ellas la EFI
    Partition y la partición de Windows (El disco C digamos...).<br>
    En mi caso no tengo Windows instalado y crearé las particiones de cero, pero vos ya tendrás 2 o más particiones
    asignadas.<br>
    Para listar los discos y particiones que tenemos usamos el siguiente comando:
  </p>

  ```sh
  lsblk
  ```

  <p>
    Si estás en una Máquina virtual o haciendo una instalación limpia habrás visto algo como lo siguiente:
  </p>

  <div align="center">
    <img alt="lsblk" src="img/lsblk1.png" />
  </div>

  <p>
    En cambio si estás instalando en una PC con windows instalado habrás visto algo más similar a lo siguiente:
  </p>

  <div align="center">
    <img alt="lsblk" src="img/lsblk2.png" />
  </div>

  <h4>Explicación breve</h4>
  <p>
    Los discos instalados los vamos identificar por verse como <code>sda</code>, <code>sdb</code>, etc. O en el caso de
    nvme (que es una unidad de almacenamiento ultra rápida) se verían más como <code>nvme0n1</code>.<br>
    El primer disco lo vamos a ver como sda, el segundo como sdb, y así sucesivamente. <br>
    Al mismo tiempo, cada disco puede tener sus particiones, por ejemplo, <code>sda1</code>, <code>sda2</code>,
    <code>sdb1</code>, <code>sdb2</code>, etc. <br>
    Cada partición puede organizarse internamente de distintas formas, que son los distintos File Systems. Por ejemplo
    el File System por defecto de Windows es NTFS, el de un Pen Drive es Fat32 y el de Linux ext4. Y cada File System
    puede montarse en alguna ubicación que deseemos. En el ejemplo vemos que mi partición <code>nvme0n1p6</code> está
    montada en <code>/home</code> o en el caso de la Máquina Virtual el único volumen <code>sda</code> no está montado
    en ningún lugar.
  </p>

  <h4>Preparando las particiones</h4>

  <p>
    Vamos a crear algunas particiones para instalar Arch Linux que van a ser las siguientes:
  </p>

  <h4>EFI Partition</h4>
  <p>
    Si tenés Windows instalado, esta partición ya existe y no tenés que tocarla porque sino puede que Windows no arranque.<br>
    En esta partición están los archivos necesarios para que el Sistema Operativo arranque. <br>
    Si vas a instalar Arch Linux solo, o en una Máquina Virtual, vamos a asignarle unos 550MiB de espacio.
  </p>

  <h4>Root</h4>
  <p>
    La partición root la vamos a montar en <code>/</code> y la vamos a formatear como 
    <code>ext4</code>.<br>
    Acá vamos a instalar el Sistema Operativo y todos los programas.<br><br>
    Si estás instalando sobre una PC real necesita tener por lo menos unos 25Gb de espacio (aunque puede ser más, en mi caso he necesitado hasta 50Gb).<br><br>
  </p>

  <h4>Home</h4>
  <p>
    La partición HOME la vamos a montar en <code>/home</code> y la vamos a formatear como <code>ext4</code> también.<br>
    Acá van a estar todos los archivos personales (Por ejemplo todo lo que está en el escritorio, la carpeta de descargas, fotos, videos, configuraciones, etc)<br>
    Vamos a asignarle todo el resto del espacio que nos sobre
  </p>

  <h4>Swap</h4>
  <p>
    La partición SWAP no tiene punto de montaje ni File System. <br>
    Es una partición que sirve para que el Sistema Operativo envíe los procesos que no se están ejecutando actualmente sobre todo en equipos con poca Memoria Ram.<br>
    Por regla general si tenés hasta 2GB de Ram es recomendable usar 4GB de Swap. <br>
    A partir de 4GB de Ram es conveniente tener no más de 4GB de Swap. <br>
    Si tenés una cantidad exagerada de ram podés omitir esta partición.
  </p>


  <h4>Ahora sí, creamos las particiones</h4>
  <p>Después de tanto blabla vamos a ejecutar el siguiente comando para crear las particiones:</p>

  ```sh
  cfdisk
  ```

  <p>
    Si nos pregunta qué tipo de tabla de particiones queremos le decimos que vamos a usar GPT. <br>
    Hasta este punto tendremos algo similar a esto:
  </p>

  <div align="center">
    <img alt="cfdisk" src="img/cfdisk.png" />
  </div>

  <br><br>

  <p>
    Si tenés windows instalado es CLAVE que no toques las particiones que ya están hechas y nos vamos a enfocar en aquellas que dicen <code>Free Space</code>.
    <br>
    Acá te podés mover con las flechas hacia arriba y abajo para seleccionar la partición, o izquierda y derecha para seleccionar una opción.
    <br>
    Si nos paramos en <code>Free Space</code> y seleccionamos <code>[New]</code> nos preguntará el espacio que le queremos asignar y vamos poner lo que ya anotamos antes, en mi caso voy a instalarlo en un disco de 120GB y lo haré de la siguiente manera: <br><br>
    - 550MiB para EFI Partition<br>
    - 4GB para Swap<br>
    - 25G para Root<br>
    - 90G para Home<br>
    <br><br>
    Prestá atención a cómo escribí las dimensiones. <br><br>
    Y con si seleccionamo <code>[Type]</code> vamos a poder seleccionar el tipo. <br><br>
    - EFI System para la EFI Partition<br>
    - Linux Root (x86-64) para Root<br>
    - Linux Home para Home<br>
    - Linux Swap para la Swap<br><br>
    <br><br>
    Sacale foto a como quedó, lo vas a necesitar. A mí me quedó algo así:
  </p>

  <div align="center">
    <img alt="cfdisk2" src="img/cfdisk2.png" />
  </div>

  <br><br>
  <p>
    Entonces nos queda:<br><br>
    - /dev/sda1 para EFI Partition<br>
    - /dev/sda2 para Swap<br>
    - /dev/sda3 para Root<br>
    - /dev/sda4 para Home<br><br><br>
    Finalmente le damos a <code>[Write]</code>, confirmamos la operación y salimos con <code>[Quit]</code>.
  </p>

  <h4>Formateando y Montando las particiones</h4>
  <p>
    Vamos a formatear y montar las particiones que acabamos de crear. <br>
    Como dijimos, vamos a usar ext4 como File System y vamos a formatear las particiones de Root y de Home con ese File System:
  </p>

  ```sh
  mkfs.ext4 /dev/sda3
  mkfs.ext4 /dev/sda4
  ```

  <p>
    Ahora toca formatear la partición EFI. SOLAMENTE SI LA ACABÁS DE CREAR SINO NO EJECUTES ESTE COMANDO.
  </p>
  
  ```sh
  mkfs.fat -F32 /dev/sda1
  ```

  <p>
    Por último queda formatear la partición de Swap.
  </p>

  ```sh
  mkswap /dev/sda2
  ```

  <p>
    Ahora queda montar las particiones que creamos sobre el File System del instalador para poder instalar sobre ellas el Sistema Operativo.<br>
    Lo haremos de la siguiente manera:
  </p>

  ```sh
  mount /dev/sda3 /mnt # Montamos Root sobre /mnt

  mkdir /mnt/home # Creamos la carpeta Home
  mount /dev/sda4 /mnt/home # Montamos Home sobre /mnt/home

  mkdir /mnt/boot # Creamos la carpeta Boot
  mount /dev/sda1 /mnt/boot # Montamos la partición EFI sobre /mnt/boot

  swapon /dev/sda2 # Activamos Swap
  ```

  <p>Y con esto ya tenemos creadas las particiones</p>
</div>




<br><br><br><br><br><br><br><br><br>
<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
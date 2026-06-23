## Manual Nmap

Nmap (Network Mapper) es una herramienta de código abierto indispensable para la exploración de redes y la auditoría de seguridad. Permite a los administradores de sistemas y a los profesionales de la seguridad descubrir hosts y servicios en una red, identificar sistemas operativos, y detectar vulnerabilidades. Nmap es increíblemente flexible y potente, capaz de realizar desde un simple escaneo de puertos hasta un análisis profundo de la red.

### 1\. Técnicas y Métodos de Escaneo

Nmap utiliza diversas técnicas para interactuar con los hosts y recopilar información. Comprender estas técnicas es fundamental para realizar escaneos efectivos y precisos.

  * **Ping Scan (-sP o -sn):** El método más básico para descubrir hosts activos. Nmap envía paquetes ICMP (solicitudes de eco) o paquetes ARP (en la red local) para determinar si un host está en línea. Si un host responde, se considera activo.

  * **TCP SYN Scan (Half-Open Scan) (-sS):** Es el tipo de escaneo más popular y a menudo el más rápido. Nmap envía un paquete SYN (synchronize) a un puerto.

      * Si el puerto está abierto, el objetivo responde con un paquete SYN/ACK (synchronize/acknowledge). Nmap envía un RST (reset) para cerrar la conexión antes de que se establezca por completo, de ahí el nombre "half-open". Esto lo hace sigiloso, ya que no deja registros completos en el lado del servidor.
      * Si el puerto está cerrado, el objetivo responde con un paquete RST.
      * Si no hay respuesta después de reintentos, el puerto se considera filtrado (posiblemente por un firewall).

  * **TCP Connect Scan (-sT):** Este escaneo es menos sigiloso que el SYN scan porque Nmap completa el "three-way handshake" de TCP. Nmap establece una conexión TCP completa con el puerto objetivo.

      * Si el puerto está abierto, la conexión se establece.
      * Si el puerto está cerrado, la conexión es rechazada (RST).
      * Es útil cuando el SYN scan no es posible, por ejemplo, si el usuario no tiene permisos de root para crear paquetes raw.

  * **UDP Scan (-sU):** Se utiliza para encontrar puertos UDP abiertos. UDP es un protocolo sin conexión, lo que hace que su escaneo sea más lento y a menudo menos confiable que el TCP. Nmap envía paquetes UDP a los puertos.

      * Si recibe una respuesta (a menudo un paquete UDP de vuelta), el puerto se considera abierto.
      * Si recibe un paquete ICMP "Port Unreachable", el puerto se considera cerrado.
      * Si no hay respuesta, el puerto se considera abierto|filtrado (podría ser un firewall que bloquea las respuestas).

  * **OS Detection (-O):** Nmap intenta determinar el sistema operativo del host analizando las respuestas a una serie de pruebas TCP, UDP e ICMP. Esto incluye el tamaño de la ventana TCP, las opciones de TCP, la secuencia de números de identificación IP, y el estilo del mensaje de error ICMP.

  * **Version Detection (-sV):** Nmap va un paso más allá del descubrimiento de puertos para determinar la versión del servicio que se ejecuta en un puerto abierto. Envía una serie de sondas a los puertos abiertos y analiza las respuestas para identificar el nombre y la versión del servicio (por ejemplo, Apache HTTP Server 2.4.41, OpenSSH 8.2p1).

  * **Firewall/IDS Evasion Techniques:** Nmap incorpora varias opciones para evadir la detección de firewalls y sistemas de detección de intrusos (IDS). Esto incluye la fragmentación de paquetes, el uso de proxies, el retardo entre paquetes, y la falsificación de direcciones MAC/IP.

### 2\. Comandos Detallados de Nmap con Ejemplos

A continuación, se presentan 20 comandos de Nmap, explicados detalladamente y con ejemplos tanto para objetivos locales (usando un rango de IPs comunes para LAN como 192.168.1.1/24 o tu propia IP local) como para `scanme.nmap.org`.

**Nota Importante:** Para los ejemplos con targets locales, reemplaza `192.168.1.1/24` o `192.168.1.100` con la dirección IP o rango de tu red local. Para encontrar tu IP local en Linux/macOS, usa `ip a` o `ifconfig`. En Windows, usa `ipconfig`. Para los ejemplos con `scanme.nmap.org`, ten en cuenta que este es un servidor destinado a pruebas, pero siempre úsalo con responsabilidad.

#### Comandos Básicos y de Descubrimiento

1.  **Escaneo Básico de Puertos Abiertos (TCP SYN Scan)**

      * **Comando:** `nmap -sS <target>`
      * **Descripción:** Realiza un escaneo SYN (half-open), que es rápido y menos propenso a ser detectado por firewalls básicos que el escaneo Connect. Requiere privilegios de root.
      * **Ejemplo Local:** `sudo nmap -sS 192.168.1.100` (Escanea una IP específica en tu red local)
      * **Ejemplo Web:** `sudo nmap -sS scanme.nmap.org`

2.  **Escaneo de Puertos TCP Connect (Full Connect Scan)**

      * **Comando:** `nmap -sT <target>`
      * **Descripción:** Realiza un escaneo TCP completo, estableciendo la conexión completa. Útil cuando no se tienen privilegios de root o cuando el SYN scan es bloqueado. Deja registros en el sistema destino.
      * **Ejemplo Local:** `nmap -sT 192.168.1.100`
      * **Ejemplo Web:** `nmap -sT scanme.nmap.org`

3.  **Escaneo de Puertos UDP**

      * **Comando:** `nmap -sU <target>`
      * **Descripción:** Escanea puertos UDP. Los puertos UDP son más difíciles de escanear que los TCP debido a su naturaleza sin conexión. Puede ser lento.
      * **Ejemplo Local:** `sudo nmap -sU 192.168.1.100`
      * **Ejemplo Web:** `sudo nmap -sU scanme.nmap.org`

4.  **Ping Scan (Host Discovery Only)**

      * **Comando:** `nmap -sn <target>` (anteriormente `-sP`)
      * **Descripción:** Realiza solo descubrimiento de hosts (ping). No escanea puertos. Útil para saber qué hosts están activos en una red.
      * **Ejemplo Local:** `sudo nmap -sn 192.168.1.1/24` (Descubre hosts activos en el rango de IPs)
      * **Ejemplo Web:** `nmap -sn scanme.nmap.org` (Verifica si el host está en línea)

5.  **Detección de Versión de Servicio**

      * **Comando:** `nmap -sV <target>`
      * **Descripción:** Intenta determinar la versión del servicio que se ejecuta en los puertos abiertos. Puede tomar más tiempo.
      * **Ejemplo Local:** `sudo nmap -sV 192.168.1.100`
      * **Ejemplo Web:** `sudo nmap -sV scanme.nmap.org`

6.  **Detección de Sistema Operativo**

      * **Comando:** `nmap -O <target>`
      * **Descripción:** Intenta identificar el sistema operativo del host. Requiere al menos un puerto abierto y privilegios de root para ser efectivo.
      * **Ejemplo Local:** `sudo nmap -O 192.168.1.100`
      * **Ejemplo Web:** `sudo nmap -O scanme.nmap.org`

7.  **Escaneo Agresivo (Combina -sV, -O, -A, traceroute y script scanning)**

      * **Comando:** `nmap -A <target>`
      * **Descripción:** Habilita la detección de SO, la detección de versiones, el escaneo de scripts predeterminados y el traceroute. Es un escaneo completo y ruidoso.
      * **Ejemplo Local:** `sudo nmap -A 192.168.1.100`
      * **Ejemplo Web:** `sudo nmap -A scanme.nmap.org`

#### Comandos de Selección de Puertos y Rendimiento

8.  **Escaneo de Puertos Específicos**

      * **Comando:** `nmap -p <port(s)> <target>`
      * **Descripción:** Escanea solo los puertos especificados. Puedes especificar puertos individuales (e.g., 80, 443), rangos (e.g., 1-1024), o una combinación.
      * **Ejemplo Local:** `sudo nmap -p 22,80,443 192.168.1.100`
      * **Ejemplo Web:** `sudo nmap -p 80,443 scanme.nmap.org`

9.  **Escaneo de los N Puertos Más Comunes**

      * **Comando:** `nmap --top-ports <N> <target>`
      * **Descripción:** Escanea los N puertos más comúnmente utilizados según la base de datos de Nmap.
      * **Ejemplo Local:** `sudo nmap --top-ports 20 192.168.1.100`
      * **Ejemplo Web:** `sudo nmap --top-ports 100 scanme.nmap.org`

10. **Acelerar el Escaneo (Paranoid, Sneaky, Polite, Normal, Aggressive, Insane)**

      * **Comando:** `nmap -T<0-5> <target>`
      * **Descripción:** Establece una plantilla de temporización.
          * `-T0`: Paranoid (muy lento, para evadir IDS)
          * `-T1`: Sneaky
          * `-T2`: Polite (ralentiza para evitar abrumar la red)
          * `-T3`: Normal (predeterminado)
          * `-T4`: Aggressive (más rápido, pero puede ser detectado)
          * `-T5`: Insane (el más rápido, muy ruidoso y propenso a errores)
      * **Ejemplo Local:** `sudo nmap -T4 192.168.1.1/24` (Escaneo agresivo de la red local)
      * **Ejemplo Web:** `sudo nmap -T3 scanme.nmap.org` (Escaneo normal)

11. **No Realizar Resolución de DNS Inversa**

      * **Comando:** `nmap -n <target>`
      * **Descripción:** Desactiva la resolución de DNS inversa, lo que puede acelerar el escaneo significativamente.
      * **Ejemplo Local:** `sudo nmap -n 192.168.1.1/24`
      * **Ejemplo Web:** `sudo nmap -n scanme.nmap.org`

#### Comandos de Salida y Manejo de Archivos

12. **Guardar Salida en Formato Normal**

      * **Comando:** `nmap -oN <outputfile.txt> <target>`
      * **Descripción:** Guarda la salida del escaneo en un archivo de texto plano, similar a la salida en la consola.
      * **Ejemplo Local:** `sudo nmap -oN nmap_lan_scan.txt 192.168.1.100`
      * **Ejemplo Web:** `sudo nmap -oN nmap_scanme_normal.txt scanme.nmap.org`

13. **Guardar Salida en Formato XML**

      * **Comando:** `nmap -oX <outputfile.xml> <target>`
      * **Descripción:** Guarda la salida en formato XML, útil para ser procesado por otras herramientas o scripts.
      * **Ejemplo Local:** `sudo nmap -oX nmap_lan_scan.xml 192.168.1.100`
      * **Ejemplo Web:** `sudo nmap -oX nmap_scanme_xml.xml scanme.nmap.org`

14. **Guardar Salida en Todos los Formatos (-oA)**

      * **Comando:** `nmap -oA <basename> <target>`
      * **Descripción:** Guarda la salida en los tres formatos principales: normal (`.nmap`), XML (`.xml`) y "grepable" (`.gnmap`).
      * **Ejemplo Local:** `sudo nmap -oA nmap_lan_all 192.168.1.100` (Generará `nmap_lan_all.nmap`, `nmap_lan_all.xml`, `nmap_lan_all.gnmap`)
      * **Ejemplo Web:** `sudo nmap -oA nmap_scanme_all scanme.nmap.org`

#### Comandos Avanzados y de Evasión

15. **Fragmentación de Paquetes**

      * **Comando:** `nmap -f <target>` (o `--mtu <size>`)
      * **Descripción:** Fragmenta los paquetes de escaneo en piezas más pequeñas. Esto puede ayudar a evadir firewalls que realizan inspección de paquetes.
      * **Ejemplo Local:** `sudo nmap -f 192.168.1.100`
      * **Ejemplo Web:** `sudo nmap -f scanme.nmap.org`

16. **Falsificación de Dirección MAC**

      * **Comando:** `nmap --spoof-mac <MAC_address|random|0> <target>`
      * **Descripción:** Permite falsificar la dirección MAC de la interfaz saliente. Útil para mantener el anonimato o evadir filtros MAC.
      * **Ejemplo Local:** `sudo nmap --spoof-mac 00:11:22:33:44:55 192.168.1.100`
      * **Ejemplo Web:** `sudo nmap --spoof-mac random scanme.nmap.org` (Genera una MAC aleatoria)

17. **Evitar Detección de Hosts (No Ping Scan)**

      * **Comando:** `nmap -Pn <target>`
      * **Descripción:** Asume que todos los hosts están en línea, saltándose la fase de ping scan. Útil si el host de destino está detrás de un firewall que bloquea ICMP.
      * **Ejemplo Local:** `sudo nmap -Pn 192.168.1.100`
      * **Ejemplo Web:** `sudo nmap -Pn scanme.nmap.org`

### 3\. Ejemplos Adicionales con el Uso de Scripts (Nmap Scripting Engine - NSE)

El Nmap Scripting Engine (NSE) es una de las características más potentes de Nmap. Permite a los usuarios escribir scripts (en lenguaje Lua) para automatizar una amplia variedad de tareas de red, desde la detección de vulnerabilidades hasta el descubrimiento de información avanzada.

Para usar scripts, se utiliza la opción `--script`. Los scripts se encuentran generalmente en el directorio `/usr/share/nmap/scripts/` en sistemas Linux. Puedes listar todos los scripts disponibles con `ls /usr/share/nmap/scripts/`.

18. **Escaneo de Vulnerabilidades Básicas (Vuln Script)**

      * **Comando:** `nmap --script vuln <target>`
      * **Descripción:** Ejecuta una categoría de scripts diseñados para detectar vulnerabilidades comunes. No es un escáner de vulnerabilidades completo, pero puede identificar problemas conocidos.
      * **Ejemplo Local:** `sudo nmap -sV --script vuln 192.168.1.100` (Se recomienda `-sV` para que los scripts de vulnerabilidad tengan más información)
      * **Ejemplo Web:** `sudo nmap -sV --script vuln scanme.nmap.org`

19. **Enumeración de SMB/NetBIOS (smb-enum-shares)**

      * **Comando:** `nmap --script smb-enum-shares <target>`
      * **Descripción:** Intenta enumerar los recursos compartidos de SMB en un host Windows o Samba. Útil para descubrir información sobre recursos compartidos de red.
      * **Ejemplo Local:** `sudo nmap -p 445 --script smb-enum-shares 192.168.1.100` (Asegúrate de que el puerto 445 (SMB) esté abierto)
      * **Ejemplo Web:** No aplicable directamente a `scanme.nmap.org` ya que no suele ejecutar SMB. Podrías intentarlo con un servidor Windows público si conoces uno, pero con precaución.

20. **Detección de Servidores Web (http-title, http-headers)**

      * **Comando:** `nmap -p 80,443 --script http-title,http-headers <target>`
      * **Descripción:** Los scripts `http-title` y `http-headers` obtienen el título de la página HTML y las cabeceras HTTP de los servidores web, respectivamente. Proporcionan información útil sobre el servidor web.
      * **Ejemplo Local:** `sudo nmap -p 80 --script http-title,http-headers 192.168.1.100`
      * **Ejemplo Web:** `sudo nmap -p 80,443 --script http-title,http-headers scanme.nmap.org`

21. **Escaneo de Vulnerabilidades SSL/TLS (ssl-enum-ciphers)**

      * **Comando:** `nmap -p 443 --script ssl-enum-ciphers <target>`
      * **Descripción:** Este script enumera los cifrados SSL/TLS soportados por un servidor y puede identificar vulnerabilidades como el soporte para cifrados débiles o protocolos obsoletos (SSLv2, SSLv3).
      * **Ejemplo Local:** `sudo nmap -p 443 --script ssl-enum-ciphers 192.168.1.100`
      * **Ejemplo Web:** `sudo nmap -p 443 --script ssl-enum-ciphers scanme.nmap.org`

22. **Brute-Force de Credenciales (ftp-brute)**

      * **Comando:** `nmap -p 21 --script ftp-brute --script-args userdb=/path/to/users.txt,passdb=/path/to/passwords.txt <target>`
      * **Descripción:** Este script intenta adivinar credenciales para servicios FTP utilizando listas de usuarios y contraseñas. **¡Úsalo solo en entornos controlados y con autorización explícita\!**
      * **Ejemplo Local (demostración, no ejecutar sin permiso):** `sudo nmap -p 21 --script ftp-brute --script-args userdb=users.txt,passdb=passwords.txt 192.168.1.100`
          * `users.txt` (ejemplo):
            ```
            admin
            user
            test
            ```
          * `passwords.txt` (ejemplo):
            ```
            password
            123456
            test
            ```
      * **Ejemplo Web:** No recomendado ni permitido en `scanme.nmap.org` ni en ningún sitio web sin permiso expreso.

### Consideraciones Finales

  * **Permisos:** Muchos de los escaneos de Nmap, especialmente los que utilizan paquetes raw (como SYN scan, OS detection), requieren privilegios de root (`sudo` en Linux/macOS) o ser ejecutado como administrador en Windows.
  * **Responsabilidad:** Nmap es una herramienta poderosa. Úsala de manera ética y legal. Realiza escaneos solo en redes o hosts para los que tengas autorización explícita. El escaneo no autorizado de sistemas es ilegal y puede tener graves consecuencias.
  * **Firewalls y IDS:** Ten en cuenta que los firewalls y los sistemas de detección de intrusos (IDS) pueden detectar y bloquear los escaneos de Nmap. Las técnicas de evasión pueden ayudar, pero no son infalibles.
  * **Resultados:** Interpreta los resultados de Nmap con cuidado. Un puerto reportado como "filtrado" podría significar que un firewall está bloqueando el tráfico, o que el host no está respondiendo de la manera esperada.

Este manual te proporciona una base sólida para comenzar a utilizar Nmap de manera efectiva para la exploración de redes y la evaluación de seguridad. ¡Experimenta con los comandos y sigue aprendiendo
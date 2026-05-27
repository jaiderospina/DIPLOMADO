# DIPLOMADO

Diplomado 2026
El hecho de que el error persista incluso al ejecutar el comando con `sudo` (`Cannot open database (/var/log/wtmp.db) read-only...`) confirma que el problema en Kali Linux no es únicamente una restricción de privilegios de usuario, sino un bloqueo o corrupción a nivel del motor de la base de datos de auditoría (`SQLite` / `wtmp.db`).

Cuando se invoca `last`, la herramienta intenta abrir la base de datos en modo lectura/escritura para actualizar índices o generar un archivo de bloqueo temporal (`wtmp.db-shm` o `wtmp.db-wal`). Si el sistema de archivos subyacente tiene inconsistencias, si el archivo está corrupto o si carece de los permisos de escritura correctos para el rol de superusuario (lo cual ocurre a veces en entornos virtualizados o contenedores WSL), la base de datos se bloquea en modo **read-only**, impidiendo su apertura.

---

### Soluciones Técnicas para Corregir o Evadir el Error

Se proponen tres caminos de mitigación desde la perspectiva de la administración de sistemas y la informática forense para resolver este incidente en la terminal:

#### 1. Reparación Definitiva de los Permisos y Estados de la Base de Datos

Es necesario asegurarse de que el usuario `root` tenga el control absoluto e inequívoco (lectura, escritura y ejecución) sobre el directorio de logs y la base de datos, además de limpiar posibles bloqueos previos:

```bash
# 1. Conceder permisos totales al archivo de la base de datos
sudo chmod 664 /var/log/wtmp.db

# 2. Asegurar que el propietario sea el sistema (root) y el grupo de auditoría (utmp)
sudo chown root:utmp /var/log/wtmp.db

# 3. Eliminar manualmente los archivos de bloqueo temporal de SQLite si existen
sudo rm -f /var/log/wtmp.db-shm /var/log/wtmp.db-wal

```

*Una vez ejecutados estos tres comandos, se debe probar nuevamente con `sudo last -a`.*

#### 2. Reinicialización del Archivo de Auditoría (Si existe corrupción)

Si el archivo `wtmp.db` se corrompió debido a un apagado inesperado de la máquina virtual o del contenedor, la solución más rápida para restablecer el servicio es forzar al sistema a crear una base de datos limpia y nueva:

```bash
# Mover la base de datos corrupta a un archivo de respaldo (backup)
sudo mv /var/log/wtmp.db /var/log/wtmp.db.bak

# El sistema creará automáticamente un nuevo 'wtmp.db' limpio al registrar el próximo evento o al invocar el comando.
sudo last -a

```

*Nota: Al hacer esto, el historial antiguo se archivará en el `.bak`, y `last` comenzará a registrar las sesiones a partir de este preciso instante.*

#### 3. Alternativa Forense: Extracción Directa desde `Systemd-Journald`

Dado que las distribuciones modernas de Linux gestionan los inicios de sesión a través de `PAM` (Pluggable Authentication Modules) y los unifican en los diarios binarios del sistema, se puede evadir por completo la herramienta `last` e interrogar directamente al demonio de registro (`journalctl`).

Este comando es infalible y extraerá el historial de sesiones directamente de las bitácoras vivas del kernel:

```bash
sudo journalctl _SYSTEMD_UNIT=systemd-logind.service | grep -E "New session|Removed session"

```

#### 4. Consulta del Historial de Autenticación de Emergencia

Si se requiere ver las direcciones IP y las terminales de las últimas conexiones de forma inmediata para fines de auditoría, el archivo de texto plano tradicional sigue estando disponible y es inmune a los errores de bases de datos:

```bash
sudo cat /var/log/auth.log | grep -E "accepted|session opened"

```

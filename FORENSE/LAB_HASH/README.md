# Guía práctica de evaluación de valores hash y checksum

Esta guía paso a paso permite comprobar cómo funcionan los algoritmos hash y checksum en Windows y Kali Linux, y cómo un cambio mínimo en un archivo produce un valor completamente diferente. También muestra por qué estas técnicas se usan para verificar integridad de datos, descargas y evidencias digitales. 

## 1. Introducción conceptual breve

Un **hash** es el resultado de aplicar una función matemática a un archivo o texto para obtener una cadena de longitud fija, como ocurre con MD5, SHA-1 o SHA-256. Un **checksum** es una suma de verificación usada para detectar cambios o errores en datos, generalmente con algoritmos más simples que los hashes criptográficos. 

La diferencia principal es que un hash criptográfico se usa para integridad y seguridad, mientras que un checksum tradicional se usa más para detectar errores accidentales en transmisión o almacenamiento. Ambos ayudan a detectar modificaciones, pero los hashes criptográficos ofrecen mejores propiedades frente a manipulación y colisiones que los checksums simples. 


### 1. Introducción conceptual

**¿Qué es un hash?**

Un **hash criptográfico** es un valor de longitud fija generado a partir del contenido de un archivo mediante un algoritmo matemático (MD5, SHA-1, SHA-256, SHA-512, etc.).

**¿Qué es un checksum?**

Un **checksum** es un valor de verificación utilizado para detectar errores de transmisión o almacenamiento. Puede ser simple (CRC32) o criptográfico.

Diferencias principales

| Hash criptográfico                   | Checksum                           |
| ------------------------------------ | ---------------------------------- |
| Diseñado para integridad y seguridad | Diseñado para detección de errores |
| Difícil de falsificar                | Más simple de calcular             |
| Ej.: SHA-256                         | Ej.: CRC32                         |

Importancia en integridad de datos

* Verificar descargas.

* Comprobar evidencia digital en informática forense.

* Detectar modificaciones no autorizadas.

* Validar copias de seguridad.

## 2. Herramientas necesarias

### Windows

- `certutil` para calcular hashes desde la consola. 
- `Get-FileHash` en PowerShell para calcular hashes con distintos algoritmos. 
- Bloc de notas para crear y modificar archivos.
- Opcional: herramientas gráficas públicas como HashCalc, QuickHash o MultiHasher para comparar resultados visualmente.

### Kali Linux

- `md5sum` para MD5. 
- `sha1sum` para SHA-1. 
- `sha256sum` para SHA-256. 
- `sha512sum` para SHA-512.
- `cksum` para checksum clásico. 
- `nano` para editar archivos desde terminal.

## 3. Preparación del entorno

El estudiante debe crear una carpeta de trabajo en ambos sistemas para almacenar los archivos de la práctica.

### Windows

En Símbolo del sistema:

```cmd
mkdir C:\hash_lab
```

### Kali Linux

En terminal:

```bash
mkdir -p ~/hash_lab
```

**Captura sugerida:**

- Captura 1: creación de la carpeta `hash_lab` en Windows.
- Captura 2: creación de la carpeta `hash_lab` en Kali Linux.

## 4. Ejercicio 1: cálculo básico de hash

**Objetivo:** calcular el hash de un archivo de texto con varios algoritmos.

### Windows

1. Abrir Bloc de notas.
2. Escribir el siguiente contenido:

```text
Este es mi primer archivo para pruebas de hash.
```

3. Guardar el archivo como `C:\hash_lab\ejercicio1.txt`.
4. Abrir Símbolo del sistema y ejecutar:

```cmd
certutil -hashfile C:\hash_lab\ejercicio1.txt MD5
certutil -hashfile C:\hash_lab\ejercicio1.txt SHA1
certutil -hashfile C:\hash_lab\ejercicio1.txt SHA256
```

5. Abrir PowerShell y ejecutar:

```powershell
Get-FileHash C:\hash_lab\ejercicio1.txt -Algorithm MD5
Get-FileHash C:\hash_lab\ejercicio1.txt -Algorithm SHA1
Get-FileHash C:\hash_lab\ejercicio1.txt -Algorithm SHA256
```

### Kali Linux

1. Abrir terminal.
2. Ejecutar:

```bash
nano ~/hash_lab/ejercicio1.txt
```

3. Escribir el mismo contenido:

```text
Este es mi primer archivo para pruebas de hash.
```

4. Guardar el archivo.
5. Ejecutar:

```bash
cd ~/hash_lab
md5sum ejercicio1.txt
sha1sum ejercicio1.txt
sha256sum ejercicio1.txt
```

**Capturas sugeridas:**

- Captura 3: contenido del archivo en Windows.
- Captura 4: hashes en cmd.
- Captura 5: hashes en PowerShell.
- Captura 6: contenido del archivo en `nano`.
- Captura 7: hashes en Kali Linux.

**Conclusión esperada:** el mismo archivo produce un valor distinto para cada algoritmo, pero siempre el mismo valor si el contenido no cambia. 

## 5. Ejercicio 2: sensibilidad a un cambio de una letra

**Objetivo:** demostrar que cambiar una sola letra altera completamente el hash.

### Windows

1. Abrir `C:\hash_lab\ejercicio1.txt`.
2. Guardar una copia como `C:\hash_lab\ejercicio1_mod1.txt`.
3. Cambiar el texto de:

```text
Este es mi primer archivo para pruebas de hash.
```

a:

```text
Este es mi primer archivo para pruebas de hashes.
```

4. Ejecutar:

```cmd
certutil -hashfile C:\hash_lab\ejercicio1.txt MD5
certutil -hashfile C:\hash_lab\ejercicio1_mod1.txt MD5
certutil -hashfile C:\hash_lab\ejercicio1.txt SHA256
certutil -hashfile C:\hash_lab\ejercicio1_mod1.txt SHA256
```

### Kali Linux

1. Ejecutar:

```bash
cd ~/hash_lab
cp ejercicio1.txt ejercicio1_mod1.txt
nano ejercicio1_mod1.txt
```

2. Cambiar la misma letra y guardar.
3. Ejecutar:

```bash
md5sum ejercicio1.txt ejercicio1_mod1.txt
sha256sum ejercicio1.txt ejercicio1_mod1.txt
```

**Capturas sugeridas:**

- Captura 8: archivo original y archivo modificado.
- Captura 9: comparación de MD5 en Windows.
- Captura 10: comparación de SHA-256 en Windows.
- Captura 11: comparación de hashes en Kali.

**Conclusión esperada:** un cambio mínimo genera un hash completamente diferente, lo que evidencia el efecto avalancha de los algoritmos hash. 

## 6. Ejercicio 3: sensibilidad a un espacio adicional

**Objetivo:** demostrar que incluso un espacio invisible modifica el hash.

### Windows

1. Abrir `C:\hash_lab\ejercicio1.txt`.
2. Guardar una copia como `C:\hash_lab\ejercicio1_mod2.txt`.
3. Agregar un espacio al final de la línea.
4. Ejecutar:

```cmd
certutil -hashfile C:\hash_lab\ejercicio1.txt SHA256
certutil -hashfile C:\hash_lab\ejercicio1_mod2.txt SHA256
```

### Kali Linux

1. Ejecutar:

```bash
cd ~/hash_lab
cp ejercicio1.txt ejercicio1_mod2.txt
nano ejercicio1_mod2.txt
```

2. Agregar un espacio al final.
3. Ejecutar:

```bash
sha256sum ejercicio1.txt ejercicio1_mod2.txt
```

**Capturas sugeridas:**

- Captura 12: edición del archivo con el espacio agregado.
- Captura 13: comparación de SHA-256 en ambos sistemas.

**Conclusión esperada:** aunque visualmente el cambio parezca insignificante, el hash cambia por completo. 
## 7. Ejercicio 4: sensibilidad a un símbolo adicional

**Objetivo:** demostrar que agregar un signo o carácter especial altera totalmente el resultado.

### Windows

1. Crear una copia llamada `C:\hash_lab\ejercicio1_mod3.txt`.
2. Agregar un carácter al final, por ejemplo `!`.
3. Ejecutar:

```cmd
certutil -hashfile C:\hash_lab\ejercicio1_mod3.txt MD5
certutil -hashfile C:\hash_lab\ejercicio1_mod3.txt SHA1
certutil -hashfile C:\hash_lab\ejercicio1_mod3.txt SHA256
```

### Kali Linux

1. Ejecutar:

```bash
cd ~/hash_lab
cp ejercicio1.txt ejercicio1_mod3.txt
nano ejercicio1_mod3.txt
```

2. Agregar `!` al final.
3. Ejecutar:

```bash
md5sum ejercicio1_mod3.txt
sha1sum ejercicio1_mod3.txt
sha256sum ejercicio1_mod3.txt
```

**Capturas sugeridas:**

- Captura 14: modificación con símbolo especial.
- Captura 15: hashes del archivo con símbolo en Windows.
- Captura 16: hashes del archivo con símbolo en Kali.

**Conclusión esperada:** cualquier carácter nuevo cambia completamente el valor generado por cada algoritmo. 

## 8. Ejercicio 5: comparación entre varios algoritmos

**Objetivo:** observar que cada algoritmo produce una salida distinta para el mismo archivo.

### Windows

```powershell
Get-FileHash C:\hash_lab\ejercicio1.txt -Algorithm MD5
Get-FileHash C:\hash_lab\ejercicio1.txt -Algorithm SHA1
Get-FileHash C:\hash_lab\ejercicio1.txt -Algorithm SHA256
Get-FileHash C:\hash_lab\ejercicio1.txt -Algorithm SHA512
```

### Kali Linux

```bash
cd ~/hash_lab
md5sum ejercicio1.txt
sha1sum ejercicio1.txt
sha256sum ejercicio1.txt
sha512sum ejercicio1.txt
```

**Capturas sugeridas:**

- Captura 17: resultados en PowerShell.
- Captura 18: resultados en Kali.

**Conclusión esperada:** cada algoritmo tiene una salida diferente, con distinta longitud y formato, pero todos dependen del contenido exacto del archivo. 

## 9. Ejercicio 6: checksum clásico frente a hash criptográfico

**Objetivo:** comparar checksum y hash sobre el mismo archivo.

### Kali Linux

Ejecutar:

```bash
cd ~/hash_lab
cksum ejercicio1.txt
cksum ejercicio1_mod1.txt
sha256sum ejercicio1.txt
sha256sum ejercicio1_mod1.txt
```

### Windows

Windows no incluye `cksum` como herramienta nativa habitual, por lo que la comparación práctica puede centrarse en hashes con `certutil` y la explicación conceptual del checksum. 

```cmd
certutil -hashfile C:\hash_lab\ejercicio1.txt SHA256
certutil -hashfile C:\hash_lab\ejercicio1_mod1.txt SHA256
```

**Capturas sugeridas:**

- Captura 19: salida de `cksum` en Kali.
- Captura 20: salida de SHA-256 en Windows.

**Conclusión esperada:** tanto checksum como hash detectan cambios, pero el hash criptográfico ofrece mejores propiedades de seguridad e integridad que un checksum simple.

## 10. Ejercicio 7: verificación de integridad de una descarga

**Objetivo:** validar la integridad de un archivo descargado comparando su hash con el valor publicado por el proveedor.

### Windows

1. Descargar un archivo cuya página publique el valor SHA-256 oficial. 
2. Guardarlo en `C:\hash_lab`.
3. Ejecutar:

```cmd
certutil -hashfile C:\hash_lab\nombre_del_archivo.iso SHA256
```

4. Comparar visualmente el resultado con el hash oficial publicado.

### Kali Linux

1. Descargar el mismo archivo en `~/hash_lab`.
2. Ejecutar:

```bash
cd ~/hash_lab
sha256sum nombre_del_archivo.iso
```

3. Comparar el valor calculado con el valor oficial.

**Capturas sugeridas:**

- Captura 21: página donde aparece el hash oficial.
- Captura 22: cálculo del hash en Windows.
- Captura 23: cálculo del hash en Kali.

**Conclusión esperada:** si el valor calculado coincide con el publicado, el archivo conserva su integridad; si no coincide, el archivo puede estar corrupto o modificado. 

## 11. Formato de evidencias que el estudiante debe entregar

El estudiante debe reunir evidencia organizada y numerada por ejercicio.

### Evidencias mínimas

- Capturas de pantalla numeradas de cada paso.
- Comandos ejecutados, copiados exactamente como fueron usados.
- Valores hash originales y modificados colocados lado a lado.
- Conclusión breve por ejercicio.

### Tabla sugerida para registrar resultados

| Archivo | MD5 | SHA-1 | SHA-256 | Observación |
|---------|-----|-------|---------|-------------|
| ejercicio1.txt | valor obtenido | valor obtenido | valor obtenido | archivo original |
| ejercicio1_mod1.txt | valor obtenido | valor obtenido | valor obtenido | cambio de una letra |
| ejercicio1_mod2.txt | valor obtenido | valor obtenido | valor obtenido | espacio adicional |
| ejercicio1_mod3.txt | valor obtenido | valor obtenido | valor obtenido | símbolo adicional |

## 12. Checklist final de entrega

- El estudiante creó la carpeta de trabajo en Windows y Kali.
- El estudiante creó el archivo original.
- El estudiante calculó MD5, SHA-1 y SHA-256 del archivo original.
- El estudiante creó al menos tres variantes del archivo original.
- El estudiante comparó los hashes del archivo original y las variantes.
- El estudiante ejecutó al menos un checksum clásico en Kali con `cksum`. [
- El estudiante verificó la integridad de una descarga con SHA-256. 
- El estudiante reunió capturas de pantalla numeradas.
- El estudiante registró todos los comandos ejecutados.
- El estudiante redactó conclusiones por cada ejercicio.

## 13. Observaciones finales para la práctica

Esta práctica permite observar de forma directa que los algoritmos hash son extremadamente sensibles a cualquier alteración en el contenido. Esa propiedad los convierte en una herramienta fundamental para verificar integridad de archivos, validar descargas, apoyar análisis forense y detectar modificaciones no autorizadas. 


## EVIDENCIAS
Subir de manera individualun documento pdf con las evidencias del ejercicio y la nomenclatura.

- **NombreApellido_lab_HASH**

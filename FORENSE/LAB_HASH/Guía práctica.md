Guía práctica

Windows + Kali Linux

### Guía práctica: Evaluación de valores Hash y Checksum

Objetivo general: demostrar de forma práctica cómo los algoritmos hash y checksum permiten verificar la integridad de archivos y cómo una modificación mínima produce un valor completamente diferente.

### 1. Introducción conceptual

¿Qué es un hash?

Un hash criptográfico es un valor de longitud fija generado a partir del contenido de un archivo mediante un algoritmo matemático (MD5, SHA-1, SHA-256, SHA-512, etc.).

¿Qué es un checksum?

Un checksum es un valor de verificación utilizado para detectar errores de transmisión o almacenamiento. Puede ser simple (CRC32) o criptográfico.

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

### 2. Herramientas necesarias

| Windows                                                                                 | Kali Linux                                        |
| --------------------------------------------------------------------------------------- | ------------------------------------------------- |
| certutil (incluido en Windows)PowerShell Get-FileHash7-Zip (opcional)HashTab (opcional) | md5sumsha1sumsha256sumcksumhashcat (demostrativo) |

### 3. Ejercicio 1: Calcular MD5 y SHA-256 de un archivo

### Objetivo

Básico

Calcular y comparar valores hash usando dos algoritmos diferentes.

### Windows

1. Crear el archivo

2. Calcular MD5

3. Calcular SHA-256

Captura esperada

CMD o PowerShell mostrando ambos hashes

### Kali Linux

1. Crear el archivo

2. Calcular MD5

3. Calcular SHA-256

Captura esperada

Terminal con los dos comandos y resultados

### Conclusión esperada

El mismo archivo genera valores distintos para MD5 y SHA-256 porque son algoritmos diferentes.

### 4. Ejercicio 2: Sensibilidad del hash (cambio de una letra)

### Objetivo

Sensibilidad

Demostrar que cambiar una sola letra altera completamente el hash.

### Windows

1. Crear archivo original

2. Obtener hash

3. Modificar una letra

4. Obtener nuevo hash

### Kali Linux

1. Crear archivo original

2. Obtener hash

3. Modificar una letra

4. Obtener nuevo hash

### Comparación esperada

| Archivo        | SHA-256         |
| -------------- | --------------- |
| original.txt   | a1b2c3d4e5f6... |
| modificado.txt | 9f8e7d6c5b4a... |

Observación clave: aunque solo se agregó una letra, el hash cambió completamente.

### 5. Ejercicio 3: Sensibilidad del hash (agregar un espacio)

### Objetivo

Integridad

Verificar que un espacio en blanco también modifica el resultado.

### Windows

### Kali Linux

### Conclusión esperada

Un espacio adicional es suficiente para producir un hash totalmente diferente.

### 6. Ejercicio 4: Verificación de integridad de una copia

### Objetivo

Verificación

Comprobar que una copia idéntica mantiene el mismo hash.

### Windows

### Kali Linux

Resultado esperado

Los dos hashes deben ser idénticos.

### 7. Ejercicio 5: Uso de checksum (CRC32 / cksum)

### Objetivo

Checksum

Comparar un checksum con un hash criptográfico.

### Windows (PowerShell)

### Kali Linux

### Conclusión esperada

El cksum devuelve un valor de verificación y el tamaño del archivo; SHA-256 proporciona una verificación criptográfica mucho más robusta.

### 8. Evidencias que debe entregar el estudiante

### Capturas obligatorias

Numeradas

* Captura 1: creación del archivo original.

* Captura 2: cálculo del hash inicial.

* Captura 3: modificación del archivo.

* Captura 4: cálculo del hash modificado.

* Captura 5: comparación visual de ambos valores.

* Captura 6: verificación de la copia idéntica.

### Tabla de resultados sugerida

| Archivo        | Algoritmo | Hash / Checksum |
| -------------- | --------- | --------------- |
| original.txt   | SHA-256   | ...             |
| modificado.txt | SHA-256   | ...             |
| datos1.txt     | SHA-1     | ...             |
| datos2.txt     | SHA-1     | ...             |
| archivo.txt    | cksum     | ...             |

### 9. Conclusiones por ejercicio

| Ejercicio | Conclusión                                                                              |
| --------- | --------------------------------------------------------------------------------------- |
| 1         | Un mismo archivo produce hashes diferentes según el algoritmo utilizado.                |
| 2         | Una sola letra cambia completamente el hash.                                            |
| 3         | Un espacio adicional también altera totalmente el hash.                                 |
| 4         | Las copias idénticas mantienen exactamente el mismo hash.                               |
| 5         | Los checksums detectan errores, pero los hashes criptográficos ofrecen mayor seguridad. |

### 10. Checklist final de entrega

### Evidencias mínimas requeridas

Completo

* ☐ Captura del archivo original.

* ☐ Captura del hash inicial (MD5 o SHA-256).

* ☐ Captura del archivo modificado.

* ☐ Captura del hash modificado.

* ☐ Tabla comparativa de hashes.

* ☐ Captura de verificación de copia idéntica.

* ☐ Captura del uso de cksum o equivalente.

* ☐ Conclusión individual de cada ejercicio.

* ☐ Conclusión general sobre integridad de datos.

Conclusión general esperada del estudiante

Los algoritmos hash son extremadamente sensibles a cualquier modificación del contenido. Cambiar una sola letra, un espacio o un carácter genera un valor hash completamente diferente, lo que permite detectar alteraciones con alta confiabilidad y constituye un mecanismo fundamental para la verificación de integridad en ciberseguridad e informática forense.


## EVIDENCIAS

Subir de manera individualun documento pdf con las evidencias del ejercicio y la nomenclatura.


- **NombreApellido_lab_HASH**
# Documentación de Pruebas en Entorno Controlado (Sandboxing)

**Nombre del Alumno:** Iván Muriel Vadillo
**Asignatura:** Puesta en Producción Segura
**Unidad:** 1

## 1. Introducción
El objetivo de esta práctica es ejecutar una aplicación en un entorno aislado (sandbox) para limitar su acceso al resto del sistema. Para ello utilizaremos la herramienta **Firejail** junto con su interfaz gráfica **Firetools**. La aplicación a probar es un script en Python (`notas.py` o `lavadero.py`).

Los lenguajes interpretados como Python requieren de un intérprete instalado en el entorno para funcionar. Firejail permite compartir binarios del sistema (como python3) mientras aísla carpetas personales.

## 2. Preparación del entorno

### 2.1. Instalación de herramientas
Primero, aseguramos que Firejail y Firetools están instalados en el sistema Linux.
Comando utilizado:
```bash
    sudo apt update
    sudo apt install firejail firetools
```
### 2.2. Preparación de la aplicación
Se ha descargado el archivo `Archivador.zip` que contiene la aplicación. Tras descomprimirlo, verificamos la estructura del directorio para localizar el código fuente.

**Estructura de archivos:**
Como se observa en la siguiente captura, la aplicación cuenta con una carpeta `src` donde reside el código fuente y un archivo `requeriments.txt` para las dependencias.

![Listado de archivos descomprimidos](image_44849f.png)

## 3. Ejecución de la aplicación en el Sandbox
Para ejecutar la aplicación de forma aislada, utilizamos el comando `firejail` seguido del intérprete y el script.

**Comando de ejecución:**
```bash
    firejail python3 src/notas/main.py
```
*(Nota: La ruta `src/notas/main.py` debe ajustarse según la ubicación real del archivo principal dentro de la carpeta `src`).*

Al ejecutar este comando, Firejail:

* Carga los perfiles de seguridad predeterminados (restringiendo acceso a `/etc`, `/var`, etc.).
* Crea un contenedor ligero alrededor del proceso.
* Inicia la aplicación Python dentro de este contenedor.

### 3.1. Verificación del Aislamiento
Para confirmar que el aislamiento está activo, abrimos una segunda terminal y consultamos la lista de procesos gestionados por Firejail:

```bash
    firejail --list
    firejail --tree
```
Estas herramientas nos muestran el PID del proceso confinado y el usuario bajo el que se está ejecutando.

## 4. Incidencias encontradas y solución

### 4.1. El problema de la ejecución como root
Durante el primer intento de ejecución, se intentó lanzar el sandbox estando logueado como el superusuario **root** (`root@kali`).

**Error obtenido:**
El sistema devolvió el error: `python3: can't open file ... [Erro 2] No such file or directory`, a pesar de que la ruta parecía correcta.

**Análisis del problema:**
Ejecutar Firejail como root conlleva varios problemas:

* **Restricciones de seguridad:** Firejail está diseñado para proteger el sistema. Al ejecutarlo como root, el comportamiento del sandbox cambia y puede imponer restricciones más severas o cambiar el directorio de trabajo virtual, impidiendo que Python localice el archivo en la ruta relativa especificada.
* **Malas prácticas:** En un entorno de *Puesta en Producción Segura*, nunca se deben ejecutar aplicaciones estándar con privilegios de administrador innecesariamente. Si la aplicación dentro del sandbox lograra escapar (vulnerabilidad de *sandbox escape*), tendría permisos de root en el sistema host.

## 5. Conclusiones
El uso de **Firejail** proporciona una capa de seguridad crítica y fácil de implementar. Hemos comprobado que el aislamiento no solo depende de la herramienta, sino del uso correcto de los privilegios de usuario (evitando `root`).

Esta práctica demuestra cómo proteger el sistema operativo base de posibles vulnerabilidades o código malicioso presente en aplicaciones de terceros escritas en lenguajes interpretados como Python.
# PPSUnidad1-ActividadSandboxing

## 📋 Descripción
Este repositorio contiene la entrega práctica correspondiente a la **Unidad 1** del módulo **Puesta en Producción Segura**.

El objetivo principal de la actividad es demostrar el uso de técnicas de **Sandboxing** (aislamiento de procesos) para ejecutar aplicaciones de terceros de forma segura, mitigando el riesgo de que vulnerabilidades o código malicioso afecten al sistema operativo anfitrión.

## 👤 Datos del Alumno
* **Nombre:** Iván Muriel
* **Asignatura:** Puesta en Producción Segura (PPS)
* **Unidad:** 1 - Seguridad en el desarrollo de software

## 📂 Contenido del Repositorio

Este repositorio se estructura de la siguiente manera:

1.  **`Reflexion.md`**:
    * Documento teórico que analiza las características de seguridad de lenguajes de bajo nivel (C/C++) frente a lenguajes gestionados (Java) y modernos (Rust).
    * Incluye una reflexión sobre la importancia de los Entornos de Desarrollo Integrados (IDE) en la seguridad preventiva.

2.  **`Documentacion_Sandboxing.md`**:
    * Guía paso a paso de la práctica realizada con **Firejail**.
    * Detalla el proceso de instalación, ejecución y verificación del aislamiento.
    * Incluye la resolución de incidencias relacionadas con los privilegios de usuario (ejecución como *root* vs usuario estándar).

3.  **Código Fuente (`src/`)**:
    * Contiene la aplicación en Python (`notas`) utilizada como objeto de prueba para el entorno aislado.

## 🛠️ Herramientas Utilizadas

* **Sistema Operativo:** Kali Linux
* **Firejail:** Herramienta de SUID sandbox para Linux.
* **Firetools:** Interfaz gráfica para gestión y monitorización de sandboxes.
* **Python 3:** Intérprete para la aplicación de prueba.

## 🚀 Instrucciones de Ejecución

Para replicar el aislamiento documentado en este repositorio, se utiliza el siguiente comando desde la raíz del proyecto (como usuario estándar):

```bash
  firejail python3 src/notas/main.py
```

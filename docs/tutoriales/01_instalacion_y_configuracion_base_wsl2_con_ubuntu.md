<p align="center">
	<img src="../../assets/logos/fpuna-corporativo-two.png" alt="Logo corporativo FP-UNA" width="220">
</p>

# Tutorial paso a paso: Instalación y configuración base de WSL2 con Ubuntu
**Sistema Operativo Windows 11 – Despliegue de Windows Subsystem for Linux y Ubuntu 22.04.5 LTS para uso académico en laboratorios de Big Data e Ingeniería de Datos**

---

## Datos institucionales

**Institución:** Universidad Nacional de Asunción  
**Unidad Académica:** Facultad Politécnica  
**Dependencia Académica:** Departamento de Enseñanza de Informática  
**Asignatura:** Electiva - Big Data  
**Carreras:** Ingeniería en Informática / Licenciatura en Ciencias Informáticas  
**Turno / Sección:** T / TQ  
**Docente Encargado:** Prof. Ing. Richard Daniel Jiménez Riveros  
**Contacto Institucional:** rjimenez@pol.una.py

---

## Autor del documento

**Prof. Ing. Richard Daniel Jiménez Riveros**  
Ingeniero en Informática  
Docente de la asignatura *Electiva - Big Data*  
Facultad Politécnica - Universidad Nacional de Asunción  

---

## Fecha y versión

- **Fecha:** 19/03/2026
- **Versión:** 1.0

---

## Nota sobre esta documentación

Este tutorial fue ajustado a partir de una versión preliminar utilizada en pruebas de concepto del entorno académico de laboratorio del curso. Los pasos están orientados a una instalación moderna de **WSL2** con **Ubuntu** sobre **Windows 11** y también son aplicables, con pequeñas variaciones, a equipos con **Windows 10 versión 2004 o superior**.

Aunque el procedimiento está validado y es estable, no todos los equipos responden exactamente igual. Diferencias en BIOS/UEFI, virtualización, políticas institucionales, versiones de Windows, drivers o software de seguridad pueden requerir pasos adicionales. Si aparece un error, no conviene forzar soluciones al azar: primero hay que verificar la causa técnica, revisar los mensajes del sistema y contrastar con la documentación oficial.

---

## Tabla de contenido

1. [Introducción](#1-introducción)  
2. [Objetivos](#2-objetivos)  
3. [Alcance del tutorial](#3-alcance-del-tutorial)  
4. [Contexto técnico](#4-contexto-técnico)  
5. [Requisitos previos](#5-requisitos-previos)  
6. [Arquitectura o flujo de referencia](#6-arquitectura-o-flujo-de-referencia)  
7. [Convenciones usadas en el documento](#7-convenciones-usadas-en-el-documento)  
8. [Procedimiento paso a paso](#8-procedimiento-paso-a-paso)  
9. [Validación del resultado](#9-validación-del-resultado)  
10. [Problemas frecuentes y soluciones](#10-problemas-frecuentes-y-soluciones)  
11. [Buenas prácticas](#11-buenas-prácticas)  
12. [Conclusión](#12-conclusión)  
13. [Referencias](#13-referencias)  

---

## 1. Introducción

En esta asignatura, WSL2 cumple un papel clave porque permite trabajar con un entorno Linux real dentro de Windows sin recurrir a dual boot ni a una máquina virtual tradicional. Esto facilita la preparación del stack de trabajo para ingeniería de datos, automatización, orquestación, uso de terminal, Git, Python, PostgreSQL, Airflow y demás herramientas que se desplegarán durante el curso.

El objetivo no es solamente “instalar Linux”, sino construir una base de trabajo reproducible, ordenada y útil para laboratorios, ensayos y proyecto final.

---

## 2. Objetivos

Al finalizar este tutorial, el estudiante podrá:

- Verificar si su equipo cumple los requisitos mínimos para ejecutar WSL2.
- Instalar WSL con Ubuntu usando el método recomendado por Microsoft.
- Realizar la configuración inicial del entorno Linux.
- Ejecutar comandos básicos de administración de WSL desde PowerShell.
- Ejecutar comandos básicos de uso cotidiano dentro de Ubuntu.
- Aplicar buenas prácticas de organización para el trabajo posterior con Git, Python y herramientas del stack del curso.

---

## 3. Alcance del tutorial

**Herramienta / componente principal:** Windows Subsystem for Linux 2 (WSL2) con Ubuntu  
**Versión objetivo:** Ubuntu 22.04.5 LTS sobre Windows 11, aplicable con pequeñas variaciones a Windows 10 versión 2004 o superior  
**Sistema operativo base:** Windows 11 / Windows 10 2004 o superior  
**Entorno de trabajo:** Consola PowerShell / Windows Terminal + terminal Ubuntu  
**IDE / cliente sugerido:** PowerShell, Windows Terminal y consola Ubuntu  
**Tipo de uso:** instalación, configuración base, verificación inicial y administración básica  

### Este tutorial cubre

- la instalación recomendada de WSL2 con Ubuntu;
- la configuración inicial del entorno Ubuntu;
- comandos básicos de administración de WSL y de uso cotidiano dentro de Ubuntu.

### Este tutorial no cubre

- la instalación de Python, PostgreSQL, Git o Airflow;
- la configuración avanzada de entornos de desarrollo para proyectos específicos;
- la automatización o endurecimiento avanzado del sistema.

---

## 4. Contexto técnico

### 4.1 ¿Qué es WSL y por qué se usa en esta asignatura?

**WSL (Windows Subsystem for Linux)** es una característica de Windows que permite ejecutar distribuciones Linux dentro del sistema operativo Windows. En este curso se utilizará porque:

- facilita el uso de herramientas de línea de comandos propias del ecosistema Linux;
- mejora la compatibilidad con stacks de desarrollo y datos;
- permite trabajar con mayor cercanía a entornos reales de despliegue;
- reduce la fricción para instalar herramientas que en Windows suelen requerir más ajustes.

### 4.2 Conceptos básicos

- **Distribución o distro:** sistema Linux concreto que se instala dentro de WSL, por ejemplo Ubuntu o Debian.
- **WSL 1:** primera generación del subsistema.
- **WSL 2:** versión recomendada actualmente; ofrece mayor compatibilidad con Linux al ejecutar un kernel Linux real.
- **Terminal:** interfaz de comandos desde la cual se administran Windows y Linux. En Windows se suele usar PowerShell o Windows Terminal; dentro de Linux se utiliza la consola de la distribución instalada.

---

## 5. Requisitos previos

## 5.1 Requisitos mínimos del sistema

Para seguir el método de instalación recomendado, el equipo debe cumplir al menos con uno de estos escenarios:

- **Windows 11**
- **Windows 10 versión 2004 o superior (Build 19041 o superior)**

## 5.2 Requisitos prácticos recomendados

- Permisos de administrador en Windows
- Conexión a Internet
- Espacio libre en disco
- Virtualización habilitada en BIOS/UEFI
- Windows actualizado

## 5.3 Recomendación operativa para la asignatura

Antes de instalar herramientas adicionales del stack, conviene dejar WSL funcionando correctamente, actualizar Ubuntu y verificar el acceso a Internet desde la terminal Linux. No avances a PostgreSQL, Python, Git o Airflow si esta base no quedó estable.

### Verificaciones rápidas

```powershell
wsl --version
wsl --status
wsl --list --verbose
```

---

## 6. Arquitectura o flujo de referencia

El flujo lógico de este tutorial puede resumirse de la siguiente manera:

- verificar compatibilidad del equipo y estado inicial de WSL en Windows;
- instalar WSL2 y desplegar Ubuntu;
- completar la configuración inicial del entorno Linux;
- validar conectividad, versión y funcionamiento básico;
- usar WSL como base para continuar con el stack técnico de la asignatura.

---

## 7. Convenciones usadas en el documento

- `comando` → instrucción a ejecutar en terminal, consola o CLI.
- `ruta/archivo` → ruta absoluta o relativa dentro del entorno del proyecto.
- `<usuario>` → dato que debe ser reemplazado por el usuario.
- `# comentario` → explicación breve dentro de un bloque de ejemplo.
- `powershell`, `bash`, `ini`, `text` → lenguajes o formatos utilizados en ejemplos.

---

## 8. Procedimiento paso a paso

### Paso 1 — Verificación inicial del equipo

## 8.1 Verificación inicial del equipo

## 8.1.1 Verificar la versión de Windows

Presiona `Win + R`, escribe:

```text
winver
```

Verifica que tu equipo tenga **Windows 10 2004+** o **Windows 11**.

## 8.1.2 Verificar si la virtualización está habilitada

Abre el **Administrador de tareas** → pestaña **Rendimiento** → sección **CPU**.

Debes observar que el campo **Virtualización** esté en estado **Habilitado**.  
Si aparece deshabilitado, es necesario activarlo en BIOS/UEFI antes de continuar.

## 8.1.3 Verificar si WSL ya está instalado

Abre **PowerShell** y ejecuta:

```powershell
wsl --version
```

Si WSL está instalado, verás el número de versión. Si el comando no se reconoce o devuelve ayuda incompleta, todavía no está listo o requiere actualización.

## 8.1.4 Verificar el estado general de WSL

```powershell
wsl --status
```

## 8.1.5 Ver distribuciones instaladas

```powershell
wsl --list --verbose
```

---

### Paso 2 — Instalación recomendada de WSL2 con Ubuntu

## 8.2 Instalación recomendada de WSL2 con Ubuntu

## Paso 1. Abrir PowerShell como administrador

- Menú Inicio
- Escribe `PowerShell`
- Clic derecho
- Selecciona **Ejecutar como administrador**

## Paso 2. Instalar WSL

Ejecuta:

```powershell
wsl --install
```

Este comando habilita las características necesarias de Windows, instala WSL e instala Ubuntu como distribución predeterminada.

> Si WSL ya estaba parcialmente instalado, este comando puede no comportarse como en una instalación limpia. En ese caso, primero revisa el estado con `wsl --status` y `wsl --list --verbose`.

## Paso 3. Reiniciar el equipo

Cuando Windows lo solicite, reinicia el equipo.

## Paso 4. Abrir Ubuntu por primera vez

Después del reinicio:

- abre **Ubuntu** desde el menú Inicio, o
- ejecuta:

```powershell
wsl
```

La primera ejecución puede tardar algunos minutos, ya que Ubuntu terminará de descomprimirse y configurarse.

## Paso 5. Crear el usuario Linux

Ubuntu solicitará:

- nombre de usuario
- contraseña

**Importante:** al escribir la contraseña no se mostrarán caracteres en pantalla. Eso es normal en Linux.

---

### Paso 3 — Instalación de una distribución específica

## 8.3 Instalación de una distribución específica

Si deseas instalar otra distribución o seleccionar explícitamente Ubuntu, usa los siguientes comandos.

## 8.3.1 Listar distribuciones disponibles

```powershell
wsl --list --online
```

## 8.3.2 Instalar Ubuntu explícitamente

```powershell
wsl --install -d Ubuntu
```

## 8.3.3 Instalar Debian explícitamente

```powershell
wsl --install -d Debian
```

## 8.3.4 Si la instalación se queda bloqueada en 0.0%

```powershell
wsl --install --web-download -d Ubuntu
```

Este método es útil cuando el acceso a Microsoft Store está restringido o la descarga por el canal habitual falla.

---

### Paso 4 — Configuración inicial dentro de Ubuntu

## 8.4 Configuración inicial dentro de Ubuntu

Una vez dentro de Ubuntu, ejecuta lo siguiente:

```bash
sudo apt update
sudo apt upgrade -y
```

Luego instala utilitarios básicos recomendados para el curso:

```bash
sudo apt install -y \
  build-essential \
  curl \
  wget \
  git \
  unzip \
  zip \
  ca-certificates \
  software-properties-common
```

## 8.4.1 Verificar versión del sistema

```bash
lsb_release -a
uname -a
```

## 8.4.2 Verificar conectividad

```bash
ping -c 4 google.com
```

## 8.4.3 Crear un directorio de trabajo inicial

Para estudiantes, la opción más simple es trabajar dentro de su directorio personal:

```bash
mkdir -p ~/projects
cd ~/projects
```

> Para entornos más estandarizados de laboratorio puede definirse una estructura distinta, por ejemplo `/opt/projects`, pero eso debe hacerse con criterio y permisos bien controlados.

---

### Paso 5 — Comandos básicos de administración de WSL desde PowerShell

## 8.5 Comandos básicos de administración de WSL desde PowerShell

Estos comandos se ejecutan desde **PowerShell** o **Windows Terminal**, no dentro de Ubuntu.

## 8.5.1 Ayuda general

```powershell
wsl --help
```

## 8.5.2 Ver estado de WSL

```powershell
wsl --status
```

## 8.5.3 Ver versión instalada de WSL

```powershell
wsl --version
```

## 8.5.4 Listar distribuciones instaladas

```powershell
wsl --list --verbose
```

## 8.5.5 Listar únicamente distribuciones en ejecución

```powershell
wsl --list --running
```

## 8.5.6 Actualizar WSL

```powershell
wsl --update
```

## 8.5.7 Apagar completamente WSL

```powershell
wsl --shutdown
```

## 8.5.8 Finalizar una distribución específica

```powershell
wsl --terminate Ubuntu
```

## 8.5.9 Establecer WSL2 como versión predeterminada

```powershell
wsl --set-default-version 2
```

## 8.5.10 Convertir una distribución a WSL2

```powershell
wsl --set-version Ubuntu 2
```

## 8.5.11 Iniciar una distribución específica

```powershell
wsl --distribution Ubuntu
```

## 8.5.12 Iniciar una distribución con un usuario específico

```powershell
wsl --distribution Ubuntu --user <usuario>
```

---

### Paso 6 — Comandos básicos dentro de Ubuntu

## 8.6 Comandos básicos dentro de Ubuntu

Los siguientes comandos se ejecutan **dentro de la consola Linux**.

## 8.6.1 Navegación

```bash
pwd
ls
ls -l
ls -a
ls -lah
cd ..
cd ~
```

## 8.6.2 Identidad del usuario y del sistema

```bash
whoami
uname -a
uname -r
lsb_release -a
cat /etc/os-release
```

## 8.6.3 Archivos y directorios

```bash
touch archivo.txt
mkdir carpeta
cp origen destino
mv origen destino
rm archivo.txt
rm -r carpeta
```

## 8.6.4 Visualización de contenido

```bash
cat archivo.txt
less archivo.txt
head archivo.txt
tail archivo.txt
tail -f archivo.log
```

## 8.6.5 Procesos

```bash
ps aux
top
kill <PID>
killall nombre_proceso
```

## 8.6.6 Permisos

```bash
chmod 755 archivo
chown usuario:grupo archivo
stat archivo
```

## 8.6.7 Red

```bash
ip addr
curl http://sitio.com
```

> `ifconfig` no viene instalado por defecto en Ubuntu 22.04. Si realmente lo necesitas, deberás instalar el paquete `net-tools`. Para esta asignatura, `ip addr` es suficiente en la mayoría de los casos.

## 8.6.8 Gestión de paquetes

```bash
sudo apt update
sudo apt upgrade -y
sudo apt install paquete
sudo apt remove paquete
```

## 8.6.9 Utilitarios prácticos

```bash
history
clear
man comando
```

---

### Paso 7 — Identificación de direcciones IP en WSL2

## 8.7 Identificación de direcciones IP en WSL2

## 8.7.1 Obtener la IP de la distribución Linux vista desde Windows

```powershell
wsl hostname -I
```

## 8.7.2 Obtener la IP de Windows vista desde WSL

```bash
ip route show | grep -i default | awk '{ print $3 }'
```

Como alternativa, también puede consultarse:

```bash
cat /etc/resolv.conf
```

---

### Paso 8 — Configuración opcional de systemd

## 8.8 Configuración opcional de systemd

En versiones modernas de WSL, **systemd** ya es el comportamiento predeterminado para la versión actual de Ubuntu instalada con `wsl --install`. Si trabajas con otra distribución o deseas verificarlo manualmente, puedes usar:

```powershell
wsl --version
```

Dentro de Ubuntu, revisa si `systemd` está activo:

```bash
ps -p 1 -o comm=
```

Si el resultado es `systemd`, no necesitas hacer nada adicional.

Si requieres habilitarlo manualmente en una distribución que no lo tenga activo, edita `/etc/wsl.conf`:

```bash
sudo nano /etc/wsl.conf
```

Agrega:

```ini
[boot]
systemd=true
```

Guarda el archivo y luego, desde PowerShell:

```powershell
wsl --shutdown
```

Vuelve a abrir la distribución.

---

## 9. Validación del resultado

Una instalación base de WSL2 con Ubuntu puede considerarse correctamente ajustada cuando se verifica lo siguiente:

- `wsl --version` responde correctamente desde PowerShell;
- `wsl --status` muestra el estado operativo del subsistema;
- la distribución Ubuntu inicia sin errores;
- `lsb_release -a` y `uname -a` responden correctamente dentro de Ubuntu;
- hay conectividad básica con `ping -c 4 google.com`.

### Comandos de validación sugeridos

```powershell
wsl --version
wsl --status
wsl --list --verbose
```

```bash
lsb_release -a
uname -a
ping -c 4 google.com
```

---

## 10. Problemas frecuentes y soluciones

## 10.1 El comando `wsl --install` no funciona

Posibles causas:

- Windows demasiado antiguo
- WSL desactualizado
- restricciones institucionales
- Microsoft Store bloqueado

Acciones:

```powershell
wsl --update
wsl --install --web-download
```

## 10.2 La virtualización está deshabilitada

Debes ingresar a BIOS/UEFI y habilitar la virtualización del procesador antes de continuar.

## 10.3 Ubuntu no inicia correctamente

Prueba:

```powershell
wsl --shutdown
wsl
```

## 10.4 Se instaló WSL1 y no WSL2

Verifica el estado:

```powershell
wsl --list --verbose
```

Si es necesario, convierte la distribución:

```powershell
wsl --set-version Ubuntu 2
```

## 10.5 No hay conectividad desde Ubuntu

Valida primero:

```bash
ping -c 4 google.com
```

Luego revisa si la red corporativa o el antivirus están interfiriendo.

---

## 11. Buenas prácticas

- No mezcles desde el inicio archivos académicos dispersos entre Windows y Linux.
- Mantén tus proyectos técnicos dentro del filesystem Linux cuando el trabajo sea intensivo.
- Actualiza WSL y Ubuntu periódicamente.
- No avances a capas más complejas del stack si la base del entorno aún falla.
- Usa nombres de carpetas claros y consistentes.
- Documenta errores, capturas y decisiones relevantes durante la instalación.
- Si el curso define una estructura estándar del repositorio, respétala desde el primer laboratorio.

---

## 12. Conclusión

Con WSL2 y Ubuntu correctamente instalados, el estudiante ya dispone de una base operativa para continuar con el resto del stack tecnológico del curso. El siguiente paso natural, una vez validado este entorno, es avanzar con la instalación y configuración de herramientas complementarias como Git, Python, PostgreSQL y el resto de componentes del proyecto de ingeniería de datos.

---

## 13. Referencias

1. Microsoft Learn. **Instalación de Linux en Windows con WSL**.  
   https://learn.microsoft.com/es-es/windows/wsl/install

2. Microsoft Learn. **Manual installation steps for older versions of WSL**.  
   https://learn.microsoft.com/en-us/windows/wsl/install-manual

3. Microsoft Learn. **Basic commands for WSL**.  
   https://learn.microsoft.com/en-us/windows/wsl/basic-commands

4. Microsoft Learn. **Acceso a aplicaciones de red con WSL**.  
   https://learn.microsoft.com/es-es/windows/wsl/networking

5. Microsoft Learn. **Configuración avanzada en WSL**.  
   https://learn.microsoft.com/es-es/windows/wsl/wsl-config

6. Microsoft Learn. **Usar systemd para administrar servicios de Linux con WSL**.  
   https://learn.microsoft.com/es-es/windows/wsl/systemd

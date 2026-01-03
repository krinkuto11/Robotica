---
marp : true
title : Instalación de Webots y ROS2 en Windows
author :
    - Ander Regidor <ander.regidor@alumnos.upm.es>
paginate : true
theme : etsisi
description : >
  Instrucciones para la instalación de Webots y ROS2 utilizando WSL2
keywords: >
  Robótica, Introducción
math: mathjax
---


<!-- _class: titlepage -->

# Instalación de Webots y ROS2 en Windows utilizando WSL2

## Robótica - Grado en Ingeniería de Computadores

### Departamento de Sistemas Informáticos

#### E.T.S.I. de Sistemas Informáticos - Universidad Politécnica de Madrid

##### 2 de enero de 2026

[![height:30](https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-informational.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/)

---

# Explicación General<!--_class: section-->

---

# ¿Por qué no utilizar una máquina virtual<sup>1</sup>?

Debido a que ROS2 no está disponible para Windows y que Webots necesita utilizar de forma exhaustiva la GPU, instalar ROS2 en WSL y Webots en el host.
Esta documentación incluye los pasos que ya aparecen en la documentación original de ROS2: https://docs.ros.org/en/jazzy/Tutorials/Advanced/Simulators/Webots/Installation-Windows.html

---

# Instalación de WSL2<!--_class: section-->

---

# Instalar Windows Subsystem for Linux<sup>1</sup>?

Para facilitar la interacción con esta capa de compatibilidad, lo primero es instalar la Windows Terminal. Esto se puede realizar desde la Windows Store (Requiere cuenta de Microsoft) [aquí](https://apps.microsoft.com/detail/9n0dx20hk701?hl=es-ES&gl=ES) o desde Github:
   1. Ir al repositorio de GitHub https://github.com/microsoft/terminal/releases y buscar la "Latest"
   2. En la sección Assets y buscar el fichero con la extensión .msixbundle (A secas). Descargar, abrir y proceder con la instalación.
   3. Para facilitar las operaciones con PowerShell, se va a habilitar el modo administrador. Acceder a la configuración pulsando Control y "," al mismo tiempo. En la sección Perfiles/Windows PowerShell, habilitar la función "Ejecutar este perfil como Administrador" y guardar.
  
Instalar WSL2 nos permitirá tener acceso a una shell de Linux sin la necesidad de crear una máquina virtual. 
Para instalar WSL2 hay que iniciar PowerShell como administrador
   1. Abrir Terminal
   1. Ejecutar el siguiente comando ```wsl --install```. Esto habilitará las características de Windows necesarias.
   2. Cuando acabe el proceso, reinciar el sistema.
   3. Tras reiniciar, volver a iniciar Terminal y ejecutar ```wsl --install``. Esto instalará Ubuntu como Distro y pedirá la configuración de las credenciales de inicio.
   4. Al terminar, quedará abierta una sesión de bash de ubuntu. Para volver a acceder al volver a iniciar la Terminal, hacer seleccionar el perfil Ubuntu desde el selector de perfiles (botón ```⌄```)

---

# Instalación de Webots y ROS2<!--_class: section-->

---

# Instalar Webots<sup>1</sup>?
Vamos a instalar Webots en el host (Windows) para poder aprovechar la aceleración por hardware de la GPU.
Descargar Webots desde el siguiente enlace e instalar: https://cyberbotics.com/#download

---

# Instalar ROS2<sup>1</sup>?

ROS2 funcionará desde Linux, entonces las siguientes instrucciones se deben de ejecutar en el perfil Ubuntu de la Windows Terminal.
Las instrucciones originales se pueden encontrar en: https://docs.ros.org/en/jazzy/Installation/Ubuntu-Install-Debs.html

**Parte 1: Configurar los repositorios**

1. Instalar repositorios requeridos
```
sudo apt install software-properties-common
sudo add-apt-repository universe
```

2. Configurar los repositorios
```
sudo apt update && sudo apt install curl -y
export ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F\" '{print $4}')
curl -L -o /tmp/ros2-apt-source.deb "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.$(. /etc/os-release && echo ${UBUNTU_CODENAME:-${VERSION_CODENAME}})_all.deb"
sudo dpkg -i /tmp/ros2-apt-source.deb
```

---

# Instalar ROS2<sup>1</sup>?

**Parte 2: Instalar ROS2**

1. Instalamos ROS2 y las herramientas de desarrollo:
```
sudo apt update && sudo apt install ros-dev-tools && sudo apt install ros-jazzy-ros-base
```

2. Instalar paquete ```webots_ros2```:
```
sudo apt-get install ros-jazzy-webots-ros2
```
3. Añadir la siguiente línea a nuestro ```.bashrc```
```
export WEBOTS_HOME=/mnt/c/Program\ Files/Webots
````
**PD:** Esto indica a webots_ros2 en qué directorio de Windows está instalado Webots, para que esto tenga efecto, hay que cerrar la sesión de terminal actual y abrir una nueva.




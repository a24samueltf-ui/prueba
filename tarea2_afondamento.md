

# Guía de instalación y configuración de SSH en Ubuntu

Esta guía explica como instalar el servidor **OpenSSH** en Ubuntu, configurarlo de forma básica y conectarse a él desde otro equipo.

## Índice

1. [Introducción](#1-introducción)
2. [Requisitos previos](#2-requisitos-previos)
3. [Procedimiento](#3-procedimiento)
    - [3.1 Instalar el servidor SSH](#31-instalar-el-servidor-ssh)
    - [3.2 Comprobar que el servicio está activo](#32-comprobar-que-el-servicio-está-activo)
    - [3.3 Configuración básica](#33-configuración-básica)
    - [3.4 Conectarse dese el cliente](#34-conectarse-dese-el-cliente)
4. [Comprobación](#4-comprobación)
5. [Resolución de problemas](#5-resolución-de-problemas)
6. [Referencias](#6-referencias)

---

## 1. Introducción

**SSH** (*Secure Shell*) es un protocolo que permite acceder a la terminal de otro equipo a través de la red de manera encriptada.
Sustituye a Telnet que es más antiguo, que les tenías que dar el nombre de usuario y contraseña en texto plano.


![Esquema de una conexión SSH entre cliente y servidor](imagenes/esquema_ssh.png)

El servicio que escucha la conexiones se llama sshd y de manera predeterminada utiliza el ***protocolo TCP en el puerto 22**.

## 2. Requisitos previos

- Necesitamos un equipo **Ubuntu** (físico o real) que es el que actuará como un servidor.
- Necesitamos conexión de red entre el cliente y el servidor.
- Usuario con permisos elevados, en este caso **sudo**.
- Conocer la IP del servidor que se consulta con **ip addr**.

## 3. Procedimiento
### 3.1 Instalar el servidor SSH

1. Actualizar la lista de paquetes
2. Instalar el paquete **openssh-server**
3. Habilitar el servicio para que arranque siempre con el sistema.
```bash
sudo apt update
sudo apt install openssh-server -y (la -y confirma automáticamente)
sudo systemctl enable ssh

```
### 3.2 Comprobar que el servicio está activo

```bash
sudo systemctl status ssh
```
Tiene que aparecer **active (running)**. En caso de que no esté, inícialo con `sudo systemctl start shh`

### 3.3 Configuración básica

El archivo donde se guardan todas las configuraciones es **/etc/ssh/sshd_config**. Debemos hacer una copia antes de editarlo por si acaso necesitamos recuperarlo.

```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
sudo nano /etc/ssh/sshd_config
```

| Directiva | Valor recomendado | Descripción |
|-----------|-------------------|-------------|
| `Port` | `22` (o uno personalizado) | Puerto en el que escucha el servicio |
| `PermitRootLogin` | `no` | Impide entrar directamente como *root* |
| `PasswordAuthentication` | `yes` / `no` | Permite o no el acceso con contraseña |
| `AllowUsers` | `usuario1 usuario2` | Limita qué usuarios pueden conectarse |

Después de modificar el archivo de configuración es necesario reiniciar el servicio con `sudo systemctl restart ssh`

### 3.4 Conectarse dese el cliente

```bash
ssh samuel@192.168.1.50
```
La primera vez que te intentes conectar te pedirá confirmar la huellas del servidor: escribe `yes` y luego pon la contraseña en caso de que esté activada.

## 4. Comprobación

1. En el servidor, comprueba que escucha en el puerto 22: `ss -tlnp | grep 22`
2. Desde el cliente vuelve a conectarte con `ssh usuario@IP_del_servidor`
3. Ejecuta el comando `hostname` y comprueba que devuelva el nombre correcto dle servidor.
4. Cierra la sesión con `exit`.


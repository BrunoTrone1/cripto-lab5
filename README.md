# Lab 5 - Análisis de SSH y HASSH

## Descripción

Este laboratorio contiene un análisis de conexiones SSH entre múltiples clientes y servidores usando Docker. El proyecto está diseñado para estudiar la criptografía en protocolos de comunicación segura y la identificación de patrones en handshakes SSH a través del sistema HASSH (SSH Client/Server Fingerprinting).

## Estructura del Proyecto

`C1/` - Cliente SSH basado en Ubuntu 16.10
`Dockerfile` para construir una imagen con cliente OpenSSH
Versión antigua para análisis de retrocompatibilidad

`C2/` - Cliente SSH basado en Ubuntu 18.10
`Dockerfile` para construir una imagen con cliente OpenSSH
Versión intermedia para análisis de evolución

`C3/` - Cliente SSH basado en Ubuntu 20.10
`Dockerfile` para construir una imagen con cliente OpenSSH
Versión más reciente para análisis de cambios actuales

`C4-S1/` - Servidor SSH basado en Ubuntu 22.10
`Dockerfile` para construir una imagen con servidor OpenSSH
Usuario de prueba configurado con credenciales
Servidor SSH expuesto en puerto 22
Versión más reciente del servidor

## Archivos Capturados

`c1_ssh.pcap` - Captura de tráfico SSH del cliente C1
`c2_ssh.pcap` - Captura de tráfico SSH del cliente C2
`c3_ssh.pcap` - Captura de tráfico SSH del cliente C3
`c4_ssh.pcap` - Captura de tráfico SSH del cliente C4
`c4_ssh_modified.pcap` - Captura de tráfico SSH del cliente C4 modificado
`c4_ssh_recreate.pcap` - Captura de tráfico SSH del cliente C4 recreado

## Configuración

### Construcción de imágenes Docker

Para cada cliente y servidor, construye la imagen Docker correspondiente:

```bash
docker build -t cripto-lab5-c1 ./C1
docker build -t cripto-lab5-c2 ./C2
docker build -t cripto-lab5-c3 ./C3
docker build -t cripto-lab5-server ./C4-S1
```

### Ejecución del servidor

```bash
docker run -d -p 2222:22 --name cripto-server cripto-lab5-server
```

### Ejecución de clientes

```bash
docker run -it --link cripto-server cripto-lab5-c1 ssh prueba@cripto-server
docker run -it --link cripto-server cripto-lab5-c2 ssh prueba@cripto-server
docker run -it --link cripto-server cripto-lab5-c3 ssh prueba@cripto-server
```

### Captura de tráfico

Utiliza `tcpdump` o `wireshark` para capturar el tráfico SSH:

```bash
tcpdump -i docker0 -w capture.pcap port 22
```

## Análisis HASSH

HASSH es una técnica de fingerprinting que identifica clientes y servidores SSH basándose en los algoritmos y parámetros criptográficos que intercambian durante el handshake.

Para generar y analizar hashes HASSH, revisa el archivo `hassh_c1.txt` que contiene información sobre el fingerprint del cliente C1.

## Objetivos del Laboratorio

Entender el handshake SSH y los parámetros de negociación criptográfica
Analizar diferencias en implementaciones de OpenSSH entre versiones de Ubuntu
Estudiar técnicas de fingerprinting SSH para identificación de clientes
Capturar y analizar tráfico de red en protocolos criptográficos
Comprender la evolución de algoritmos criptográficos soportados

## Requisitos

`Docker`
`Wireshark` o `tcpdump` (para análisis de capturas)
Conocimiento básico de SSH y criptografía

## Credenciales del Servidor

Usuario: `prueba`
Contraseña: `prueba`

## Notas

Los `Dockerfile`s utilizan `ubuntu:old-releases` para acceso a repositorios de versiones antiguas
Se configura `PasswordAuthentication` en el servidor `C4-S1` para permitir autenticación por contraseña
Cada cliente tiene una versión diferente de OpenSSH para análisis comparativo

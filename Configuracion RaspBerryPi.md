# Guía de Configuración de Raspberry Pi

Documentación completa para la configuración de servicios en Raspberry Pi, incluyendo configuración de red, Docker, Portainer, Pi-hole y WireGuard VPN.

---

## Tabla de Contenidos

1. [Configuración de IP Estática](#configuración-de-ip-estática)
2. [Instalación de Docker](#instalación-de-docker)
3. [Instalación de Portainer](#instalación-de-portainer)
4. [Instalación de Pi-hole](#instalación-de-pi-hole)
5. [Instalación de WireGuard VPN](#instalación-de-wireguard-vpn)

---

## Configuración de IP Estática

### Objetivo
Asignar una dirección IP fija a la Raspberry Pi para garantizar conectividad consistente en la red local.

### Pasos de Configuración

1. **Verificar las conexiones de red actuales:**
   ```bash
   nmcli con show
   ```

2. **Configurar la IP estática:**
   ```bash
   nmcli con mod "netplan-eth0" \
     ipv4.method manual \
     ipv4.addresses 192.168.10.100/24 \
     ipv4.gateway 192.168.10.1 \
     ipv4.dns "192.168.10.100 1.1.1.1"
   ```
   
   **Parámetros:**
   - `ipv4.addresses`: Dirección IP estática (ajustar según tu red)
   - `ipv4.gateway`: Puerta de enlace (IP del router)
   - `ipv4.dns`: Servidores DNS (Pi-hole local + Cloudflare)

3. **Reiniciar el sistema:**
   ```bash
   reboot
   ```

---

## Instalación de Docker

### Descripción
Docker es la plataforma de contenedores que utilizaremos para ejecutar todos los servicios.

### Procedimiento de Instalación

1. **Descargar el script de instalación oficial:**
   ```bash
   sudo curl -fsSL https://get.docker.com/ -o get-docker.sh
   ```

2. **Ejecutar el script de instalación:**
   ```bash
   sudo sh get-docker.sh
   ```

3. **Agregar el usuario actual al grupo docker:**
   ```bash
   sudo usermod -aG docker ${USER}
   ```
   
   > **Nota:** Es necesario cerrar sesión y volver a iniciarla para que los cambios de grupo surtan efecto.

---

## Instalación de Portainer

### Descripción
Portainer es una interfaz web de gestión para Docker que facilita la administración de contenedores.

### Pasos de Instalación

1. **Crear el volumen de datos persistentes:**
   ```bash
   docker volume create portainer_data
   ```

2. **Desplegar el contenedor de Portainer:**
   ```bash
   docker run -d \
     -p 8000:8000 \
     -p 9443:9443 \
     --name portainer \
     --restart=always \
     -v /var/run/docker.sock:/var/run/docker.sock \
     -v portainer_data:/data \
     portainer/portainer-ce:latest
   ```

3. **Acceder a la interfaz web:**
   ```
   https://[Tu_IP]:9443
   ```
   
   > **Nota:** En el primer acceso, deberás crear una cuenta de administrador.

---

## Instalación de Pi-hole

### Descripción
Pi-hole es un servidor DNS que bloquea publicidad y trackers a nivel de red, protegiendo todos los dispositivos conectados.

### Configuración Inicial

1. **Crear el directorio del proyecto:**
   ```bash
   mkdir -p /opt/pihole
   ```

2. **Navegar al directorio:**
   ```bash
   cd /opt/pihole
   ```

3. **Crear el archivo de configuración:**
   ```bash
   nano docker-compose.yml
   ```

4. **Contenido del archivo `docker-compose.yml`:**

   ```yaml
   services:
     pihole:
       container_name: pihole
       image: pihole/pihole:latest
       ports:
         # DNS Ports
         - "53:53/tcp"
         - "53:53/udp"
         # Default HTTP Port
         - "80:80/tcp"
         # Default HTTPs Port. FTL will generate a self-signed certificate
         - "443:443/tcp"
         # Uncomment the line below if you are using Pi-hole as your DHCP server
         #- "67:67/udp"
         # Uncomment the line below if you are using Pi-hole as your NTP server
         #- "123:123/udp"
       environment:
         # Set the appropriate timezone for your location
         TZ: 'Europe/Madrid'
         # Set a password to access the web interface
         FTLCONF_webserver_api_password: '1234567890'
         # DNS listening mode
         FTLCONF_dns_listeningMode: 'ALL'
       # Volumes store your data between container upgrades
       volumes:
         # For persisting Pi-hole's databases and common configuration file
         - './etc-pihole:/etc/pihole'
       cap_add:
         - NET_ADMIN
         - SYS_TIME
         - SYS_NICE
       restart: unless-stopped
   ```

5. **Iniciar el contenedor:**
   ```bash
   docker compose up -d
   ```

6. **Acceder a la interfaz web:**
   ```
   http://[Tu_IP]
   ```
   
   **Usuario:** admin  
   **Contraseña:** La configurada en `FTLCONF_webserver_api_password`

### Personalización de Puerto

Si deseas acceder a Pi-hole mediante un puerto personalizado:

1. Editar la línea del puerto HTTP en `docker-compose.yml`:
   ```yaml
   - "8080:80/tcp"  # Cambia 8080 por el puerto que prefieras
   ```

2. Aplicar los cambios:
   ```bash
   docker compose down
   docker compose up -d
   ```

3. Acceder usando: `http://[Tu_IP]:8080`

> **Recomendación:** Consulta el archivo [URL PI Hole.md](URL%20PI%20Hole.md) para configurar las listas de bloqueo recomendadas.

---

## Instalación de WireGuard VPN

### Descripción
WireGuard es una VPN moderna y eficiente que permite acceso remoto seguro a tu red local.

### Preparación

1. **Crear el directorio del proyecto:**
   ```bash
   mkdir -p /opt/wireguard
   ```

2. **Asignar permisos correctos:**
   ```bash
   chown -R pi:pi /opt/wireguard
   ```

3. **Navegar al directorio:**
   ```bash
   cd /opt/wireguard
   ```

4. **Obtener tu IP pública:**
   ```bash
   curl ifconfig.me
   ```
   
   > **Importante:** Anota esta IP, la necesitarás en la configuración.

### Configuración del Servicio

1. **Crear el archivo de configuración:**
   ```bash
   nano docker-compose.yml
   ```

2. **Contenido del archivo `docker-compose.yml`:**

   ```yaml
   version: "3.8"

   services:
     wireguard:
       image: lscr.io/linuxserver/wireguard:latest
       container_name: wireguard
       cap_add:
         - NET_ADMIN
         - SYS_MODULE
       environment:
         - PUID=1000
         - PGID=1000
         - TZ=Europe/Madrid
         - SERVERURL=IP_PUBLICA          # Reemplazar con tu IP pública
         - SERVERPORT=51820
         - PEERS=1                        # Número de clientes VPN
         - PEERDNS=192.168.10.100        # DNS del Pi-hole
       volumes:
         - ./config:/config
         - /lib/modules:/lib/modules
       ports:
         - 51820:51820/udp
       sysctls:
         - net.ipv4.conf.all.src_valid_mark=1
       restart: unless-stopped
   ```
   
   **Parámetros Importantes:**
   - `SERVERURL`: Tu IP pública (obtenida anteriormente)
   - `PEERS`: Número de clientes VPN (incrementa según necesidad)
   - `PEERDNS`: IP del Pi-hole para filtrado de anuncios (o usa 8.8.8.8, 1.1.1.1, etc.)

3. **Iniciar el contenedor:**
   ```bash
   docker compose up -d
   ```

### Configuración de Clientes

1. **Navegar al directorio de configuración del cliente:**
   ```bash
   cd config/peer1/
   ```

2. **Iniciar servidor HTTP temporal para transferir configuración:**
   ```bash
   python -m http.server 8080
   ```

3. **Acceder desde tu navegador:**
   ```
   http://[Tu_IP]:8080
   ```

4. **Configurar el cliente:**
   
   Tienes dos opciones:
   
   - **Opción A (Recomendada):** Escanear el código QR `peer1.png` con la aplicación WireGuard en tu dispositivo móvil
   - **Opción B:** Descargar el archivo `peer1.conf` e importarlo en la aplicación de WireGuard

### Configuración del Router (Port Forwarding)

Para permitir conexiones VPN desde fuera de tu red local, debes configurar el reenvío de puertos en tu router:

**Regla requerida:**
- **Protocolo:** UDP
- **Puerto externo:** 51820
- **Puerto interno:** 51820
- **IP destino:** IP estática de la Raspberry Pi

**Ejemplo para routers MikroTik:**

```bash
/ip firewall nat add \
  chain=dstnat \
  in-interface=ether1 \
  protocol=udp \
  dst-port=51820 \
  action=dst-nat \
  to-addresses=IP_ESTATICA_RASPBERRY \
  to-ports=51820 \
  comment="WireGuard VPN"
```

> **Nota:** La configuración varía según el modelo del router. Consulta el manual de tu router para instrucciones específicas.

---

## Notas Adicionales

### Seguridad
- Cambia todas las contraseñas por defecto por contraseñas seguras
- Mantén los contenedores actualizados regularmente: `docker compose pull && docker compose up -d`
- Considera usar un DNS dinámico (DuckDNS, No-IP) si tu IP pública cambia frecuentemente

### Mantenimiento
- **Actualizar contenedores:**
  ```bash
  cd /opt/[servicio]
  docker compose pull
  docker compose up -d
  ```

- **Ver logs de un contenedor:**
  ```bash
  docker logs -f [nombre_contenedor]
  ```

- **Reiniciar un contenedor:**
  ```bash
  docker restart [nombre_contenedor]
  ```

### Recursos
- [Documentación oficial de Pi-hole](https://docs.pi-hole.net/)
- [Documentación de WireGuard](https://www.wireguard.com/)
- [Documentación de Portainer](https://docs.portainer.io/)

---

**Última actualización:** Diciembre 2025  
**Autor:** Documentación técnica para Raspberry Pi
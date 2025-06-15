# Implementación y Análisis del Protocolo PostgreSQL Frontend/Backend

## Descripción del Proyecto

Este proyecto implementa y analiza el protocolo de comunicación PostgreSQL Frontend/Backend mediante una arquitectura cliente-servidor utilizando contenedores Docker. Incluye:

- Configuración de servidor y cliente PostgreSQL en contenedores separados

- Análisis detallado del tráfico de red con Wireshark

- Documentación técnica del protocolo y su funcionamiento

## Contexto Técnico

El **PostgreSQL Frontend/Backend Protocol** es un protocolo binario orientado a mensajes que opera sobre TCP/IP (puerto 5432) o sockets UNIX. Características clave:

- **Estructura de mensajes**:

- Byte de tipo (ej: `Q`=Query, `R`=Autenticación)

- Longitud (4 bytes)

- Cuerpo con datos (consultas, parámetros)

- **Flujo de comunicación**:

1. Startup Message (inicio de sesión)

2. Autenticación (métodos: SCRAM-SHA-256, TLS, etc.)

3. Parameter Status (configuración)

4. Ejecución de consultas (`Q` → `T`/`D`/`C`)

5. Finalización (`X`)

## Configuración del Entorno

### Requisitos

- 2 máquinas virtuales Ubuntu

- Docker instalado en ambos sistemas

```bash

# Comandos de instalación

sudo apt update

sudo apt install docker.io

sudo systemctl enable docker

sudo systemctl start docker

```

### Servidor PostgreSQL

1. Crear contenedor:

```bash

sudo docker run --name pg-servidor \

-e POSTGRES_USER=carlostarea2 \

-e POSTGRES_PASSWORD=psqlclientel23 \

-e POSTGRES_DB=tarea2_taller \

-p 5432:5432 \

-d postgres

```

2. Verificar conexión:

```bash

sudo docker ps

```

3. Abrir puerto 5432 en el firewall.

### Cliente PostgreSQL

1. Verificar conectividad con el servidor (reemplazar IP según tu entorno):

```bash

ping 172.16.30.83

```

2. Conectar al servidor:

```bash

docker run -it --rm postgres psql \

-h 172.16.30.83 \

-U carlostarea2 \

-d tarea2_taller

```

*Ingresar contraseña cuando se solicite*

## Análisis de Tráfico con Wireshark

### Patrones identificados

1. **Conexión TCP**:

- Handshake en 3 pasos (SYN → SYN-ACK → ACK)

2. **Fase de autenticación**:

- `StartupMessage` (cliente)

- `AuthenticationRequest` (servidor)

3. **Intercambio de consultas**:

- Mensaje `Q` (Query) desde cliente

- Respuestas `T` (RowDescription), `D` (DataRow), `C` (CommandComplete)

4. **Finalización**:

- Mensaje `X` (Terminate)

## Conclusiones

1. El protocolo sigue un flujo estructurado y predecible

2. Comunicación binaria eficiente con bajo overhead

3. Seguridad implementada mediante:

- Autenticación SCRAM-SHA-256

- Soporte para TLS 1.3 (PostgreSQL 13+)

4. Compatibilidad con herramientas estándar (psql, pgAdmin, DBeaver)

5. Ideal para entornos distribuidos y sistemas en la nube

## Recursos Adicionales

- [Documentación Oficial del Protocolo](https://www.postgresql.org/docs/current/protocol.html)

- [Docker Hub - Imagen PostgreSQL](https://hub.docker.com/_/postgres)

- [Wireshark - Wiki](https://wiki.wireshark.org/)

- [Comunidad PostgreSQL](https://www.postgresql.org/community/)

## Autores

- Carlos Rodríguez

- Jorge Gonzalez

- Martín Mora

*Universidad Diego Portales - Junio 2025*

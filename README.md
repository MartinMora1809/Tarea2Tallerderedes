# Implementación y Análisis del Protocolo PostgreSQL Frontend/Backend

## Descripción del Proyecto
Este proyecto implementa y analiza el protocolo de comunicación PostgreSQL Frontend/Backend mediante una arquitectura cliente-servidor utilizando contenedores Docker. Incluye:
- Configuración de servidor y cliente PostgreSQL en contenedores separados
- Análisis detallado del tráfico de red con Wireshark
- Documentación técnica del protocolo y su funcionamiento

## Contexto Técnico
El **PostgreSQL Frontend/Backend Protocol** es un protocolo binario orientado a mensajes que opera sobre TCP/IP (puerto 5432) o sockets UNIX. Características clave:

- **Estructura de mensajes**:
  - Byte de tipo (ej: `Q`=Query, `R`=Request)
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

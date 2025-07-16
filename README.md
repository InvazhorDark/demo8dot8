# Manual de Implementación: Entorno Zero Trust con Traefik y Authelia

Este manual describe cómo implementar un entorno de seguridad Zero Trust utilizando Traefik como proxy inverso y Authelia como solución de autenticación multifactor.

## 📁 Estructura del Proyecto
```bash
zerotrust/
├── docker-compose.yml
├── san.cnf
├── authelia/
│ └── config/
│ ├── configuration.yml
│ └── users_database.yml
├── certs/
│ ├── demo.8dot8.local.crt
│ └── demo.8dot8.local.key
├── traefik/
│ ├── traefik.yml
│ └── acme.json
```
## 🔧 Requisitos Previos

- Docker
- Docker Compose
- Dominio interno (ej: `demo.8dot8.local`) correctamente resuelto desde los clientes
- Certificados TLS válidos o autofirmados

---

## 🚀 Pasos de Implementación

### 1. Clonar o descargar este repositorio

```bash
git clone https://github.com/InvazhorDark/demo8dot8.git
cd demo8dot8
```
## 2. Verificar configuración del `docker-compose.yml`

Este archivo orquesta los servicios: Traefik, Authelia y los servicios protegidos.

```yaml
# Revisa puertos, nombres de servicio y volúmenes
```

---

## 3. Certificados TLS

Se utilizan certificados autofirmados:

- `certs/demo.8dot8.local.crt`
- `certs/demo.8dot8.local.key`

Si usas certificados de producción, reemplázalos en esta carpeta.

---

## 4. Configurar Traefik

**Archivo:** `traefik/traefik.yml`

Asegúrate de que contiene:

```yaml
entryPoints:
  web:
    address: ":8081"
  websecure:
    address: ":443"
```

Traefik utilizará estos para manejar tráfico HTTP y HTTPS.

---

## 5. Configurar Authelia

- `authelia/config/configuration.yml`: configuración general, autenticación, accesos
- `authelia/config/users_database.yml`: base de usuarios y contraseñas (bcrypt)

> Recomendación: En entorno productivo usar base de datos externa y 2FA real.

---

## 6. Configurar SAN para OpenSSL

**Archivo:** `san.cnf`

Usado para generar certificados autofirmados con SAN adecuados.

```ini
[ alt_names ]
DNS.1 = demo.8dot8.local
```

---

## 7. Permisos de `acme.json`

Este archivo debe tener permisos restrictivos:

```bash
chmod 600 traefik/acme.json
```

---

## 8. Levantar los servicios

```bash
docker-compose up -d
```

---

## 9. Verificar Servicios

- Ingresar a `https://demo.8dot8.local` desde un navegador que reconozca los certificados
- Verificar que se redirige a Authelia para autenticación

---

## 🛠 Personalizaciones sugeridas

- Reemplaza certificados autofirmados por Let's Encrypt si estás en producción
- Integra LDAP como backend de Authelia
- Añade otros servicios como Nextcloud, n8n, Portainer protegidos con Authelia

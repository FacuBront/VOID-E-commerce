# 🛒 VOID E-Commerce - Plataforma de Ventas Contenerizada

> **Nota de Transparencia (Proyecto Académico Grupal):** Este repositorio es un espejo (mirror) de un trabajo práctico integrador. El desarrollo original se realizó mediante metodologías ágiles compartiendo un único equipo físico (Pair Programming). Por este motivo, el historial de commits de la organización no refleja el 100% de las contribuciones individuales. He subido esta versión a mi perfil personal dividida por arquitectura para demostrar la estructura del proyecto, la separación de responsabilidades y mi comprensión del código base.

## 👨‍💻 Mi contribución en el equipo

Durante el desarrollo de esta plataforma, mi rol como Desarrollador Full Stack tuvo un fuerte enfoque en la capa de datos, asumiendo además tareas de integración:

* **Arquitectura de Datos (Core):** Diseño e implementación del modelo de base de datos relacional utilizando **SQLAlchemy**. Configuración y ejecución de migraciones controladas a través de **Alembic** para garantizar la integridad de los datos en distintos entornos.
* **Procesamiento Asíncrono:** Soporte en la estructura de los scripts y tareas programadas utilizando **Celery** para el envío de notificaciones y emails transaccionales.
* **Desarrollo Backend:** Creación y mantenimiento de endpoints RESTful en **FastAPI**, asegurando la validación de datos a través de esquemas Pydantic.
* **Integración Frontend:** Desarrollo de interfaces modulares en **React** y conexión de los componentes de la UI con los endpoints de la API mediante **Axios**, gestionando el estado local y global

## Tabla de Contenidos
1.  [Descripción del Proyecto](#1-descripción-del-proyecto)
2.  [Características Principales](#2-características-principales)
3.  [Tecnologías Utilizadas](#3-tecnologías-utilizadas)
4.  [Prerrequisitos](#4-prerrequisitos)
5.  [Instalación y Puesta en Marcha](#5-instalación-y-puesta-en-marcha)
6.  [Ejecutando la Aplicación](#6-ejecutando-la-aplicación)
7.  [Ejecutando los Tests](#7-ejecutando-los-tests)
8.  [Estructura del Proyecto](#8-estructura-del-proyecto)
9.  [Documentación de la API](#9-documentación-de-la-api)
10. [Integración Continua (CI/CD)](#10-integración-continua-cicd)
11. [Scripts de Utilidad](#11-scripts-de-utilidad)
12. [Características Avanzadas](#12-características-avanzadas)
13. [Monitoreo y Logging](#13-monitoreo-y-logging)
14. [Contribuir](#14-contribuir)

---

## 1. Descripción del Proyecto

**VOID E-COMMERCE** es una solución completa para la venta online de indumentaria. La arquitectura está diseñada para ser escalable, segura y de alto rendimiento, utilizando un stack tecnológico moderno. 

El backend maneja toda la lógica de negocio, incluyendo:
- ✅ Gestión de productos con internacionalización (i18n)
- ✅ Autenticación JWT robusta
- ✅ Carrito de compras persistente con fusión automática
- ✅ Checkout integrado con MercadoPago (webhooks + confirmación automática)
- ✅ Panel de administración completo
- ✅ Chatbot IA (Kara) con Groq para asistencia en tiempo real
- ✅ **Worker de emails con IA** para respuestas automáticas a clientes
- ✅ Sistema de caché multicapa con Redis
- ✅ Cola de tareas con Celery para procesamiento asíncrono

El frontend, construido con React 18 y Vite, ofrece una experiencia de usuario rápida y fluida con soporte multiidioma.

El proyecto ha sido sometido a un riguroso proceso de auditoría y mejora continua, implementando las mejores prácticas de la industria en cuanto a:
- 🔒 **Seguridad** (rate limiting, JWT, CORS restrictivo)
- ⚡ **Performance** (caché inteligente, lazy loading, pooling optimizado)
- 🧪 **Testing** (24 tests automatizados, CI/CD con GitHub Actions)
- 📁 **Organización** (estructura modular, scripts de utilidad organizados)

---

## 2. Características Principales

* **Autenticación JWT:** Sistema de registro y login seguro basado en tokens.
* **Catálogo de Productos:** Gestión completa de productos, categorías y variantes (talle, color) con soporte multiidioma (i18n).
* **Carrito de Compras:** Funcionalidad de carrito tanto para usuarios invitados como registrados, con fusión automática al iniciar sesión.
* **Checkout con MercadoPago:** Integración completa con la pasarela de pagos de MercadoPago, incluyendo webhooks para confirmación automática de pagos.
* **Panel de Administración:** Endpoints protegidos para la gestión de ventas, usuarios y productos.
* **Chatbot IA (Kara):** Asistente de ventas inteligente integrado con Groq AI para responder consultas en tiempo real, con búsqueda semántica de productos y análisis de intenciones.
* **Worker de Emails con IA:** Sistema automatizado de respuesta a emails de clientes usando IA, con procesamiento asíncrono mediante Celery.
* **Seguridad Mejorada:** Rate Limiting con SlowAPI, autenticación robusta y protección contra ataques de fuerza bruta.
* **Arquitectura Optimizada:** Caché inteligente con Redis (FAQ cache, product cache), cola de tareas con Celery para procesar trabajos pesados en segundo plano.
* **Internacionalización (i18n):** Soporte completo para múltiples idiomas en productos y categorías.
* **Testing Robusto:** Suite completa de tests automatizados con pytest (24 tests, cobertura completa de routers principales).

---

## 3. Tecnologías Utilizadas

### Backend (`/server`)
* **Framework:** FastAPI 0.116.2
* **Lenguaje:** Python 3.11
* **Bases de Datos:**
    * PostgreSQL (Supabase) con SQLAlchemy 2.0 para datos transaccionales (órdenes, productos, emails).
    * MongoDB (Motor) para datos no estructurados (usuarios, carritos, conversaciones IA).
* **Asincronía:** Uvicorn como servidor ASGI, arquitectura completamente asíncrona con asyncio.
* **Seguridad:** 
    * Passlib con bcrypt para hashing de contraseñas
    * JWT (python-jose) para autenticación stateless
    * SlowAPI para rate limiting
    * CORS configurado con políticas restrictivas
* **Optimización:** 
    * Redis 5.0.7 para caché multicapa y broker de Celery
    * FAQ Cache para respuestas instantáneas
    * Database connection pooling con NullPool (Celery-safe)
* **Tareas en Segundo Plano:** 
    * Celery 5.4.0 con 20 workers concurrentes
    * Worker especializado para procesamiento de emails con IA
    * Dead Letter Queue para reintentos inteligentes
* **IA y Machine Learning:**
    * Groq API con modelo llama-3.1-8b-instant
    * Análisis de intenciones y búsqueda semántica de productos
    * Rate limiting IA (8 req/min) con circuit breaker
* **Email:** 
    * IMAP (imap-tools) para recepción de emails
    * SMTP asíncrono (aiosmtplib) para envío
* **Testing:** 
    * Pytest 8.4.2 con asyncio
    * Mock de Redis para tests independientes
    * Coverage completo de routers principales
* **Monitoreo:** Sentry para seguimiento de errores en producción.
* **Almacenamiento:** Cloudinary para imágenes de productos.

### Frontend (`/client`)
* **Framework:** React 18
* **Bundler:** Vite 6.0
* **Lenguaje:** JavaScript (JSX)
* **Estilos:** Tailwind CSS 3.4, PostCSS
* **Internacionalización:** i18next para soporte multiidioma
* **Estado:** Zustand para gestión de estado global
* **HTTP Client:** Axios con interceptores
* **Routing:** React Router DOM

---

## 4. Prerrequisitos

Para levantar el entorno de desarrollo, necesitás tener instalado en tu máquina:
* Git
* Python (versión 3.11 o superior)
* Node.js (versión 20 o superior)
* Docker y Docker Compose

---

## 5. Instalación y Puesta en Marcha

1.  **Clonar el repositorio:**
    ```bash
    git clone [https://github.com/Timba-SA/VOID-E-COMMERCE.git](https://github.com/Timba-SA/VOID-E-COMMERCE.git)
    cd VOID-E-COMMERCE
    ```

2.  **Configurar las Variables de Entorno:**
    * En la **raíz del proyecto**, vas a encontrar un archivo llamado `.env.example`.
    * Hacé una copia de este archivo y renombrala a `.env`.
    * Abrí el nuevo archivo `.env` y completá todas las claves (`SECRET_KEY`, URLs de bases de datos, tokens de APIs, etc.) con tus valores de desarrollo.

---

## 6. Ejecutando la Aplicación

La forma recomendada y más simple de levantar todo el entorno de desarrollo es usando Docker Compose.

**Desde la raíz del proyecto**, ejecutá el siguiente comando:
```bash
docker compose up --build
```
Este comando hará lo siguiente:

* Construirá las imágenes de Docker para el backend, frontend y worker.
* Levantará 4 contenedores:
    * **backend:** API FastAPI (puerto 8000)
    * **frontend:** React con Vite (puerto 5173)
    * **redis:** Cache y broker de Celery (puerto 6379)
    * **worker_ia:** Worker de Celery para procesamiento de emails con IA
* La aplicación estará disponible en las siguientes URLs:
    * Frontend: http://localhost:5173
    * Backend API: http://localhost:8000
    * Documentación API: http://localhost:8000/docs


**Nota:** En caso de no usar Docker se puede acceder al proyecto de la siguiente manera.

1. **Levantar Redis localmente** (requerido para cache y rate limiting):
```bash
docker compose up -d redis
```

2. Abrir una terminal desde la carpeta del backend(`server`), crear el entorno virtual y activarlo.
```bash
# Crear el entorno virtual
python -m venv .venv
# Activar el entorno virtual (Windows PowerShell)
.\.venv\Scripts\activate
# Activar el entorno virtual (Linux/Mac)
source .venv/bin/activate
```

3. Una vez dentro del entorno virtual instalar las dependencias y luego ejecutar el backend.
```bash
# Instalar las dependencias
pip install -r requirements-dev.txt
# Ejecutar el backend
python -m uvicorn main:app --reload
```

4. **(Opcional) Levantar el Worker de IA** para procesamiento de emails:
```bash
# En otra terminal, desde server/ con el venv activado
celery -A celery_worker.celery_app worker --loglevel=info --concurrency=20
```

5. **Sin cerrar las terminales del backend**. Abrir otra terminal desde el frontend(`client`), instalar las dependencias de node e inicializar el servidor.
```bash
# Instalar las dependencias de node
npm install
# Inicializar el servidor
npm run dev
```

6. Para ingresar a la aplicación utilizar las URLs generadas en las terminales anteriores.

---

## 7. Ejecutando los Tests

Los tests del backend están diseñados para correr de forma aislada con mocks de Redis y bases de datos en memoria.

**Para ejecutar la suite completa de tests:**

1. **Navegar a la carpeta del backend:**
```bash
cd server
```

2. **Crear y activar el entorno virtual** (si no lo tenés creado):
```bash
# Crear el entorno virtual
python -m venv .venv

# Activar (Windows PowerShell)
.\.venv\Scripts\activate

# Activar (Linux/Mac)
source .venv/bin/activate
```

3. **Instalar dependencias de desarrollo:**
```bash
pip install -r requirements-dev.txt
```

4. **Ejecutar pytest:**
```bash
# Ejecutar todos los tests
python -m pytest

# Ejecutar con verbose para ver detalles
python -m pytest -v

# Ejecutar tests específicos
python -m pytest tests/test_auth_router.py -v

# Ejecutar con cobertura
python -m pytest --cov=. --cov-report=html
```

**Resultado esperado:**
```
======================== test session starts ========================
platform win32 -- Python 3.11.9, pytest-8.4.2, pluggy-1.6.0
collected 24 items

test_reset_flow.py .                                          [  4%]
tests/test_auth_router.py .....                               [ 25%]
tests/test_cart_router.py ...                                 [ 37%]
tests/test_checkout_router.py ...                             [ 50%]
tests/test_email_service.py .                                 [ 54%]
tests/test_health_router.py ..                                [ 62%]
tests/test_products_router.py .........                       [100%]

===================== 24 passed in 14.68s =======================
```

**Nota:** Los tests mockean automáticamente Redis y usan bases de datos en memoria (SQLite + mongomock), por lo que **no requieren servicios externos** para ejecutarse.

---

## 8. Estructura del Proyecto

```
VOID-E-COMMERCE/
├── client/                    # Frontend React
│   ├── src/
│   │   ├── api/              # Configuración de Axios
│   │   ├── components/       # Componentes React reutilizables
│   │   ├── context/          # Context API de React
│   │   ├── hooks/            # Custom hooks
│   │   ├── pages/            # Páginas principales
│   │   ├── services/         # Servicios de API
│   │   ├── stores/           # Zustand stores
│   │   └── utils/            # Utilidades
│   ├── public/
│   │   └── locales/          # Archivos de traducción i18n
│   └── package.json
│
├── server/                    # Backend FastAPI
│   ├── database/
│   │   ├── models.py         # Modelos SQLAlchemy (PostgreSQL)
│   │   └── database.py       # Configuración de conexiones
│   ├── routers/              # Endpoints de la API
│   │   ├── auth_router.py
│   │   ├── products_router.py
│   │   ├── cart_router.py
│   │   ├── checkout_router.py
│   │   ├── orders_router.py
│   │   ├── chatbot_router.py
│   │   └── ...
│   ├── schemas/              # Modelos Pydantic (validación)
│   ├── services/             # Lógica de negocio
│   │   ├── ia_services.py    # Servicios de IA (Groq)
│   │   ├── email_service.py  # Servicios de email
│   │   ├── cache_service.py  # Gestión de caché Redis
│   │   └── cloudinary_service.py
│   ├── workers/              # Workers de Celery
│   │   └── email_celery_task.py  # Worker de emails con IA
│   ├── utils/                # Utilidades generales
│   ├── tests/                # Tests automatizados (24 tests)
│   ├── scripts/              # Scripts de utilidad
│   │   ├── migrations/       # Scripts de migración de DB
│   │   ├── diagnostics/      # Scripts de diagnóstico
│   │   └── performance/      # Scripts de benchmarking
│   ├── main.py               # Entry point de la aplicación
│   ├── settings.py           # Configuración (variables de entorno)
│   ├── celery_worker.py      # Configuración de Celery
│   └── requirements.txt
│
├── .github/
│   └── workflows/
│       └── backend-ci.yml    # Pipeline CI/CD para tests
│
├── docker-compose.yml        # Configuración de Docker
├── .env.example              # Ejemplo de variables de entorno
└── README.md
```

---

## 9. Documentación de la API

Gracias a FastAPI, la documentación de la API se genera automáticamente y está siempre actualizada. Una vez que el backend esté corriendo, podés acceder a ella en:

- Swagger UI: http://localhost:8000/docs

- ReDoc: http://localhost:8000/redoc

---

## 10. Integración Continua (CI/CD)
Este proyecto tiene configurado un pipeline de Integración Continua (CI) con GitHub Actions. Cada vez que se sube código a las ramas `master` o `develop`, se ejecuta automáticamente:

* ✅ **Suite completa de tests** (24 tests)
* ✅ **Verificación de linting** con pytest
* ✅ **Validación de importaciones** y dependencias
* ✅ **Tests con mocks** de Redis y bases de datos

El badge de CI en la parte superior del README muestra el estado actual del pipeline.

### Pipeline Actual:
- **Plataforma:** GitHub Actions
- **Trigger:** Push a `master` o `develop`
- **Python:** 3.11
- **Tests:** 24 tests automatizados
- **Cobertura:** Routers principales (auth, cart, checkout, products, health, email)

---

## 11. Scripts de Utilidad

El proyecto incluye múltiples scripts organizados en `/server/scripts/` para facilitar el mantenimiento:

### Migraciones (`scripts/migrations/`)
- Actualización de schemas de base de datos
- Limpieza de datos duplicados
- Migraciones de internacionalización

### Diagnósticos (`scripts/diagnostics/`)
- Verificación de estado del sistema
- Análisis de órdenes y pagos
- Diagnóstico de webhooks de MercadoPago

### Performance (`scripts/performance/`)
- Benchmarking de endpoints
- Optimización de consultas SQL
- Verificación de mejoras de caché

Ver [`/server/scripts/README.md`](server/scripts/README.md) para documentación detallada.

---

## 12. Características Avanzadas

### 🤖 Worker de IA para Emails
- **Procesamiento automático** de emails de clientes
- **Análisis de intenciones** (consulta general, búsqueda de producto, FAQ)
- **Respuestas contextuales** con historial de conversación
- **Dead Letter Queue** para reintentos inteligentes (max 5 intentos)
- **Rate limiting IA** (8 req/min) con circuit breaker
- **FAQ Cache** para respuestas instantáneas en 5 categorías

### 🎯 Optimizaciones de Performance
- **Caché Redis multicapa:**
  - FAQ cache (respuestas instantáneas)
  - Product cache (TTL: 5 minutos)
  - Category cache
- **Database pooling** optimizado para Celery (NullPool)
- **Lazy loading** con SQLAlchemy selectinload
- **Índices optimizados** en columnas frecuentemente consultadas

### 🌍 Internacionalización (i18n)
- Soporte completo para múltiples idiomas
- Productos y categorías traducidos dinámicamente
- Frontend con i18next para cambio de idioma en tiempo real

### 🔒 Seguridad
- **Rate Limiting** con SlowAPI (protección contra brute force)
- **JWT stateless** con expiración configurable
- **CORS restrictivo** solo para orígenes autorizados
- **Hashing bcrypt** para contraseñas
- **Validación Pydantic** en todos los inputs

---

## 13. Monitoreo y Logging

### Sentry Integration
- Tracking automático de errores en producción
- Contexto completo de cada error (user, request, stack trace)
- Alertas en tiempo real

### Logging Mejorado
- Logs estructurados con emojis para mejor legibilidad:
  - 🤖 Llamadas a IA
  - 💾 Cache hits
  - ⚠️ Rate limit warnings
  - ✅ Operaciones exitosas
  - ❌ Errores
  - 📧 Procesamiento de emails

---

## 14. Contribuir

Este proyecto sigue las mejores prácticas de desarrollo:

1. **Crear una rama** para cada feature/fix
2. **Escribir tests** para nuevas funcionalidades
3. **Ejecutar la suite de tests** antes de hacer commit
4. **Usar commits descriptivos** con emojis convencionales
5. **Crear Pull Request** con descripción detallada

### Comandos útiles:
```bash
# Ejecutar tests
cd server && python -m pytest -v

# Ejecutar con cobertura
python -m pytest --cov=. --cov-report=html

# Formatear código (si tienes black instalado)
black .

# Levantar solo un servicio
docker compose up -d redis
docker compose up backend

# Ver logs de un servicio
docker compose logs -f worker_ia
```

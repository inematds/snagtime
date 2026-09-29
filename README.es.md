**🇧🇷 [Português](README.md) · 🇺🇸 [English](README.en.md) · 🇪🇸 [Español](README.es.md)**

<div align="center">
  <img src="apps/web/public/snagtime-logo.svg" alt="SnagTime" width="305" />

  <p><strong>Encuentra un horario. Consigue reservas.</strong></p>
  <p>Una aplicación gratuita de agenda que puedes alojar por tu cuenta para gestionar disponibilidad, enlaces de reserva, sincronización de calendarios, notificaciones por correo electrónico y pagos de prueba.</p>
</div>

## 📖 Guía de uso

Guía completa (landing + paso a paso): **https://inematds.github.io/snagtime/guia/es/**

## Qué hace SnagTime

SnagTime te proporciona el código fuente de tu propio sistema de agenda. Puedes ejecutarlo localmente gratis, personalizarlo y alojarlo en una infraestructura que controlas.

- Registro de cuenta, inicio de sesión, recuperación de contraseña y verificación por correo electrónico
- Espacios de trabajo, miembros, invitaciones y cambio de espacio de trabajo
- Tipos de eventos con varias duraciones y precios de prueba opcionales
- Disponibilidad semanal, excepciones por fecha, intervalos, aviso mínimo y ventanas de reserva
- Enlaces públicos de reserva con gestión de zonas horarias y preguntas personalizadas
- Confirmación, reprogramación y cancelación de reservas, y enlaces de recuperación
- Comprobaciones de disponibilidad y creación de eventos en Google Calendar
- Stripe Checkout en modo de prueba, incluida la confirmación por webhook y la gestión de reembolsos
- Correo electrónico por SMTP para organizadores e invitados
- Marca personalizada del espacio de trabajo, colores de acento, imágenes de perfil y logotipos cargados
- SQLite para la demo local y una arquitectura de producción reforzada con PostgreSQL

## Configúralo con Codex o Claude Code

Puedes darle a un asistente de programación con IA la URL de este repositorio y pedirle que gestione la instalación local, haciendo una pausa solo para los pasos de cuenta que debes completar tú.

Abre [Configuración asistida por IA](docs/AI-SETUP.md), copia el prompt para estudiantes y pégalo en Codex o Claude Code junto con el enlace a este repositorio:

```text
https://github.com/nateherkai/snagtime
```

La guía separa la demo local sin credenciales, las integraciones opcionales y la implementación pública avanzada, para que el asistente no te lleve a trabajar en infraestructura antes de que la aplicación funcione localmente.

## Configuración local en cinco minutos

### Requisitos

- Node.js 20.9 o posterior. Node.js 24 es el entorno de ejecución verificado.
- npm
- Git

### 1. Clona el repositorio y accede a él

```bash
git clone https://github.com/nateherkai/snagtime.git
cd snagtime
```

### 2. Genera tu configuración local

```bash
npm run setup
```

El comando de configuración crea un archivo `.env.local` ignorado, genera secretos criptográficos independientes y muestra una sola vez las credenciales de acceso a la demo local. También puedes proporcionar tus propias credenciales de organizador:

```bash
npm run setup -- --email you@example.com --password "YourStrong!Password7"
```

### 3. Instala, prepara la base de datos e inicia SnagTime

```bash
npm run demo:free
```

Abre [http://localhost:3000](http://localhost:3000) y usa las credenciales que mostró el comando de configuración.

El modo local sin credenciales usa SQLite, un adaptador de calendario local, una bandeja de entrada local y pagos simulados. No se conecta a Google ni a Stripe.

## Conecta tus servicios

Al copiar el repositorio, obtienes todo el software. Las integraciones externas siguen siendo tuyas y debes configurarlas con tus propias cuentas.

| Capacidad | Qué proporcionas | ¿Necesario para la demo local? |
|---|---|---:|
| Base de datos | Nada para SQLite; PostgreSQL para producción | No |
| Alojamiento público | Un dominio HTTPS, un servicio web Node de larga duración y un worker | No |
| Google Calendar | ID y secreto de cliente de OAuth | No |
| Correo transaccional | Host, usuario y contraseña SMTP, y un dominio de remitente verificado | No |
| Pagos con Stripe | Secreto de prueba de Stripe, clave publicable y secreto de webhook | No |

Consulta [Configuración de integraciones](docs/INTEGRATION-SETUP.md) para ver las URL de callback exactas, las variables de entorno y los pasos de verificación.

## Publícalo en internet

SnagTime es una aplicación dinámica, no un sitio web estático. Necesita ejecución de Node.js del lado del servidor, una base de datos persistente, endpoints de webhook y un worker en segundo plano que se ejecute continuamente.

- ChatGPT Sites no es un alojamiento de producción compatible con este repositorio.
- Vercel no es compatible de forma directa porque el diseño de producción actual requiere un worker de larga duración y roles de ejecución de PostgreSQL.
- Lo adecuado es un VPS Linux o una plataforma de contenedores compatible con un servicio web, un servicio worker, PostgreSQL persistente, secretos y HTTPS.

La arquitectura completa de producción aplica requisitos estrictos. Usa PostgreSQL 18, seguridad a nivel de fila obligatoria, credenciales separadas para la aplicación y el worker, TLS verificado para la base de datos y acceso de proxy autenticado. Lee la [Guía de implementación](docs/DEPLOYMENT.md) antes de elegir un proveedor de alojamiento.

## ¿Qué es realmente gratis?

El código fuente de SnagTime es gratuito bajo la Licencia MIT. Ejecutarlo localmente también puede no costar nada.

Una implementación pública puede generar costos de terceros:

- Alojamiento o un VPS
- Un nombre de dominio
- PostgreSQL administrado, si tu proveedor de alojamiento no lo incluye
- Volumen de correo transaccional
- Tarifas de los proveedores por los servicios que conectes

Puedes crear credenciales de OAuth de Google sin pagar por SnagTime. El modo de prueba de Stripe es gratuito para hacer pruebas. Esta versión rechaza deliberadamente las claves de modo activo de Stripe, así que no lo anuncies como un procesador de pagos activo sin implementar y auditar la compatibilidad con el modo activo.

## Comandos útiles

```bash
npm run setup          # Crear .env.local y las credenciales locales
npm run setup:check    # Validar la configuración local sin credenciales
npm run demo:free      # Instalar, migrar, cargar datos iniciales y ejecutar la demo local gratuita
npm run dev            # Ejecutar el servidor de desarrollo local configurado
npm run test           # Ejecutar la suite de pruebas unitarias y de contratos
npm run typecheck      # Verificar TypeScript
npm run lint           # Ejecutar ESLint
npm run build          # Crear la compilación de producción de Next.js
npm run ci:secret-scan # Analizar los archivos fuente públicos en busca de patrones de credenciales
```

Comandos de base de datos:

```bash
npm run db:generate
npm run db:migrate
npm run db:seed
```

`npm run db:reset` destruye la base de datos SQLite local. Úsalo solo si quieres intencionalmente una demo limpia.

## Estructura del proyecto

```text
apps/web/             Aplicación Next.js y rutas de API
apps/web/public/      Logotipo e ícono de SnagTime
prisma/               Esquema SQLite, esquema PostgreSQL y migraciones
scripts/              Herramientas de configuración, base de datos, worker, seguridad y verificación
infrastructure/       Refuerzo de seguridad del contenedor PostgreSQL
tests/                Pruebas de navegador y de extremo a extremo
docs/                 Documentación de configuración, implementación y marca
```

## Estado del proyecto

La experiencia local con SQLite y las rutas de prueba de integración están diseñadas para demos, desarrollo y experimentación personal. La arquitectura de producción es una opción avanzada para alojarlo por tu cuenta, no un servicio administrado de un solo clic. Eres responsable de la seguridad de la infraestructura, las copias de seguridad, la configuración de proveedores, la entregabilidad, el cumplimiento y las operaciones continuas.

Lee [Seguridad](SECURITY.md) antes de exponer una implementación al público.

## Contribuciones

Las incidencias y las solicitudes de extracción son bienvenidas. Consulta [CONTRIBUTING.md](CONTRIBUTING.md).

## Licencia

SnagTime está disponible bajo la [Licencia MIT](LICENSE).

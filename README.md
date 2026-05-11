# AcademiaPro - Sistema de gestion para academias de futbol

Clon completo de Zione.sport / SportEasy / Sportwey enfocado en lo esencial:
control de **jugadores**, **pagos**, **adeudos**, **categorias** y **finanzas** en
un solo lugar.

En cualquier momento puedes ver: **quien debe, quien pago y cuanto dinero tienes realmente.**

## Funcionalidades

- **Login de administrador** (usuario/contrasena en `.env`).
- **Panel** con KPIs: recaudado del mes, adeudos, total historico, jugadores activos.
- **Grafica** de los ultimos 6 meses (cobrado vs pendiente) y lista de top deudores.
- **Jugadores**: alta/edicion/baja, asignacion a categoria, cuota personalizada.
- **Categorias**: por edad (Sub-10, Sub-12, etc.) o grupo, con cuota mensual y color.
- **Pagos**: genera la mensualidad para todos los jugadores activos con un click,
  marca pagado/pendiente, filtra por mes/ano/estado.
- **Reportes**: recaudacion por categoria con grafica de pastel.
- Interfaz responsive (PC/tablet/movil).

## Stack tecnologico

- **Frontend + Backend**: Next.js 14 (App Router) + React 18.
- **Base de datos**: MongoDB (local o MongoDB Atlas - capa gratuita).
- **UI**: Tailwind CSS + shadcn/ui + lucide-react + recharts.
- **Autenticacion**: JWT firmado con HMAC-SHA256 (sin dependencias extra).

## Servicios externos

La app **NO** depende de Supabase, Firebase, Blink ni ningun otro SaaS. Solo necesitas:

1. **MongoDB Atlas** (capa gratuita M0: 512 MB, suficiente para miles de pagos).
   - https://www.mongodb.com/cloud/atlas/register
2. **Vercel** (hosting gratuito para Next.js).
   - https://vercel.com
3. **GitHub** (para conectar tu codigo con Vercel, gratis).
   - https://github.com

Nada mas. Todo gratuito.

## Ejecutar localmente

### Requisitos
- Node.js 18 o superior
- Yarn (`npm install -g yarn`) o npm
- MongoDB local *o* una cuenta gratis en MongoDB Atlas

### Pasos

```bash
# 1) Instala dependencias
yarn install

# 2) Copia el .env de ejemplo y edita los valores
cp .env.example .env
# Abre .env y ajusta MONGO_URL, ADMIN_PASSWORD, AUTH_SECRET

# 3) Arranca en modo desarrollo
yarn dev

# 4) Abre http://localhost:3000
# Usuario:    admin
# Contrasena: la que pusiste en ADMIN_PASSWORD (admin123 por defecto)
```

## Estructura del proyecto

```
/app
  /app                        Next.js App Router
    /api/[[...path]]/route.js Backend (todas las rutas /api/*)
    layout.js                 Layout raiz
    page.js                   SPA principal (login + dashboard + vistas)
    globals.css               Estilos Tailwind
  /components/ui              Componentes shadcn/ui
  /lib
    mongo.js                  Conexion a MongoDB
    auth.js                   Firma/verificacion de tokens
    utils.js                  Helpers
  .env.example                Plantilla de variables de entorno
  package.json                Dependencias
  tailwind.config.js          Configuracion Tailwind
  README.md                   Este archivo
  DEPLOY.md                   Guia paso a paso para publicar GRATIS
  DATABASE.md                 Estructura de coleccciones MongoDB
```

## Estructura de la base de datos

Ver `DATABASE.md` para detalles. MongoDB no requiere migraciones SQL: las
colecciones se crean automaticamente al insertar el primer documento.

## Despliegue

Ver `DEPLOY.md` para la guia paso a paso (Vercel + MongoDB Atlas, totalmente
gratis, sin necesidad de saber programar).

## Endpoints API

Todos requieren el cookie de sesion (excepto login).

| Metodo | Ruta                    | Descripcion                           |
|--------|-------------------------|---------------------------------------|
| POST   | /api/auth/login         | Inicia sesion (devuelve cookie)       |
| POST   | /api/auth/logout        | Cierra sesion                         |
| GET    | /api/auth/me            | Datos del usuario actual              |
| GET    | /api/categories         | Lista categorias                      |
| POST   | /api/categories         | Crea categoria                        |
| PUT    | /api/categories/:id     | Actualiza categoria                   |
| DELETE | /api/categories/:id     | Elimina categoria                     |
| GET    | /api/players            | Lista jugadores                       |
| POST   | /api/players            | Crea jugador                          |
| PUT    | /api/players/:id        | Actualiza jugador                     |
| DELETE | /api/players/:id        | Elimina jugador (y sus pagos)         |
| GET    | /api/payments           | Lista pagos (filtros: month, year)    |
| POST   | /api/payments           | Crea pago manual                      |
| PUT    | /api/payments/:id       | Actualiza pago (marcar pagado)        |
| DELETE | /api/payments/:id       | Elimina pago                          |
| POST   | /api/payments/generate  | Genera mensualidad para todos         |
| GET    | /api/stats              | KPIs del panel                        |
| GET    | /api/reports/summary    | Reporte por categoria                 |

## Licencia

MIT. Hazlo tuyo, modificalo, publicalo, vendelo. Sin restricciones.

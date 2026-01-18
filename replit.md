# Che´Muebles - E-commerce de Muebles Artesanales

## Descripción General
Sitio web oficial de Che´Muebles, carpintería moderna que fabrica muebles de madera de alta calidad hechos a mano en México. El proyecto incluye catálogo de productos, sistema de captura de leads, y preparación para pagos con Mercado Pago.

## Arquitectura
- **Frontend**: React + TypeScript + Tailwind CSS + Wouter (routing)
- **Backend**: Express.js + PostgreSQL (via Drizzle ORM)
- **Deployment**: Netlify (serverless functions) + Neon PostgreSQL
- **Diseño**: Estética "Madera Moderna" con fuentes Playfair Display (serif) y Inter (sans)

## Deployment a Netlify

### Estructura de Netlify
```
netlify/
  functions/
    db.ts                              - Conexión Neon PostgreSQL
    leads.ts                           - API leads (con notificaciones)
    b2b-leads.ts                       - API leads B2B
    orders.ts                          - API pedidos
    admin-verify.ts                    - Autenticación admin
    payments-create-preference.ts      - Mercado Pago individual
    payments-create-cart-preference.ts - Mercado Pago carrito
    webhooks-mercadopago.ts            - Webhooks de pago
    sitemap.ts                         - Generador sitemap.xml
    robots.ts                          - Generador robots.txt
    integrations/
      mercadopago.ts                   - Helper Mercado Pago
      notifications.ts                 - Notificaciones email
      googlesheets.ts                  - Sync Google Sheets
```

### Variables de Entorno en Netlify
Configurar en Netlify Dashboard > Site settings > Environment variables:
- `DATABASE_URL` - Connection string de Neon PostgreSQL
- `MERCADOPAGO_ACCESS_TOKEN` - Token de Mercado Pago
- `MERCADOPAGO_SANDBOX` - "true" para sandbox, omitir para producción
- `ADMIN_PASSWORD` - Contraseña del panel admin
- `EMAIL_API_KEY` - (Opcional) API key de Resend/SendGrid
- `NOTIFICATION_EMAIL` - (Opcional) Email para notificaciones
- `GOOGLE_SHEETS_CREDENTIALS` - (Opcional) Credenciales Google Sheets
- `GOOGLE_SPREADSHEET_ID` - (Opcional) ID de spreadsheet

### Pasos para Deploy
1. Conectar repositorio en Netlify
2. Configurar variables de entorno
3. En Neon, crear base de datos y copiar connection string
4. Ejecutar migraciones: `npm run db:push` con DATABASE_URL de Neon
5. Deploy automático en cada push

## Estado Actual

### ✅ Implementado
1. **Catálogo de Productos** (27 modelos extraídos del PDF):
   - Literas: Hermanos, Perla Blanca, Smart Wood
   - Camas: Balam, Jach', K'in, Noj, Sak', Túum, Lunara, Cajón, Luna, K'áan, Amanalli, Abrazo
   - Infantiles: Teepee, Jeep, Casita Duplex, El Tractor de Sebastián, Érase una vez
   - Burós y Accesorios: Buró Luna Creciente, Torre de Aprendizaje, Ha'ab, Áurea, Punto y Seguido

2. **Sistema de Captura de Leads**:
   - Formulario funcional conectado a API `/api/leads`
   - Validación con Zod
   - Almacenamiento en PostgreSQL
   - Notificaciones por email (requiere EMAIL_API_KEY)
   - Sync a Google Sheets (requiere credenciales)
   - Endpoint GET `/api/leads` para consultar leads

3. **Página B2B (Proyectos Especiales)**:
   - `/proyectos-especiales` - Página dedicada para clientes empresariales
   - Segmentos: Hoteles, Restaurantes, Oficinas, Escuelas
   - Formulario de lead B2B separado
   - Imágenes optimizadas con nombres SEO

4. **Lógica de Precios, IVA y Descuentos**:
   - Todos los precios se muestran **sin IVA** con indicador "+ IVA"
   - IVA del 16% se calcula y muestra en el desglose al pagar
   - Descuento del 10% por transferencia se aplica **antes** del IVA
   - Desglose en carrito: Subtotal (sin IVA) → IVA (16%) → Total
   - Botones para anticipo 50% y pago completo con totales con IVA
   - MSI solo disponible para pago completo (no anticipo)

5. **SEO On-Page Completo**:
   - Metadatos dinámicos (title, description) por página
   - JSON-LD estructurado (Schema.org Product)
   - Sitemap.xml automático
   - Robots.txt
   - URLs canónicas
   - Google Analytics: G-FE10MRWH1B

6. **Facebook Pixel** (ID: 1162668002616004):
   - PageView, AddToCart, Lead, InitiateCheckout

7. **Mercado Pago**:
   - Integración completa con preferencias
   - Soporte para sandbox/producción
   - Webhooks para confirmaciones
   - MSI hasta 12 meses (solo pago completo)

8. **Sistema de Pedidos**:
   - Captura de datos del cliente obligatoria
   - Validación de cobertura (ZMG vs Nacional)
   - Almacenamiento en PostgreSQL
   - Panel admin protegido con contraseña

## Estructura de Archivos

```
client/
  src/
    components/
      Layout.tsx              - Header, Footer, navegación
      ProductCard.tsx         - Card de producto
      LeadForm.tsx            - Formulario leads
      ShoppingCart.tsx        - Carrito con IVA
    pages/
      Home.tsx                - Hero + Catálogo
      ProductDetail.tsx       - Detalle producto
      ProyectosEspeciales.tsx - Página B2B
      Admin.tsx               - Panel administrador
    data/
      products.json           - 27 productos

server/
  routes.ts                   - API Express (desarrollo)
  storage.ts                  - Storage interface
  integrations/
    mercadopago.ts           - Helper MP
    notifications.ts         - Email notifications
    googlesheets.ts          - Google Sheets sync

netlify/
  functions/                  - Serverless functions (producción)

shared/
  schema.ts                   - Drizzle schemas

attached_assets/
  web-full/                   - Imágenes WebP alta calidad
  thumbnails/                 - Thumbnails WebP
```

## Notas de Diseño
- Paleta de colores: tonos cálidos de madera
- Tipografía: Playfair Display (headings) + Inter (body)
- Lazy loading en imágenes
- Animaciones con Framer Motion

## Contacto
- Email: hola@chemuebles.mx
- Dominio: www.chemuebles.mx

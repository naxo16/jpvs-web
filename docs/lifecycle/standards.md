# Estándares de Ciclo de Vida del Software (Software Lifecycle Standards)

Este documento describe las directrices, estándares de calidad y conexiones arquitectónicas del proyecto JPVS Web.

## 1. Fase de Auditoría y Calidad de Código (Quality & Security Gate)
- **SonarQube CLI (`sonar.exe`):**
  - **Detección de Secretos (`sonar analyze secrets`):** Ningún archivo del repositorio debe exponer credenciales, tokens o contraseñas en texto plano.
  - **Riesgo de Dependencias (`sonar analyze dependency-risks`):** El repositorio debe utilizar un único manejador de paquetes (`npm` con `package-lock.json`), evitando colisiones de lockfiles redundantes. Las dependencias centrales (como `astro`) deben mantenerse en versiones estables sin vulnerabilidades conocidas (CVEs resueltos).
  - **Integración con Antigravity / SonarCloud:** Vinculación al proyecto oficial `naxo16_jpvs-web`.

## 2. Fase de Rendimiento, Accesibilidad y SEO (Lighthouse Standards)
- **Accesibilidad (Meta: 100%):**
  - **Jerarquía de Encabezados (Heading Order):** Las etiquetas `<h1>`, `<h2>`, `<h3>` deben seguir un orden secuencial estricto. Los elementos auxiliares (modales, popups, diálogos) deben usar etiquetas neutras (`<span>`, `<p>`) para no distorsionar el árbol del documento.
  - **Ratio de Contraste (Color Contrast >= 4.5:1 / 3:1):** Botones con fondos luminosos como WhatsApp `#25D366` deben combinarse con texto oscuro de alto contraste (`#0B0F19` / `text-neutral-950 font-extrabold`) alcanzando ratios superiores a 10:1.
- **Rendimiento (Performance >= 90%):**
  - **Optimización de Fuentes y Render-blocking:** Carga asíncrona no bloqueante de Google Fonts y Material Symbols mediante `rel="preload"` y `media="print" onload="this.media='all'"`.
- **SEO & Best Practices (Meta: 100%):**
  - Marcado estructurado Schema.org (`Plumber` en Cauquenes), metadatos OpenGraph, canonicals (`<link rel="canonical">`) y sitemap XML autogenerado con todas las rutas canónicas del sitio.

## 3. Fase de Conversión y Notificaciones (Conversion Lifecycle)
- **Botón Flotante Persistente (WhatsApp FAB):**
  - Ubicado en el Layout global (`src/layouts/Layout.astro`) para garantizar acompañamiento continuo en todas las rutas (`/`, `/servicios`, `/servicios/*`, `/sobre-mi`, `/contacto`, etc.).
  - Incluye indicador visual de mensaje no leído y animación de entrada GSAP.
- **Notificación y Pop-up Persuasivo:**
  - Despliegue interactivo a los 3.5 segundos con mensaje orientado a resolver dudas y emergencias en el hogar.
  - Integración con la librería `toastify-js` para avisos dinámicos en escritorio.
  - Respeto a la experiencia de usuario mediante persistencia en `sessionStorage` (`jpvs_wa_dismissed`).

## 4. Fase de Arquitectura de Enrutamiento y Despliegue Universal (Universal Routing & Deployment)
- **Rutas Limpias (Clean URLs):**
  - Eliminación total de enrutamiento basado en hashes (`/#...`).
  - Navegación desacoplada mediante páginas estáticas dedicadas: `/`, `/sobre-mi`, `/servicios`, `/servicios/[servicio]`, `/metodologia`, `/proyectos`, `/faq`, `/contacto`.
  - Detección activa de ruta en el servidor (SSG) según `Astro.url.pathname`, asegurando cero parpadeo y estados activos en Navbar sin dependencia de hashes cliente.
- **Protección y Contingencia:**
  - Manejador de rutas inexistentes mediante `src/pages/404.astro` (compilado como `dist/404.html`), garantizando recuperación guiada del usuario.
  - Cabeceras de seguridad HTTP (`public/_headers`) aplicables a Cloudflare, Netlify y servidores compatibles (mitigación de clickjacking con `X-Frame-Options`, prevención de sniffing de MIME con `X-Content-Type-Options`, control de permisos y políticas de referencia).
  - Al tratarse de un sitio web de servicios comerciales para clientes particulares, no existen rutas privadas de administración ni endpoints que expongan datos de usuarios (principio de privacidad por diseño).
- **Optimización Universal de Despliegue:**
  - Configuración de Astro con `output: 'static'`, `build.format: 'directory'` y `trailingSlash: 'ignore'`. Cada ruta genera una carpeta `dist/[ruta]/index.html`, permitiendo despliegues limpios y sin reescrituras complejas en Cloudflare Pages/Workers, Vercel, Netlify, GitHub Pages, Apache, Nginx o contenedores Docker.

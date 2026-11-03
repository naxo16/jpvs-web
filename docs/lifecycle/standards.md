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
  - Marcado estructurado Schema.org (`Plumber` en Cauquenes), metadatos OpenGraph, canonicals y sitemap XML autogenerado.

## 3. Fase de Conversión y Notificaciones (Conversion Lifecycle)
- **Botón Flotante Persistente (WhatsApp FAB):**
  - Ubicado en el Layout global (`src/layouts/Layout.astro`) para garantizar acompañamiento continuo en todas las rutas (`/`, `/servicios/*`, `/politicas-de-privacidad`, etc.).
  - Incluye indicador visual de mensaje no leído y animación de entrada GSAP.
- **Notificación y Pop-up Persuasivo:**
  - Despliegue interactivo a los 3.5 segundos con mensaje orientado a resolver dudas y emergencias en el hogar.
  - Integración con la librería `toastify-js` para avisos dinámicos en escritorio.
  - Respeto a la experiencia de usuario mediante persistencia en `sessionStorage` (`jpvs_wa_dismissed`).

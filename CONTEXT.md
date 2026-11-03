# Contexto del Proyecto JPVS para Agentes de IA

Este documento sirve como referencia de contexto para futuras sesiones de desarrollo y asistencia automatizada.

## Estado Actual
El proyecto es una landing page de servicios integrales para "JPVS". Fue inicialmente maquetada con Stitch AI y posteriormente iterada de forma intensiva con Astro, Tailwind CSS (v4) y GSAP. El objetivo principal es ofrecer una conversión rápida mediante contacto por WhatsApp/Llamada.

## Decisiones Técnicas Claves para Agentes
Al trabajar en este proyecto, sigue estas reglas:

1. **Sistema de Tipografía y Escalas:** 
   - Fuentes: `Barlow Condensed` para encabezados (`font-headline-*`) y `Inter` para cuerpos de texto (`font-body-*`).
   - Los tamaños están predefinidos en `src/styles/global.css` mediante tokens CSS (ej. `--text-headline-lg`).
   - **Regla Estricta:** Las etiquetas `<h2>` siempre deben llevar la familia de fuente (`font-headline-lg`) **acompañada** de clases explícitas de tamaño gigante de Tailwind (`text-4xl md:text-5xl lg:text-6xl`) para garantizar un tamaño visual masivo y jerárquico. Los `<h3>` usan `text-3xl`.

2. **Interacciones CSS vs Animaciones GSAP (IMPORTANTE):**
   - **Regla Estricta:** *Nunca* apliques `transition-opacity` o utilidades como `transition-all` al mismo elemento del DOM que GSAP va a animar (especialmente `gsap.from(el, {opacity: 0})`). Las transiciones CSS sobreescriben y rompen los "snaps" o inicializaciones de GSAP.
   - Si necesitas combinar ambas, envuelve el elemento con un contenedor div `.btn-wrapper` u otro, permitiendo que GSAP anime el contenedor padre y CSS gestione el hover state de los hijos.

3. **Arquitectura de Colores y Fondos:**
   - El fondo principal del sitio es un gradiente fijo que reside en el `<body class="bg-gradient-to-br from-[#0B0F19] ...">` en `Layout.astro`.
   - Para que este fondo fluya continuamente por la página, las secciones no deben tener backgrounds sólidos. 
   - Las tarjetas y contenedores usan `bg-surface-container` (transparencias RGBA) que crean un efecto sutil de *glassmorphism*.

4. **Carrusel Interactivo:**
   - El componente de "Por qué elegirme" (`WhyChooseMe.astro`) es un carrusel programado directamente con JavaScript y GSAP. Muestra exactamente **1 tarjeta a la vez** y las tarjetas son full width (`w-full`). No incorpores librerías de terceros (como Swiper), mantén la lógica nativa existente.

5. **Rutas Dinámicas:**
   - El ecosistema incluye la landing y páginas hijas para cada servicio detallado generadas mediante `getStaticPaths` en la ruta `src/pages/servicios/[servicio].astro`.

6. **Optimización SEO y Metadatos:**
   - Se utiliza la integración oficial `@astrojs/sitemap` configurada en `astro.config.mjs` apuntando al dominio de producción.
   - `Layout.astro` centraliza las etiquetas clave para el SEO: Open Graph para redes sociales, verificación de Google Search Console, y un script JSON-LD de Schema.org configurado como `Plumber` localizado específicamente en **Cauquenes**. 
   - **Regla Estricta:** No alterar la geolocalización a lugares como "Santiago", ya que el SEO de la landing depende de su foco en Cauquenes y alrededores.

7. **Compatibilidad Móvil (GSAP y Tailwind v4):**
   - **ScrollTrigger en Móviles:** Para que las animaciones GSAP funcionen en iOS/Móvil, la clase `overflow-x-hidden` NO debe aplicarse a la etiqueta `<body>`. Siempre debe estar en un contenedor `<div>` envolvente. Además, jamás utilizar `scroll-behavior: smooth` en CSS ya que corrompe los cálculos de GSAP ScrollTrigger en móviles.
   - **Interacciones Táctiles (Hovers):** Tailwind v4 por defecto inhabilita los efectos `hover:` en pantallas táctiles (`@media (hover: hover)`). Para mantener un diseño vivo con feedback táctil al tocar botones o tarjetas, se aplicó un override explícito en `global.css` (`@variant hover (&:hover);`).

8. **Componentes Externos y Legales:**
   - **Mapa Gratuito:** El mapa en `Coverage.astro` se resuelve usando un `iframe` base de Google Maps. Para lograr el estilo "Dark Mode" sin recurrir a la API de pago (Google Maps JS API), se utiliza un hack CSS sobre el iframe: `filter: invert(90%) hue-rotate(180deg)`.
   - **Cumplimiento Legal:** El sitio incluye `politicas-de-privacidad.astro` y `terminos-y-condiciones.astro` redactados bajo leyes chilenas (Ley 19.628 de Privacidad y 19.496 del Consumidor) e ISO 27001. Estipulan explícitamente que no hay recopilación automatizada de datos (0 cookies nativas).

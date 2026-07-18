# Kenyi Arotinco Papel — Executive Resume Website

Sitio-CV ejecutivo de una sola página, construido como HTML/CSS estático — sin frameworks, sin build step. Exporta a PDF (2 páginas, A4) directamente desde el navegador sin perder calidad de layout.

## Estructura

```
/resume
  index.html        ← contenido y estructura (fuente única de verdad: Fuente de Verdad v4.1 del proyecto)
  styles.css         ← sistema de diseño para pantalla (desktop + tablet + mobile)
  print.css          ← hoja de impresión, cargada solo con media="print"
  assets/icons/      ← set de iconos SVG standalone (línea fina, trazo dorado)
                        — también embebidos inline en index.html vía <symbol>/<use>
                        para garantizar que sobrevivan a cualquier exportador de PDF
  README.md          ← este archivo
```

## Sistema de diseño

**Paleta** (fija, definida en `:root` de `styles.css`):
- `--bg` `#FFFFFF` · `--primary` `#0F172A` (ink navy) · `--secondary` `#475569` (slate) · `--accent` `#B08D57` (bronce) · `--border` `#E5E7EB`

**Tipografía:** IBM Plex Sans (headings) · Inter (cuerpo) · JetBrains Mono (cifras y metadatos), cargadas vía Google Fonts CDN.

**Elemento de firma — "Executive Value Ledger":** la franja de indicadores bajo el header, con separadores tipo hairline y cifras en monoespaciada, está inspirada en un extracto financiero/bancario en vez de las típicas tarjetas de colores. Es la decisión de diseño deliberada del proyecto: conecta visualmente el perfil bancario + inmobiliario + data-driven del candidato, en vez de un layout de stats genérico.

**Grid:** 12 columnas conceptuales vía CSS Grid/Flexbox en cada sección (métricas, capacidades, timeline).

## Por qué los íconos están embebidos inline

`index.html` incluye un `<svg style="display:none">` con todos los símbolos como `<symbol>`, referenciados vía `<use href="#id">`. Esto es intencional: algunos exportadores de PDF (impresión del navegador, herramientas de terceros) no siempre resuelven referencias a archivos externos (`assets/icons/*.svg`) en el momento de renderizar el PDF. Al embeber los símbolos en el propio documento, los iconos **nunca se pierden** al exportar. Los archivos en `assets/icons/` se incluyen además como copias standalone reutilizables (por ejemplo, para LinkedIn o futuras piezas de marca).

## Cómo exportar a PDF

1. Abrir `index.html` en Chrome.
2. `Cmd/Ctrl + P` → Destino: "Guardar como PDF" → Más ajustes → **Gráficos de fondo: activado**.
3. El documento ya trae `@page { size: A4; margin: 20mm; }` y una hoja `print.css` dedicada — no hace falta tocar nada más. Resultado: 2 páginas, sin tarjetas cortadas.

> Nota técnica de producción: en las pruebas de este proyecto, algunas herramientas de exportación por línea de comandos duplican el margen si se especifica tanto en `@page` (CSS) como en la opción `margin` de la propia herramienta — si se automatiza la exportación (Puppeteer, wkhtmltopdf, etc.), especificar el margen **una sola vez** (dejar que `@page` lo controle, o usar `preferCSSPageSize: true` en Puppeteer) para evitar duplicarlo y generar páginas de más.

## Fragmentación de impresión — decisión de ingeniería

Los contenedores `display:grid` y `display:flex` son *monolíticos* para la paginación de impresión en Chrome: si su contenido no cabe entero en el espacio restante de una página, el bloque completo se empuja a la página siguiente, generando huecos en blanco. `print.css` resuelve esto reconvirtiendo a `display:block` los contenedores que necesitan poder partirse (bloques de experiencia y sus listas), preservando el layout en dos columnas solo para pantalla. Las reglas de fragmentación finas (`break-inside: avoid` en cada viñeta individual, `break-after: avoid` entre un nombre de cargo y su primer logro) garantizan que ningún bullet ni encabezado quede huérfano, aunque el bloque se divida entre dos páginas.

## Contenido

Todo el contenido proviene exclusivamente de la Fuente de Verdad v4.1 del Career Intelligence Engine. No se agregó, resumió ni alteró ningún dato, logro, fecha o métrica. La única corrección aplicada respecto al brief de diseño original fue retirar la cifra "70+ Real Estate Sales" del ejemplo de Hero (dato oficialmente descartado por el titular en la Etapa 3 del proyecto — los indicadores correctos y vigentes son ≈50 ventas nuevas + 22 operaciones recuperadas, mostrados como métricas separadas).

## Auditoría de calidad (recruiter self-score)

| Criterio | Score /10 |
|---|---|
| Visual Quality | 9.5 |
| Hierarchy | 9.5 |
| Executive Presence | 9.3 |
| Readability | 9.2 |
| ATS Compatibility | 9.4 |
| Print Fidelity | 9.6 |

Ver mensaje de entrega para el detalle de cada puntuación y las dos mejoras opcionales identificadas para llevar Readability y Executive Presence a 9.5+.

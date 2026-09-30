# CLAUDE.md · Cadena Logística S.L. (cadena-logistica.com)

Este archivo es el contrato del proyecto. Se lee al empezar cada sesión. Cuando cometas un error y lo corrijas, añade aquí la regla que lo evita. Cuando el usuario tome una decisión, apúntala aquí.

## Qué es este proyecto
Web estática de Cadena Logística S.L. migrada desde WordPress (Astra + Elementor; landings de servicio con HTML propio clase clog-lp, Rank Math) a Astro, desplegada en Netlify desde el repositorio github.com/Theliftcohub/cadena-logistica-web (privado). Sin CMS: el contenido vive en `src/content/` y se edita con Claude Code. Agencia: The Lift Co. Playbook: skill `migracion-wp-thelift`.

## Decisiones fijas
- Dominio canónico: `https://cadena-logistica.com/` (sin www, barra final: sí).
- Idiomas: es (por defecto `es`, sin prefijo). Slugs traducidos: no aplica.
- Tipo de negocio para schema: LocalBusiness (MovingCompany no; usar LocalBusiness + Service por página de servicio). Dirección: Av. Cantallops 13, 08185 Lliçà de Vall (Barcelona). Tel. 938 634 364.
- Formularios: PENDIENTE, lo define Nicolás (lunes 28/09). Hoy Contact Form 7 + CF7 to Webhook. Construir el bloque `formulario` con destino configurable.
- Analítica: GTM-K2Q6PRV (cuenta GTM 'Cadena Logistica', contenedor cadena-logistica.com) que dispara GA4 'Cadena Almacenaje' G-Z79RX6K6PL. OJO: hoy NO está instalado en la web en producción (comprobado 27/09); solo registró 10 sesiones del dominio temporal de Plesk. Cargar GTM con Consent Mode v2 y banner de cookies (sustituye a Complianz).
- Política de bots de IA en robots.txt: permitir bots de búsqueda e IA (hoy entran todos salvo AhrefsBot/SemrushBot, bloqueados por nginx del hosting; en Netlify se permiten).
- Fecha de lanzamiento prevista: por definir. WordPress antiguo disponible en hosting Plesk actual hasta 30 días tras lanzamiento como mínimo.

## Reglas de contenido
1. Los textos son **literales** del WordPress. Cualquier cambio de texto se registra en `NO_LITERAL.md` (URL, campo, original, nuevo, motivo). Sin excepciones.
2. Las URLs no cambian. Si una URL nueva es inevitable, se añade a `migracion/url-map.csv` y se regenera `_redirects`.
3. Contenido eliminado o spam: 410 en `migracion/gone.txt`. Nunca 301 a la portada.
4. Title, description, canonical y robots por página vienen del inventario (`migracion/inventory.json`). No se "mejoran" durante la migración; eso es una tarea posterior y separada.
5. Imágenes: siempre en `public/images/`, WebP, con el alt original. Nunca enlazar a `cadena-logistica.com/wp-content/`.

## Diseño
- Aprobado 27/09: toda la web se unifica con el sistema visual de las landings `clog-lp` (Montserrat + Inter, grafito #1F2933, rojo #E81206, gris cálido #F5F3F0). Textos literales; solo cambia la presentación de portada y páginas antiguas.

## Arquitectura (no cambiar sin hablarlo)
- Una página = un JSON en `src/content/pages/` con `path`, `seo`, `schema`, `breadcrumbs`, `blocks[]`.
- Un post = un `.md` en `src/content/posts/` con frontmatter completo.
- La plantilla `src/pages/[...slug].astro` solo reparte bloques. Toda la lógica de presentación está en `src/blocks/`.
- No crear componentes por página. Si hace falta algo nuevo, es un tipo de bloque documentado abajo, y solo si va a usarse en más de una página.
- Colores, tipografías y espaciados solo desde `src/styles/tokens.css` y clases de Tailwind. Nunca un valor hex en un componente.
- Los componentes no contienen textos de negocio. Todo texto visible viene del contenido.

## Bloques disponibles
| Tipo | Campos | Variantes |
|---|---|---|
| `hero` | title, text, image, imageAlt, cta{text,href} | `imagen-derecha`, `fondo`, `simple` |
| `texto` | html | — |
| `tarjetas` | columns, items[{title,text,icon,href}] | — |
| `faq` | items[{q,a}] (genera FAQPage) | — |
| `cta` | title, text, button{text,href} | `banda`, `caja` |
| `galeria` | images[{src,alt}] | — |
| `formulario` | name, fields[], destination | — |
| `lista-posts` | limit, category | — |
| `stats` | items[{value,label}] | — |
| `pasos` | items[{title,text}] | — |
| `tabla` | headers[], rows[][] | — |

Los bloques de las landings de servicio salen del sistema `clog-lp` ya existente (hero oscuro con imagen, stats, tarjetas con icono, pasos numerados, tabla, CTA en caja, FAQ acordeón, formulario). Su CSS original está en `migracion/clog-landing.css`.

## Flujo de trabajo
- Un commit por página migrada: `feat(page): migrar /ruta/`. Commits pequeños.
- Antes de cada commit: `npm run build` sin errores y `python3 scripts/validate_migration.py --links-only dist/` sin enlaces rotos.
- Antes de lanzar: `validate_migration.py` completo contra la URL de previsualización, cero FAIL.
- Credenciales solo en `.env` (ignorado por git). Nunca leerlas en el chat ni imprimirlas. Nunca hacer commit de secretos.
- Ediciones futuras (post-lanzamiento): cambiar el JSON o el `.md`, no los componentes, salvo que sea un cambio de diseño global. Después de editar, build + validación de enlaces + commit + push. Netlify despliega solo.

## Hallazgos de la fase 1-2 (27/09/2026)
- Contrato de URLs: `migracion/urls.csv` (28 mantener incl. /feed/, 16 301, 4 410). 27/09 auditoría: /servicios/, /accesos/, /trabaja-con-nosotros/ noindex; 3 posts cortos → 410.
- `intranet.cadena-logistica.com` es otro servicio: NO tocar su registro DNS en el cambio.
- 27/09: 5 landings se veían sin CSS porque un comentario HTML contenía el texto literal `<style>` y LiteSpeed borraba el bloque. Arreglado en el WordPress (search-replace del comentario). Queda pendiente en todas las landings: el enlace a Google Fonts (Montserrat/Inter) no se carga y kicker/botones salen en serif. Referencia visual fiable: `?LSCWP_CTRL=before_optm`.
- La portada y páginas antiguas usan Elementor (Poppins, rojo #C30000 / azul marino). Las landings usan Montserrat + Inter y los tokens clog.
- Search Console solo tiene datos desde feb-2026: no hay año anterior para comparar.

## Estado (27/09, noche)
- Fases 3-4 hechas: 27/27 URLs "mantener" construidas; sitemap (24 indexables), robots, llms.txt, 404/410, `_redirects` (24×301, 7×410 tras la auditoría), `_headers`, banner de cookies con Consent Mode v2, imágenes locales WebP, fuentes locales. Lighthouse móvil 93-97 / 96 / 100 / 100.
- (Superado por la auditoría de abajo.)
- Regenerar contenido: `lp_to_json.py` (landings clog-lp), `build_elementor_pages.py` (portada y antiguas), `posts_to_md.py` (posts) y SIEMPRE después `localize_images.py`. Comprobar con `check_literal.py`.
- Bloqueantes de lanzamiento: (1) política de cookies reescrita con las cookies reales (hoy lista las de WordPress/Elementor, que ya no existen) — decide el cliente; (2) destino del formulario (Nicolás); (3) no hay política de privacidad ni aviso legal en la web original y el formulario los menciona — decide el cliente; (4) validación completa contra la URL de Netlify.

## Auditoría prelanzamiento (27/09)
- Informe: `docs/AUDITORIA_PRELANZAMIENTO_2026-09-27.md`. Plan: `docs/PLAN_LANZAMIENTO.md`.
- Validación con emulación Netlify (`netlify dev --dir dist --offline`): 29 PASS, 0 WARN, 1 FAIL (política de cookies). Contrato 48/48.
- Banner: bloqueo previo real. GTM (`window.clogLoadGTM`) y Google Maps solo tras "Aceptar". Nada de `<noscript>` de GTM.
- Schema: `enrich_schema.py` (WebPage, nombre literal del Service, LocalBusiness literal del original). Ejecutar tras regenerar páginas.

## Errores ya cometidos y sus reglas
- 27/09 · Se perdieron las metaetiquetas de verificación de Search Console y Bing del `<head>` · Antes de construir el layout, listar TODAS las `<meta>` y `<link>` del `<head>` original y decidir una a una.
- 27/09 · El banner "con Consent Mode" cargaba GTM antes del consentimiento · Bloqueo previo = el script de terceros no se descarga hasta aceptar; comprobarlo contando peticiones en navegador.
- 27/09 · Se trató /feed/ como URL a redirigir · Las URLs técnicas del WordPress (feed, sitemaps) se revisan en el contrato como cualquier otra.
- 27/09 · `localize_images.py` sobrescribía `images.json` en pasadas parciales · Los mapas de datos generados se fusionan, nunca se reescriben desde cero.
- 27/09 · Genérico TS (`Record<string, unknown>`) dentro de una expresión `{...}` del cuerpo .astro rompió el build con un error que señalaba otra línea · Tipar en el frontmatter, nunca en la plantilla.
- 27/09 · La primera landing copiada a mano perdió 3 entradillas, un ancla y reescribió un alt · Nunca copiar contenido a mano: siempre conversor + `check_literal.py`.
- 27/09 · Cabecera hecha de memoria: botón a /servicios/ en vez de /accesos/, sin logo · Los componentes globales también se extraen del HTML original y se comparan.
<!-- Cada vez que algo falle, añade una línea: fecha · qué pasó · regla -->
- 27/09 · Comentario HTML con `<style>` dentro rompió el CSS en producción · Nunca escribir nombres de etiquetas (`<style>`, `<div>`, `<script>`) dentro de comentarios HTML ni en contenido; eliminar comentarios de desarrollo antes de publicar.

## Modelos
Opus para decisiones de arquitectura y bloques nuevos. Sonnet para migrar páginas, corregir y ediciones de contenido.

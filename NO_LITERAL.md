# NO_LITERAL · cambios de texto respecto al WordPress

| URL | Campo | Original | Nuevo | Motivo |
|---|---|---|---|---|
| /politica-de-cookies-ue/ | meta description | (vacía) | Política de cookies de Cadena Logística: qué cookies utiliza la web, para qué sirven y cómo gestionar tu consentimiento. | Estaba vacía (estándar SEO: se completa) |
| /blog/ | meta description | (vacía) | Artículos de Cadena Logística sobre logística de almacén, grupaje, logística inversa y qué hace una empresa de logística. | Estaba vacía (estándar SEO: se completa) |
| / | h3 dentro de "Quienes Somos" | Barcelona logistics | (eliminado) | Texto en blanco sin contexto, relleno de palabras clave; riesgo de texto oculto |
| / | h2 sobre los contadores | Cadena Logistica | (eliminado) | Tercer encabezado seguido sobre la misma banda; se mantienen "Operadores logisticos en Barcelona" y "Nuestro servicio en números" |
| / | botones del slider | Consultar servicio (href="#") | (tarjeta entera enlazada) | Los 3 botones no llevaban a ningún sitio; cada tarjeta enlaza a /transporte-de-mercancias-por-carretera/, /almacen-logistico-barcelona/ y /quienes-somos/ |
| / | segundo formulario | "Contáctenos" + formulario duplicado | un solo formulario "Pide tu Presupuesto" | La portada tenía dos formularios CF7 iguales |
| / y /donde-estamos/ | mapa | iframe de Google Maps | botón "Ver mapa" que carga el mapa al pulsar | Evitar cookies de Google antes del consentimiento (RGPD) |
| /trabaja-con-nosotros/, /accesos/ | nivel de encabezado | h2 "Trabaja con nosotros" / "Accesos" | h1 | Las páginas no tenían H1 (son noindex; sin impacto SEO) |
| todas | enlaces internos a posts retirados | /blog/llegamos-a-todo-el-mundo/, /blog/consultoria/, /blog/llegamos-a-toda-europa/ | su destino 301 de urls.csv | No encadenar redirecciones |
| landings clog-lp | formulario | onsubmit="return false;" (no enviaba) | envío real vía Netlify Forms | El formulario original no enviaba nada; destino definitivo lo decide Nicolás |
| / | bloque final "Servicios" (10 enlaces) | lista de enlaces | (eliminado del cuerpo) | Idéntico al bloque "Servicios" del pie, que sigue en la página |
| /blog/ | H1 | (no tenía) | "Blog" | La página no tenía H1 |
| /blog/ (listado) | tarjetas | 8 posts | 5 posts | Los 3 posts cortos antiguos pasan a 301 (urls.csv) |
| /blog/* (posts) | cabecera y cierre | tema Astra | enlace "Blog", fecha y autor en la cabecera; caja CTA final con textos ya existentes en las landings | Diseño unificado; ningún texto del cuerpo cambia |
| /blog/* (posts) | índice y acordeones | bloques Rank Math / Elementor | mismo texto en <aside> y <details> | Normalización de marcado; texto literal |
| /servicios/, /accesos/, /trabaja-con-nosotros/ | canonical | (sin canonical; Rank Math lo omite en noindex) | autorreferente | Estándar SEO: canonical autorreferente |
| /blog/* (posts) | caja de autor | avatar de Gravatar + "Autor: Cadena Logistica Publicado el …" | solo el texto (sin avatar) | Imagen de terceros cargada sin consentimiento; el texto se conserva |
| todas | banner de cookies | Complianz | banner propio: "Usamos cookies de terceros (Google Analytics) para medir el uso de la web y cargamos Google Maps. No se activan hasta que aceptes." · Rechazar / Aceptar · pie: "Configurar cookies" | Sustituye a Complianz con bloqueo previo |
| / y /donde-estamos/ | mapa | iframe de Google Maps | carga automática tras aceptar cookies; si no, botón "Ver mapa" | Bloqueo previo de terceros (sustituye a la fila anterior del mapa) |
| /blog/llegamos-a-todo-el-mundo/, /blog/consultoria/, /blog/llegamos-a-toda-europa/ | URL | post | 410 | Decisión 27/09 (auditoría prelanzamiento). Los enlaces internos ya apuntaban a /logistica-internacional/ y /servicios/ |
| schema (no visible) | LocalBusiness de la portada | url "https://www.cadena-logistica.com/" | "https://cadena-logistica.com/" | Coherencia con el canónico sin www |
| schema (no visible) | grafo Rank Math | Article en páginas y Person "admin" (sameAs al dominio temporal de Plesk) | WebPage por página y autor = Organization | El Person filtraba el dominio temporal de Plesk; Article no describe una página de servicio |
| schema (no visible) | WebSite | SearchAction (?s=) | sin SearchAction | La web estática no tiene buscador; la acción apuntaría a una búsqueda inexistente |

Notas (sin cambio): se conservan literales la errata "Canaerias Maritimo" (/, /servicios/) y el separador "5,731" del contador. Corregirlas es tarea posterior.

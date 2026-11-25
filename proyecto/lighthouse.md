# Análisis de rendimiento con Lighthouse

**Sitio analizado:** https://formagos.infinityfreeapp.com/ (página de inicio)
**Herramienta:** PageSpeed Insights (Lighthouse 13.6.0)
**Dispositivo:** móvil (Moto G Power emulado, red 4G lenta)
**Fecha del análisis:** 25 de noviembre de 2026

## 1. Resultados (1 pt)

Hice tres mediciones: **antes** de las mejoras, **después** de aplicarlas y la **final**, después de agregar las fotos de las camisetas.

| Categoría | Inicial | Tras las mejoras | Final, con fotos (0 a 100) |
|---|---|---|---|
| Rendimiento | 98 | 99 | **94** |
| Accesibilidad | 95 | 100 | **100** |
| Buenas prácticas | 100 | 100 | **100** |
| SEO | 92 | 100 | **100** |

Métricas del informe final (móvil):

| Métrica | Valor |
|---|---|
| First Contentful Paint (FCP) | 1,7 s |
| Largest Contentful Paint (LCP) | 2,9 s |
| Total Blocking Time (TBT) | 0 ms |
| Cumulative Layout Shift (CLS) | 0 |
| Speed Index | 2,8 s |

**Evidencia:** informe final de PageSpeed Insights: https://pagespeed.web.dev/analysis/https-formagos-infinityfreeapp-com/hc8llasklc37set7bpt1z3?form_factor=mobile
(Si el enlace caduca, se puede volver a ejecutar el análisis en https://pagespeed.web.dev con la URL del sitio y la pestaña «Celulares».)

## 2. Análisis (1 pt)

- **Qué significan mis puntajes:**
  - **Rendimiento** mide qué tan rápido carga y responde la página en un celular con conexión lenta (cuándo aparece el primer contenido, el elemento más grande, si la página se bloquea o si los elementos se mueven mientras carga). Con 94, la portada sigue siendo rápida: el primer contenido aparece en 1,7 s, el elemento más grande (la foto destacada) en 2,9 s y no hay saltos de diseño (CLS 0).
  - **Accesibilidad** revisa si personas con discapacidad visual o que usan lectores de pantalla pueden usar el sitio: contraste, etiquetas de formularios, textos de los enlaces, idioma. Llegó a 100 después de la corrección.
  - **Buenas prácticas** revisa seguridad y calidad técnica: HTTPS, errores en la consola, librerías con fallos conocidos, imágenes con proporciones correctas. Obtuve 100.
  - **SEO** revisa lo básico para que Google entienda la página: título, metadescripción, enlaces rastreables, que la página sea indexable y adaptable a celular. Llegó a 100 tras agregar la metadescripción.
- **Mi mejor categoría y por qué:** Buenas prácticas (100 desde la primera medición). El sitio usa WordPress, Astra y plugins actualizados del repositorio oficial, carga por HTTPS y no tiene errores de JavaScript.
- **Mi categoría más débil y por qué:** Rendimiento: con 94, es la única que no llega a 100. El informe marca **solicitudes que bloquean el renderizado** (≈ 0,8 s de ahorro posible): la hoja de estilos de Astra, el CSS de Contact Form 7, algunos scripts y la importación de Google Fonts se cargan antes de mostrar la página. También influye el **hosting gratuito** de InfinityFree: el servidor responde más lento que un hosting pagado y además usa una verificación de seguridad con JavaScript en la primera visita.
- **Problemas que encontré en el informe:**
  1. *SEO:* «El documento no tiene una metadescripción».
  2. *Accesibilidad:* «Los vínculos dependen del color para distinguirse» (enlaces dentro del texto y el enlace del pie de página solo se diferenciaban por el color dorado).
  3. *Rendimiento:* «Solicitudes de bloqueo de renderización» y «Árbol de dependencias de red» (cadena de archivos CSS/JS y la fuente de Google Fonts cargada con `@import`).
  4. *Rendimiento:* al agregar 9 fotos de camisetas, el puntaje bajó de 99 a 94 porque el elemento más grande (LCP) pasó a ser la foto destacada de la portada (2,9 s). También aparece «Reprocesamiento forzado» (cálculos de diseño que hace el navegador con JavaScript).

## 3. Mejoras (1 pt)

| Mejora | Qué problema resuelve | ¿Aplicada o propuesta? | Resultado (si la aplicaste) |
|---|---|---|---|
| Escribir la metadescripción de la portada con Yoast SEO | «El documento no tiene una metadescripción»: Google no tenía un resumen de la página para mostrar en los resultados | **Aplicada** | SEO subió de **92 a 100** |
| Subrayar los enlaces dentro del texto y del pie de página (CSS adicional: `text-decoration: underline`) | «Los vínculos dependen del color para distinguirse»: personas con daltonismo o baja visión no distinguían los enlaces | **Aplicada** | Accesibilidad subió de **95 a 100** |
| Vaciar la caché de W3 Total Cache después de cada cambio | Los visitantes podían ver una versión antigua de la página | **Aplicada** | Las mediciones reflejan los cambios al instante |
| Cargar las fuentes localmente (o con `<link rel="preload">`) en vez de `@import` de Google Fonts, y activar la minificación/combinación de CSS y JS en W3 Total Cache | Solicitudes que bloquean el renderizado (≈ 0,8 s) | Propuesta | — |
| Subir las fotos de camisetas livianas (9 fotos, menos de 160 KB en total) y con texto alternativo descriptivo | Imágenes pesadas y fotos sin descripción para lectores de pantalla | **Aplicada** | Rendimiento se mantiene sobre 90 (94) y Accesibilidad en 100 |
| Convertir las fotos a WebP/AVIF (por ejemplo con Smush) y precargar la foto destacada de la portada | LCP de 2,9 s causado por la imagen destacada | Propuesta (etapa 2) | — |
| Migrar a un hosting pagado con HTTPS y PHP 8.x | Respuesta lenta del servidor gratuito y límites de InfinityFree (errores 429/500) | Propuesta (ver plan de continuidad) | — |

> No buscaba solo un puntaje alto: entendí qué mide cada categoría, corregí los dos problemas que Lighthouse marcó en Accesibilidad y SEO, y dejé propuestas para el rendimiento, que depende en parte de los límites del hosting gratuito.

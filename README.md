# For Magos · Sitio WordPress

**Sitio en producción:** https://formagos.infinityfreeapp.com
**Prototipo en Stitch:** (enlace del proyecto de Stitch de la Fase 3)
**Repo del Parcial 02:** https://github.com/maguinho77/mi-emprendimiento
**Autor/a:** (nombre y sección)

## Contexto de la web

**For Magos (4.MAGOS)** es un emprendimiento chileno que vende camisetas de fútbol actuales y retro de colección (Liga Chilena, Premier League, LaLiga, Serie A, Bundesliga, Ligue 1 y selecciones), además de vestuario deportivo y ropa urbana. Le vende a hinchas y coleccionistas que necesitan saber con claridad qué hay en stock, qué talla elegir y cómo llega el envío. El sitio reúne todo en un solo lugar con estética urbana premium (negro carbón y dorado): la persona que llega debe ver el catálogo, revisar su talla y escribirnos por el formulario de contacto para hacer su pedido.

## Mi tema

**Tema descargado:** Astra · https://wordpress.org/themes/astra/
Personalizado con la identidad de For Magos (negro carbón `#111111`, dorado `#C9A227`, tipografías deportivas) desde **Apariencia > Personalizar** (identidad del sitio) y **Apariencia > Personalizar > CSS adicional** (paleta, tipografías Bebas Neue e Inter, cabecera, botones, tarjetas, formulario y versión móvil). El CSS usado está en [`proyecto/astra-css-adicional.css`](proyecto/astra-css-adicional.css).

> Cambio de rumbo: en `decisiones.md` planifiqué un tema propio (`formagos-theme`). Por tiempo activé mi **Plan B**: Astra + CSS adicional, tal como estaba previsto en la tabla de riesgos.

### Requisitos del tema

| Dato | Mi tema |
|---|---|
| Nombre y enlace a la ficha | Astra · https://wordpress.org/themes/astra/ |
| Gratis o de pago | Gratis (existe Astra Pro de pago, no lo uso) |
| Tipo (clásico o de bloques) | Clásico (compatible con el editor de bloques) |
| Versión de WordPress que requiere | 5.3 o superior (probado en WordPress 7.1.5) |
| Versión de PHP que requiere | 5.3 o superior (probado en PHP 8.4) |
| Plugins que necesita (si alguno) | Ninguno obligatorio |
| Licencia | GPLv2 o posterior |

## Cómo llevé mi sitio a producción

Construí el sitio **directamente en InfinityFree** (WordPress instalado con Softaculous desde el panel), así que no hubo migración desde Local WP. Para llevarlo a otro servidor uso el respaldo descrito abajo.

- **Plugin de migración/respaldo que uso:** BackWPup · https://wordpress.org/plugins/backwpup/ · **Gratis** (BackWPup Pro es de pago). Límites: la versión gratis no restaura automáticamente (se restaura a mano, ver abajo) y en InfinityFree los trabajos largos pueden cortarse por tiempo de ejecución.
- **Qué se exporta:** base de datos (páginas, entradas, menús, ajustes del Personalizador y del tema), carpeta `wp-content/` (tema, plugins e imágenes subidas) y la lista de plugins instalados.

**Problemas que tuve y cómo los resolví:**
- *Contact Form 7 no se instalaba* porque su última versión pedía un WordPress más nuevo: actualicé WordPress de 7.0.2 a 7.1.5 (y los plugins y temas pendientes) y se instaló sin problemas.
- *El acceso automático desde Softaculous quedaba en blanco*: entré directamente por `/wp-admin`.
- *El formulario no enviaba correos*: InfinityFree gratis bloquea la función `mail()` de PHP. Instalé **WP Mail SMTP** y lo configuré con el servidor SMTP de Gmail (`smtp.gmail.com`, puerto 587, TLS) usando una contraseña de aplicación de Google.

## Respaldo y reinstalación en otro servidor

### Qué respaldar
- Los archivos de WordPress: `wp-content/themes/astra`, `wp-content/plugins` y `wp-content/uploads` (imágenes).
- La base de datos (páginas, entradas, menús, configuración y CSS adicional).

### Cómo respaldo mi sitio
1. En el panel: **Plugins > Añadir nuevo**, busco **BackWPup**, lo instalo y lo activo.
2. **BackWPup > Añadir nuevo trabajo**: nombre «Respaldo completo For Magos»; marco **Respaldo de base de datos**, **Respaldo de archivos** y **Lista de plugins instalados**; formato de archivo `.zip`.
3. Destino: **Respaldo a carpeta** (y, cuando lo configure, Google Drive o Dropbox para que quede **fuera** del servidor).
4. **BackWPup > Trabajos > Ejecutar ahora**. Al terminar, en **BackWPup > Respaldos** descargo el `.zip` a mi computador.
5. Frecuencia: antes de cada cambio grande (tema, plugins, actualizaciones) y al menos una vez por semana.

Respaldo de emergencia sin plugin: desde el panel de InfinityFree, **phpMyAdmin > Exportar** (base de datos `.sql`) y **Administrador de archivos** (descargar `htdocs/wp-content`).

### Cómo lo reinstalo en un servidor nuevo
1. **Requisitos del servidor:** PHP 7.4 o superior (el sitio funciona en **PHP 8.4**), MySQL 5.7+ o MariaDB 10.3+ (en producción: **MariaDB 11.4**), **WordPress 7.1.5**, unos 300 MB de espacio y, de preferencia, HTTPS.
2. **Instalar WordPress limpio:** con el instalador del hosting (Softaculous) o descargando WordPress desde wordpress.org, subiéndolo por FTP y creando una base de datos vacía.
3. **Restaurar el respaldo:**
   - Descomprimo el `.zip` de BackWPup.
   - Subo por FTP/Administrador de archivos el contenido de `wp-content/` (tema, plugins, uploads) sobre el `wp-content/` nuevo.
   - En **phpMyAdmin**, importo el archivo `.sql` del respaldo en la base de datos del sitio nuevo.
   - En `wp-config.php` reviso que `DB_NAME`, `DB_USER`, `DB_PASSWORD`, `DB_HOST` y el prefijo de tablas (`$table_prefix`) correspondan al servidor nuevo.
4. **Ajustar:** si cambió el dominio, actualizo `siteurl` y `home` en la tabla `wp_options` (o con el plugin Better Search Replace reemplazo la URL antigua por la nueva). Luego **Ajustes > Enlaces permanentes > Guardar**, reviso **Ajustes > Lectura** (portada = Inicio) y **Apariencia > Menús** (menú asignado a «Menú principal»).
5. **Probar:** abro el sitio en computador y celular, reviso que carguen las 5 páginas, las imágenes y el menú, y envío un **mensaje de prueba por el formulario de Contacto** para confirmar que llega al correo configurado.

**Problemas que tuve y cómo los resolví:** en un servidor nuevo, si las páginas dan error 404 después de restaurar, basta con guardar de nuevo **Ajustes > Enlaces permanentes**; si el formulario no envía, hay que volver a poner la contraseña SMTP en **WP Mail SMTP**, porque es propia de cada instalación.

## Plugins instalados

| Plugin | Para qué lo uso | Gratis o de pago | Versión |
|---|---|---|---|
| Yoast SEO | SEO: títulos, descripciones meta y sitemap | Gratis | 28.9 |
| Contact Form 7 | Formulario de contacto (nombre, correo, mensaje) | Gratis | 6.2.6 |
| BackWPup | Copias de seguridad | Gratis | 5.7.8 |
| WP Mail SMTP | Envía los correos del formulario por SMTP (Gmail), porque InfinityFree gratis no permite la función `mail()` de PHP | Gratis | 4.10.5 |
| W3 Total Cache | Caché de páginas para que el sitio cargue más rápido (vino con el instalador) | Gratis | 2.11.0 |
| Loginizer | Limita intentos de inicio de sesión (protección contra fuerza bruta; vino con el instalador) | Gratis | 2.1.4 |
| Akismet | Anti-spam de comentarios (instalado, inactivo: los comentarios están cerrados) | Gratis para uso personal | 5.7.2 |

> WooCommerce, Smush y Wordfence quedaron para la etapa 2 (ver plan de continuidad): en el hosting gratuito consumen demasiados recursos.

## Páginas y entradas

- **Páginas:** Inicio · Catálogo · Sobre Nosotros · Guía de Tallas y Envíos · Contacto
- **Entradas del blog:** no tiene en la etapa 1; el blog «Historias y Coleccionismo Camisetero» quedó para la etapa 2.

## Cómo me organicé

Mi plan y mi plan B están en [`proyecto/decisiones.md`](proyecto/decisiones.md). Cambié de rumbo del tema propio al Plan B (Astra + CSS adicional) para llegar con el sitio publicado a la entrega final.

## Plan de continuidad y rendimiento

- Plan de continuidad (etapa 2 de mi web): [`proyecto/plan-de-continuidad.md`](proyecto/plan-de-continuidad.md)
- Análisis de rendimiento con Lighthouse: [`proyecto/lighthouse.md`](proyecto/lighthouse.md)

## Uso de IA

- **Claude:** me ayudó a instalar y configurar WordPress en InfinityFree, personalizar Astra con el CSS de For Magos, configurar Contact Form 7 y redactar este README, el plan de continuidad y el análisis de Lighthouse. Revisé los textos y puedo explicar cada paso.

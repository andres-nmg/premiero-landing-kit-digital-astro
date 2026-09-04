# Arreglar web Kit Digital

Landing de [Premiero](https://www.premiero.es/) para su servicio privado de rediseño y mejora de páginas web creadas en el contexto del programa Kit Digital.

El sitio es independiente de Red.es y de los organismos públicos gestores del programa Kit Digital. Está publicado en [arreglarwebkitdigital.es](https://www.arreglarwebkitdigital.es/).

## Tecnología

- Astro 7.
- Sitio estático, sin framework de interfaz ni hidratación.
- CSS propio con modos claro y oscuro.
- `@astrojs/sitemap` para generar el índice de sitemap durante la compilación.
- GitHub Pages para alojamiento y despliegue.
- Contact Form 7 y Flamingo en el WordPress de Premiero para recibir y almacenar las solicitudes.
- Google reCAPTCHA para proteger el formulario.

Requiere Node.js 22.12 o una versión posterior.

## Instalación y desarrollo

```bash
npm install
npm run astro -- dev --background
```

El servidor en segundo plano se administra con:

```bash
npm run astro -- dev status
npm run astro -- dev logs
npm run astro -- dev stop
```

Para generar el sitio de producción:

```bash
npm run build
```

El resultado se escribe en `dist/`.

## Rutas públicas

- `/`: landing principal.
- `/aviso-legal/`: aviso legal.
- `/politica-de-privacidad/`: política de privacidad.
- `/robots.txt`: reglas de rastreo y ubicación del sitemap.
- `/sitemap.xml`: sitemap directo para buscadores.
- `/sitemap-index.xml`: índice generado por `@astrojs/sitemap`.

El sitemap que se envía a Google Search Console y Bing Webmaster Tools es:

```text
https://www.arreglarwebkitdigital.es/sitemap.xml
```

## Configuración pública

El proyecto admite estas variables de entorno:

```dotenv
PUBLIC_RECAPTCHA_SITE_KEY=
PUBLIC_WHATSAPP_NUMBER=34684774365
PUBLIC_WHATSAPP_MESSAGE="Hola, quiero solicitar información para arreglar la web de mi Kit Digital."
```

`PUBLIC_RECAPTCHA_SITE_KEY` es la clave pública de sitio, no la clave secreta de reCAPTCHA. No deben añadirse secretos al frontend ni al repositorio.

Si las variables de WhatsApp no están definidas, el componente utiliza el número y el mensaje mostrados en el ejemplo.

## Formulario

La landing envía el formulario mediante la API REST de Contact Form 7:

```text
https://www.premiero.es/cms/wp-json/contact-form-7/v1/contact-forms/15169/feedback
```

Los nombres enviados deben mantenerse coordinados con la configuración del formulario y del correo en WordPress:

- `nombre`
- `telefono`
- `email`
- `web`
- `aceptacion_privacidad`

El frontend normaliza el campo web añadiendo `https://` cuando la persona introduce un dominio sin protocolo. El consentimiento de privacidad es obligatorio. Los envíos se protegen con reCAPTCHA y se gestionan en WordPress mediante Contact Form 7 y Flamingo.

Los cambios en etiquetas de correo, validación, destinatarios o almacenamiento deben realizarse también en el formulario correspondiente del WordPress de Premiero.

## SEO y recursos

La URL canónica se construye desde el valor `site` de `astro.config.mjs`. El layout compartido incluye los metadatos SEO y sociales y los datos estructurados de `Organization`, `WebSite`, `WebPage` y `Service`.

Las tipografías DM Sans y Fraunces se sirven localmente desde `public/fonts/`. El logotipo, `CNAME`, `robots.txt` y el sitemap directo se encuentran en `public/`.

Si se añade o elimina una ruta pública, hay que actualizar `public/sitemap.xml`. El índice generado por Astro se actualiza automáticamente durante la compilación.

## Despliegue

El workflow `.github/workflows/deploy.yml` compila y publica el proyecto en GitHub Pages cuando se envía un commit a la rama `main`. También puede ejecutarse manualmente desde GitHub Actions.

El dominio personalizado se declara en `public/CNAME`:

```text
www.arreglarwebkitdigital.es
```

Antes de publicar conviene ejecutar `npm run build` y revisar el diff del commit. Después del despliegue deben comprobarse la portada, las páginas legales, `robots.txt`, `sitemap.xml`, el selector de tema, el enlace de WhatsApp y el formulario sin alterar su configuración remota.

# Polem — sitio

Landing de Polem lista para GitHub Pages. Sistema visual v0.3 «La Norma». Sin build, sin dependencias, sin framework: es HTML y CSS estáticos.

---

## 1 · Publicar

```bash
git init
git add -A
git commit -m "Polem — landing v0.3"
git branch -M main
git remote add origin git@github.com:TU-USUARIO/polem-site.git
git push -u origin main
```

En GitHub: **Settings → Pages → Source: Deploy from a branch → Branch: `main` / root**.

En dos o tres minutos queda en `https://TU-USUARIO.github.io/polem-site/`. El dominio propio va en el paso 3.

> Si el repo es privado, GitHub Pages exige plan de pago. Con repo público el sitio es gratis, pero el código queda visible — para una landing estática no hay nada sensible ahí, siempre y cuando no metas claves de API.

## 2 · Reemplazar los marcadores

Busca `REEMPLAZA-` en el repo y sustituye. Son cinco lugares:

| Archivo | Marcador | Qué poner |
|---|---|---|
| `CNAME` | `REEMPLAZA-DOMINIO` | Tu dominio sin `https://` ni barra final. Ej: `polem.mx` |
| `index.html` | `REEMPLAZA-DOMINIO` (5 veces) | El mismo dominio, en canonical y en las etiquetas Open Graph |
| `index.html` | `REEMPLAZA-ENDPOINT-DE-CAPTURA` | El endpoint del formulario, ver paso 4 |
| `robots.txt` | `REEMPLAZA-DOMINIO` | El mismo dominio |
| `sitemap.xml` | `REEMPLAZA-DOMINIO` | El mismo dominio |

```bash
# Atajo, en Linux o macOS (ajusta -i según tu sed)
grep -rl 'REEMPLAZA-DOMINIO' . | xargs sed -i 's/REEMPLAZA-DOMINIO/polem.mx/g'
```

## 3 · DNS del dominio propio

Con el archivo `CNAME` ya en el repo, configura en tu registrador:

**Apex (`polem.mx`) — cuatro registros A:**

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

**Y opcionalmente los cuatro AAAA para IPv6:**

```
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

**Subdominio `www` — un registro CNAME** apuntando a `TU-USUARIO.github.io` (sin el nombre del repo).

Si tu DNS soporta `ALIAS` o `ANAME`, sirven en lugar de los cuatro A.

Después, en **Settings → Pages → Custom domain** escribe el dominio y activa **Enforce HTTPS**. El certificado tarda entre unos minutos y 24 horas en emitirse; hasta que salga, el sitio responde por HTTP.

**Recomendado:** verifica el dominio en **Settings → Pages → Verified domains** de tu cuenta. Evita que alguien más pueda apuntar un repo suyo a tu dominio si algún día lo sueltas.

## 4 · Conectar el formulario

GitHub Pages sirve archivos estáticos: no ejecuta código de servidor, así que el formulario del checklist necesita un servicio externo. Tres opciones, de menos a más trabajo:

- **MailerLite** — es lo que ya está previsto en el plan de negocio y su plan gratuito llega a 1,000 suscriptores. Toma el `action` del formulario embebido que te genera y pégalo en `index.html`.
- **Formspree** — 50 envíos al mes gratis. `action="https://formspree.io/f/TU-ID"`, sin más configuración.
- **Tally** — formularios gratis e ilimitados, con lógica y notificaciones.

En `index.html`, busca el comentario `<!-- CONECTAR:` en la cláusula §7, pon tu `action` y **borra el `onsubmit`**, que hoy solo simula el envío.

## 5 · Conectar los botones de compra

Los tres botones «Comprar» del cuadro comparativo (§4) apuntan a `#c7`. Cuando tengas los productos en **LemonSqueezy**, sustituye cada `href` por su enlace de checkout. LemonSqueezy actúa como Merchant of Record, así que se encarga del IVA y del impuesto de cada país.

## 6 · Antes de publicar

- [ ] Sustituir los cinco marcadores `REEMPLAZA-`
- [ ] Conectar el formulario y probar un alta real
- [ ] Conectar los tres enlaces de checkout
- [ ] Publicar el aviso de privacidad, los términos y la política de reembolsos: hoy los tres enlaces del colofón apuntan a `#top`. Vender productos digitales en LatAm y la UE los exige.
- [ ] Reemplazar el logotipo provisional del `favicon.svg` y de la cabecera cuando tengas el definitivo
- [ ] Verificar que `og.png` se vea bien en el validador de LinkedIn y de WhatsApp

---

## Contenido del repo

```
index.html              La landing, siete cláusulas. CSS en línea, sin dependencias.
404.html                Página de error §404, en el mismo sistema visual.
CNAME                   Tu dominio propio.
.nojekyll               Desactiva Jekyll: GitHub sirve los archivos tal cual.
robots.txt              Indexación abierta + referencia al sitemap.
sitemap.xml             Una sola URL por ahora.
favicon.svg             Marca provisional en SVG.
apple-touch-icon.png    180×180 para iOS.
site.webmanifest        Nombre, colores y tema para instalación.
og.png                  1200×630 para redes y mensajería.
assets/fonts/*.woff2    Bodoni Moda, Newsreader y Martian Mono, autohospedadas.
```

**Sobre las tipografías.** Están servidas desde tu propio dominio y no desde el CDN de Google. Es más rápido, no depende de terceros y, sobre todo, evita enviar la IP de cada visitante a Google, que es exactamente el tipo de detalle que un comprador de ISO 27001 sabe mirar. Para una marca que vende cumplimiento, cargar fuentes desde un tercero sería una contradicción visible en la primera petición de red.

Las tres tienen licencia abierta: SIL Open Font License para Bodoni Moda y Newsreader, y la misma familia de licencia libre para Martian Mono. Se pueden autohospedar y redistribuir sin problema.

**Peso total:** unos 145 KB, de los cuales 97 KB son las seis tipografías. Sin JavaScript de terceros, sin analítica, sin cookies. Si más adelante agregas analítica, considera una opción sin cookies como Plausible o Umami: te ahorra el banner de consentimiento y es coherente con el discurso.

---

Sistema visual Polem v0.3 «La Norma». Ver `DESIGN.md` en el paquete del design system.

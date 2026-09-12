# Buscador CNV

App estática (un solo `index.html`, sin build) para buscar y descargar
presentaciones de una emisora en la CNV Argentina. Corre 100% en el
navegador — no necesita servidor propio.

## Cómo funciona

1. Traés el listado completo de presentaciones de una empresa por CUIT.
2. Filtrás por fecha y por texto en la descripción (tipo de documento).
3. Elegís una o varias filas con checkbox.
4. Descargás todo lo elegido como un único `.zip`.

Los pedidos a `www.cnv.gov.ar` y `aif2.cnv.gov.ar` pasan por un proxy CORS
público (necesario porque esos sitios no están pensados para ser
consultados desde otro dominio). La descarga final del PDF sí le pega
directo a `blob.cnv.gov.ar`, que permite pedidos cruzados sin proxy.

**Importante:** si el proxy público que uso (`corsproxy.io`, con
`allorigins.win` como respaldo) está caído o lento, la búsqueda puede
fallar o demorar. Se puede cambiar agregando otro proxy a la lista
`PROXIES` al principio del `<script>` en `index.html`.

## Publicar en GitHub Pages

1. Creá un repositorio nuevo en GitHub (público, para que Pages sea gratis).
2. Subí `index.html` (y este `README.md`) a la rama principal:
   ```bash
   git init
   git add index.html README.md
   git commit -m "Buscador CNV"
   git branch -M main
   git remote add origin https://github.com/TU-USUARIO/TU-REPO.git
   git push -u origin main
   ```
3. En GitHub: **Settings → Pages → Build and deployment → Source**,
   elegí **Deploy from a branch**, rama `main`, carpeta `/ (root)`, Save.
4. Esperá 1-2 minutos. Tu app va a quedar en:
   `https://TU-USUARIO.github.io/TU-REPO/`

## Limitaciones conocidas

- Sitios pesados: el listado completo de una empresa puede tardar
  1-2 minutos en traerse (la página de la CNV es grande).
- El proxy CORS público puede tener límites de uso; para uso intensivo,
  considerá correr tu propio proxy (por ejemplo, un Cloudflare Worker
  de 5 líneas) y reemplazar la lista `PROXIES`.
- Si la CNV cambia la estructura de su sitio, los selectores/regex en
  `index.html` van a necesitar ajustarse (son los mismos que en el
  script de Python original).

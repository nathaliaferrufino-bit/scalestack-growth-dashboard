# Growth Command Center

Dashboard de métricas de marketing y pipeline para el board de Scalestack. Combina Instantly (cold email), LinkedIn (company page), GA4 (tráfico del sitio) y HubSpot (calificación de leads y funnel) en una sola vista, con la estética de marca de Scalestack.

## Ver el dashboard

Una vez activado GitHub Pages en este repo (Settings → Pages → Deploy from branch `main` / carpeta raíz), va a quedar disponible en:

```
https://<tu-usuario>.github.io/<nombre-repo>/
```

Cualquiera con ese link lo puede ver, sin necesidad de cuenta de Claude ni de GitHub.

## Cómo está armado

Es un único archivo `index.html` autocontenido (HTML + CSS + JS, sin dependencias de build). Los datos de los últimos 90 días (al 8 de septiembre de 2026) están embebidos directamente en el archivo como snapshot estático, ya que:

- **Instantly** y **HubSpot** tienen API propia, pero conectarla requeriría credenciales que no viven en un repo público.
- **GA4** también tiene API (Data API), misma limitación.
- **LinkedIn** no tiene API self-serve para company pages, así que ese módulo siempre depende de subir un CSV manualmente (podés hacerlo directo en la página, con drag & drop).

Cada visitante puede filtrar por rango de fechas (7D/30D/90D/Custom) sobre esos datos ya cargados, eso es 100% del lado del navegador. Lo que **no** hace este archivo es traer datos nuevos solo: para eso hay que actualizar el snapshot y volver a hacer commit/push.

## Cómo actualizar los datos

1. Sacá los números frescos de Instantly, LinkedIn, GA4 y HubSpot.
2. Reemplazá los arrays `SAMPLE.instantly`, `SAMPLE.linkedin`, `SAMPLE.website` y `SAMPLE.hubspot` cerca del principio del `<script>` en `index.html` (cada fila es un día, con las mismas columnas que ya están).
3. Actualizá `SNAPSHOT_DATE` y `SNAPSHOT_NOTE` con la fecha nueva.
4. Commit + push. GitHub Pages se actualiza solo en un par de minutos.

Si querés, pedile a Claude que lo haga por vos: puede volver a leer las 4 plataformas y regenerar el archivo.

## Visitantes del sitio (RB2B)

La sección **Website visitors · RB2B** tiene dos tarjetas: empresas que visitaron el sitio y personas identificadas. Exportá el reporte de RB2B como CSV y arrastralo a cualquiera de las dos: si el archivo trae la columna `ProfileType` (el reporte combinado de RB2B), llena las dos tarjetas de una.

Estos datos son a nivel persona (nombres, LinkedIn, emails), así que **nunca se guardan en el repo**: el CSV se procesa solo en el navegador de quien lo sube, no se publica ni se persiste, y se borra al recargar la página. Los emails no se muestran. El filtro de fechas (7D/30D/90D) aplica sobre la columna `LastSeenAt`.

## Editar el diseño

Todo el CSS está en el `<style>` dentro de `index.html` (tokens de color en `:root`, con variante dark mode automática). Podés editarlo directo en GitHub (ícono de lápiz) o clonando el repo.

# Dashboard de Sistemas — Colegio San Carlos

Panel estático de una sola página que centraliza el acceso a las aplicaciones
del colegio (Google Apps Script), agrupadas por nivel educativo.

Sin frameworks, sin CDN, sin build, sin servidor: son dos archivos.

| Archivo | Para qué sirve |
|---|---|
| `index.html` | La página completa (HTML + CSS + JS en un solo archivo). |
| `links.json` | La lista de proyectos. **Es el único archivo que vas a tocar para el día a día.** |

---

## 1. Cargar tus links

Abrí `links.json` y reemplazá cada `PEGAR_LINK_AQUI` por el link de
implementación de la app (el que termina en `/exec`).

```json
{
  "nombre": "Admisión Inicial",
  "nivel": "Inicial",
  "url": "https://script.google.com/macros/s/ABC123.../exec",
  "descripcion": "Formulario de admisión para nivel inicial",
  "estado": "activo"
}
```

Campos:

- **nombre** — título de la tarjeta. Es también lo que filtra el buscador.
- **nivel** — en qué sección aparece: `Inicial`, `Primaria`, `Secundaria` o
  `Herramientas`. Cualquier otro valor (`Biblioteca`, `Administración`, …) cae
  automáticamente en *Herramientas / Otros*, así que no rompe nada.
- **url** — el link `/exec`. Si lo dejás vacío o en `PEGAR_LINK_AQUI`, la tarjeta
  se muestra igual con el botón en gris y el texto *Link pendiente*.
- **descripcion** — una línea corta.
- **estado** — `activo` (verde), `en revisión` (amarillo) o `archivado` (gris).

Para **agregar** un proyecto, copiá un bloque `{ ... }`, pegálo en la lista y
separálo con una coma. Cuidado con dos cosas típicas de JSON: comillas dobles
siempre, y sin coma después del último bloque. Si algo queda mal escrito, la
página te lo avisa arriba con el error exacto en lugar de quedarse en blanco.

El orden de las secciones es siempre Inicial → Primaria → Secundaria →
Herramientas / Otros, sin importar el orden del archivo. Dentro de cada sección
las tarjetas respetan el orden en que las escribiste. Una sección sin proyectos
no se muestra.

## 2. Publicar en GitHub Pages

### Opción A — repo nuevo (lo más simple)

1. Creá un repositorio **público** en GitHub (por ejemplo `dashboard`).
2. Subí `index.html` y `links.json` a la raíz (*Add file → Upload files*).
3. En el repo: **Settings → Pages**.
4. En *Build and deployment*: Source = **Deploy from a branch**,
   Branch = **main**, carpeta = **/ (root)** → *Save*.
5. Esperá un minuto. Queda en `https://<usuario>.github.io/<repo>/`.

### Opción B — carpeta `/docs` de un repo que ya tenés

1. Creá una carpeta `docs/` y poné adentro `index.html` y `links.json`.
2. **Settings → Pages** → Branch = **main**, carpeta = **/docs** → *Save*.
3. Queda en `https://<usuario>.github.io/<repo>/`.

> Los dos archivos tienen que quedar **juntos, en la misma carpeta**: la página
> busca `links.json` al lado de `index.html`.

Después de editar `links.json`, GitHub Pages tarda hasta un par de minutos en
actualizar. Si seguís viendo lo viejo, recargá con `Ctrl + F5`.

## 3. Ajustes rápidos

Todo lo configurable está junto, arriba del `<script>` en `index.html`:

- **Fecha de última actualización** — `ULTIMA_ACTUALIZACION`. Vacío (`""`) la
  calcula sola con la fecha del archivo; o escribila a mano: `"16/09/2026"`.
- **Secciones** — `SECCIONES`: cambiá el orden o el texto visible.
- **Estados** — `ESTADOS`: colores y etiquetas.

El nombre del colegio y el subtítulo están en el `<header>` del HTML.

## Nota sobre abrir el archivo con doble clic

Si abrís `index.html` haciendo doble clic (protocolo `file://`), el navegador
**bloquea** la lectura de `links.json` por seguridad. Para que igual veas algo,
el HTML lleva al final una copia de respaldo de los datos y un cartel que avisa
que se está usando.

Eso es sólo para esa situación: publicado en GitHub Pages siempre se lee
`links.json`. Si querés probar localmente con datos reales, levantá un servidor
en la carpeta:

```bash
python3 -m http.server 8000
# y abrí http://localhost:8000
```

## Fuera de alcance

Este panel es sólo acceso rápido a links existentes: no tiene base de datos ni
autenticación. Cualquiera con la URL ve la lista de apps (el control de acceso
lo sigue haciendo cada app de Apps Script por su cuenta).

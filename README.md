# Dashboard de Sistemas — Colegio San Carlos

Panel estático de una sola página que centraliza el acceso a las aplicaciones
del colegio (Google Apps Script), agrupadas por nivel educativo.

**En línea: https://sistemas-sancarlos.vercel.app**

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
  `General`. Cualquier otro valor (`Biblioteca`, `Administración`, …) cae
  automáticamente en *General*, así que no rompe nada.
- **url** — el link `/exec`. Si lo dejás vacío o en `PEGAR_LINK_AQUI`, la tarjeta
  se muestra igual con el botón en gris y el texto *Link pendiente*.
- **descripcion** — una línea corta.
- **estado** — `activo` (verde), `en revisión` (amarillo) o `archivado` (gris).

Para **agregar** un proyecto, copiá un bloque `{ ... }`, pegálo en la lista y
separálo con una coma. Cuidado con dos cosas típicas de JSON: comillas dobles
siempre, y sin coma después del último bloque. Si algo queda mal escrito, la
página te lo avisa arriba con el error exacto en lugar de quedarse en blanco.

El orden de las secciones es siempre Inicial → Primaria → Secundaria →
General, sin importar el orden del archivo. Dentro de cada sección
las tarjetas respetan el orden en que las escribiste. Una sección sin proyectos
no se muestra.

## 2. Publicar los cambios

El sitio vive en Vercel, en el proyecto **sistemas-sancarlos**, y se publica
desde la rama `main` de este repositorio.

### Cómo actualizar

Editás `links.json` (desde GitHub o desde tu compu) y hacés commit a `main`.

> **Importante:** hoy el repositorio **no** está conectado a Vercel, así que un
> push a `main` *no* publica solo. Ese paso hay que darlo una vez:
>
> 1. Entrá a https://vercel.com/aula-tech-s-projects/sistemas-sancarlos/settings/git
> 2. *Connect Git Repository* → elegí `aulatechsc/sistemasagus` → rama `main`.
>
> Desde ese momento, cada push a `main` publica solo en un minuto. Mientras
> tanto, los deploys se disparan a mano desde el panel de Vercel
> (*Deployments → Redeploy*).

### El logo

Si ponés un archivo **`logo.png`** junto a `index.html`, el encabezado usa el
isologo real del colegio. Si no está, queda el lockup tipográfico
(COLEGIO / SAN CARLOS) que imita la marca con tipografía serif. No hay que
tocar el HTML: el cambio es automático.

### Alternativa: GitHub Pages

Si alguna vez querés publicarlo también en Pages: **Settings → Pages**,
Source = *Deploy from a branch*, Branch = `main`, carpeta `/ (root)`.
Los dos archivos tienen que quedar juntos en la misma carpeta.

## 3. Ajustes rápidos

Todo lo configurable está junto, arriba del `<script>` en `index.html`:

- **Fecha de última actualización** — `ULTIMA_ACTUALIZACION`. Vacío (`""`) la
  calcula sola con la fecha del archivo; o escribila a mano: `"16/09/2026"`.
- **Secciones** — `SECCIONES`: cambiá el orden o el texto visible.
- **Estados** — `ESTADOS`: colores y etiquetas.

El nombre del colegio y el subtítulo están en el `<header>` del HTML.

**Paleta institucional** (arriba del todo en el `<style>`, como variables CSS):

| Token | Valor | Uso |
|---|---|---|
| `--azul` | `#00476C` | Color institucional: títulos, botones, foco |
| `--rojo` | `#E4051F` | Acento del isologo: filete del encabezado y la marca |
| `--pizarra` | `#4A6E82` | Gris azulado del recuadro "DIÁLOGOS" |

Los colores de estado (verde / ámbar / gris) son semánticos y van aparte de la
marca a propósito: el rojo institucional no se usa para "archivado" ni para
errores, para que no se confunda una cosa con la otra.

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

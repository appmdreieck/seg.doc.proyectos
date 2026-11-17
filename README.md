# Plataforma de Proyectos + app de campo

Seguimiento y documentación de proyectos (Sistema Multialerta y alertamiento sísmico).

| Página | Dirección | Para quién |
|---|---|---|
| **Plataforma en línea** | `https://appmdreieck.github.io/seg.doc.proyectos/` | Gerencia, dirección y quien sube documentación |
| **App de campo** | `https://appmdreieck.github.io/seg.doc.proyectos/app.html` | Técnicos en sitio (funciona sin internet) |

Los datos viven en la hoja índice de Google Sheets y los archivos en la carpeta de Google Drive. El servidor es un script de Google Apps Script (`apps-script/Code.gs`).

## Archivos

| Archivo | Qué es |
|---|---|
| `index.html` | Plataforma: proyectos, avance por etapa, actividades, bitácora de campo, documentos (subir a Drive), sitios, fallas y nuevo proyecto |
| `app.html` | App de campo: Proceso, Falla, Observación y Entrega con fotos, GPS y firma; guarda sin señal y envía al recuperarla |
| `config.js` | La URL del servidor (Apps Script `/exec`) |
| `sw.js`, `manifest.webmanifest`, `icon-*.png` | Permiten instalar la app en el celular y abrirla sin internet |
| `apps-script/Code.gs` | Copia del código del servidor (se pega en Apps Script, no se ejecuta desde aquí) |
| `.nojekyll` | Indica a GitHub Pages que publique los archivos tal cual |

## Publicar en GitHub Pages

1. Sube todos los archivos a la raíz del repositorio `seg.doc.proyectos` (Add file › Upload files), incluida la carpeta `apps-script`.
2. En **Settings › Pages**: *Source* = **Deploy from a branch**, *Branch* = **main** y carpeta **/ (root)**. Guarda.
3. En uno o dos minutos la plataforma queda en la dirección de arriba.

## Servidor (Apps Script)

1. Pega `apps-script/Code.gs` en el proyecto de Apps Script y ejecuta **configurar** una vez. En el registro aparecen la **clave de la plataforma** y el **código del técnico de prueba**.
2. **Implementar › Nueva implementación › Aplicación web**, *Ejecutar como*: **Yo**, *Quién tiene acceso*: **Cualquier usuario**.
3. Copia la URL `/exec` en `config.js`.
4. Si cambias el código: **Implementar › Administrar implementaciones › Editar › Nueva versión** (la URL no cambia).

## Usuarios

- **Plataforma:** hoja **Usuarios** · `nombre`, `clave`, `rol` (`admin`, `editor` o `consulta`), `activo` (`sí`/`no`).
  - `admin`: todo, incluido crear proyectos.
  - `editor`: subir documentos, mover actividades, resolver fallas, avance manual e hitos.
  - `consulta`: solo ver.
- **App de campo:** hoja **Tecnicos** · `id_tecnico` (ej. `JPEREZ`), `nombre`, `codigo` (el que escribe en la app), `activo`, `proyectos` (`*` = todos, o códigos separados por coma).
- **Asignar sitios:** columna `tecnico_asignado` de la hoja **Sitios** (con el `id_tecnico`). En la app aparecen primero.

## Fotos en la plataforma

Las miniaturas se ven si quien abre la plataforma tiene acceso a la carpeta de Drive con su cuenta de Google. Compárteles la carpeta (lector), o cambia `COMPARTIR_ARCHIVOS_CON_ENLACE` a `true` en `Code.gs` para que cualquiera con el enlace de un archivo lo vea.

## Reglas automáticas

- **Proceso** con “trabajo terminado” → el sitio cuenta como instalado (y entregado).
- **Entrega** con todos los materiales y firma → sitio entregado y acta *Firmada*; incompleta o sin firma → *Entrega parcial* / *Falta firma*.
- **Falla** sin solución → falla *Abierta* + actividad “Resolver falla” a 3 días.
- Cada registro genera su PDF en Drive con nombre `PROYECTO_SITIO_TIPO_FECHA_VERSION.pdf`.
- Hitos con condición `entregados=total` o `instalados=total` se marcan solos al cumplirse.

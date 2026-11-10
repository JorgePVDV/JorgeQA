# Portafolio QA · Jorge Alberto Paez Vargas

Portafolio web hecho solo con HTML y CSS (sin instalar nada).

## Cómo abrirlo en Visual Studio Code

1. Descomprime la carpeta y ábrela con **Archivo → Abrir carpeta**.
2. Instala la extensión **Live Server** (Ritwick Dey).
3. Clic derecho sobre `index.html` → **Open with Live Server**.
   La página se recarga sola cada vez que guardas un cambio.

## Dónde cambiar cada cosa

| Quiero cambiar… | Archivo | Qué buscar |
|---|---|---|
| Mi usuario de GitHub, correo y LinkedIn | `index.html` | Comentarios `CAMBIA` (inicio y contacto) |
| Las cifras del inicio (5 proyectos, 97 casos…) | `index.html` | `<ul class="hero-stats">` |
| Agregar un proyecto | `index.html` | Copia un `<article class="card">` completo y pégalo antes del comentario «PARA AGREGAR UN PROYECTO» |
| La barra verde/roja de resultados | `index.html` | `style="width:62.5%"` (aprobados + fallidos = 100%) |
| Colores | `styles.css` | Variables al inicio, dentro de `:root` |
| Tipografías | `styles.css` | `--font` y `--font-title` |

## Pendiente por tu parte

- La página de ajustes de GitHub (`github.com/settings/profile`) es privada y no puedo verla.
  Reemplaza `TU-USUARIO` por tu usuario público de GitHub.
- Revisa que los 5 documentos de Google estén compartidos como «Cualquier persona con el enlace»,
  si no, quien visite tu portafolio no podrá abrirlos.
- Las incidencias de Jira suelen requerir inicio de sesión; por eso no se enlazan directamente.

## Publicarlo gratis

Sube la carpeta a un repositorio de GitHub y activa **Settings → Pages**
(rama `main`, carpeta `/root`). Tu portafolio quedará en `https://TU-USUARIO.github.io/NOMBRE-REPO`.

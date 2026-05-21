# Datadelico Blog

Blog personal multilingüe construido con [Hugo](https://gohugo.io/) y desplegado en [Cloudflare Pages](https://pages.cloudflare.com/).

---

## Tecnologías y dependencias

| Herramienta | Versión | Rol |
|-------------|---------|-----|
| [Hugo](https://gohugo.io/) | `0.161.1` Extended | Generador de sitio estático |
| [hugo-coder](https://github.com/luizdepra/hugo-coder) | ~2.8.x (git submodule) | Tema |
| [Cloudflare Pages](https://pages.cloudflare.com/) | — | Hosting / CDN / despliegue |
| Markdown / Goldmark | bundled con Hugo | Formato de contenido |
| Hugo Pipes | bundled con Hugo | Pipeline de assets (SCSS, imágenes) |

> **Hugo Extended** es necesario para compilar SCSS. Compruébalo con `hugo version`; debe aparecer `+extended`.

---

## Estructura del proyecto

```
datadelico_blog/
├── archetypes/           # Plantillas de front matter para hugo new
├── config.toml           # Configuración principal del sitio
├── content/              # Contenido Markdown fuente
│   ├── _index.md         # Página de inicio (ES)
│   ├── _index.en.md      # Página de inicio (EN)
│   ├── blog/             # Entradas del blog
│   ├── quien-soy/        # Página "Quién soy / About"
│   ├── writeups/         # Write-ups de máquinas y CTFs
│   └── contacto/         # Página de contacto
├── layouts/
│   └── partials/         # Sobreescrituras de parciales del tema
├── resources/            # Caché de assets generados por Hugo Pipes (ignorado en git)
├── static/               # Archivos estáticos copiados tal cual al output
│   └── images/           # Imágenes (avatar, favicon, etc.)
├── themes/
│   └── hugo-coder/       # Tema (git submodule)
├── public/               # ⚠️ Output generado — no editar a mano (ignorado en git)
└── .gitmodules
```

---

## Primeros pasos (Getting Started)

### 1. Requisitos previos

Instala **Hugo Extended** y **git**:

```bash
# Este proyecto usa exactamente Hugo 0.161.1 (ver .tool-versions)
# Debian 13 (Trixie) se queda en 0.131.0 por apt, así que conviene usar
# el .deb oficial de Hugo Extended
sudo apt update && sudo apt install git

# Verificar que es Extended
hugo version   # debe mostrar "hugo v0.161.1+extended"
```

### Instalar exactamente la versión del proyecto (`0.161.1`)

```bash
# Eliminar una versión antigua instalada por apt (si existe)
sudo apt remove -y hugo

# Descargar e instalar Hugo Extended 0.161.1
curl -fL https://github.com/gohugoio/hugo/releases/download/v0.161.1/hugo_extended_0.161.1_linux-amd64.deb \
  -o /tmp/hugo_extended_0.161.1_linux-amd64.deb
sudo dpkg -i /tmp/hugo_extended_0.161.1_linux-amd64.deb

# Si bash sigue recordando la ruta antigua (/usr/bin/hugo)
hash -r

# Verificar versión final
hugo version
```

> La versión fijada por el proyecto está en `.tool-versions`: `hugo 0.161.1`.

<details>
<summary>Otras plataformas</summary>

```bash
# macOS (Homebrew)
brew install hugo

# Windows (Scoop)
scoop install hugo-extended

# Cualquier Linux — descargar el .deb Extended oficial de GitHub
wget https://github.com/gohugoio/hugo/releases/download/v0.161.1/hugo_extended_0.161.1_linux-amd64.deb
sudo dpkg -i hugo_extended_0.161.1_linux-amd64.deb
```
</details>

### 2. Clonar el repositorio con el submodule del tema

```bash
git clone --recurse-submodules https://github.com/datadelico/datadelico_blog.git
cd datadelico_blog
```

Si ya clonaste sin `--recurse-submodules`:

```bash
git submodule update --init --recursive
```

### 3. Servidor de desarrollo local

```bash
hugo server -D
```

El sitio estará disponible en [http://localhost:1313](http://localhost:1313).  
La flag `-D` incluye los posts en estado `draft: true`.

### 4. Crear una nueva entrada

```bash
hugo new blog/mi-nueva-entrada.md
```

El archivo se crea en `content/blog/` con `draft: true`. Edita el contenido y cambia `draft: false` cuando esté listo para publicar.

### 5. Build de producción

```bash
hugo --gc --minify
```

El sitio generado queda en `public/`. Cloudflare Pages lo despliega automáticamente desde la rama configurada.

---

## Comandos útiles de desarrollo

```bash
# Servidor local con drafts (recarga automática)
hugo server -D

# Servidor accesible desde la red local
hugo server -D --bind 0.0.0.0 --port 1313

# Build de producción (sin drafts, minificado, limpia caché)
hugo --gc --minify

# Build incluyendo drafts (para previsualizar el output final)
hugo -D --gc --minify

# Limpiar el output y regenerar desde cero
rm -rf public/ && hugo --gc --minify

# Crear una nueva entrada de blog
hugo new blog/mi-nueva-entrada.md

# Crear un nuevo write-up
hugo new writeups/nombre-de-la-maquina.md

# Actualizar el tema (submodule)
git submodule update --remote themes/hugo-coder

# Comprobar versión de Hugo (debe ser +extended)
hugo version
```

---



Los principales parámetros a personalizar:

| Parámetro | Descripción |
|-----------|-------------|
| `baseURL` | URL pública del sitio |
| `params.author` | Nombre del autor |
| `params.avatarURL` | Ruta al avatar (relativa a `static/`) |
| `params.description` / `params.info` | Descripción del sitio |
| `[[params.social]]` | Links a redes sociales |
| `[services.disqus]` | Shortname de Disqus para comentarios |
| `googleAnalytics` | ID de Google Analytics |

---

## Idiomas (Multilingual)

El sitio está configurado para dos idiomas en `config.toml`:

| Código | Idioma | Estado |
|--------|--------|--------|
| `es` | Español (por defecto) | ✅ Activo |
| `en` | English | ✅ Activo |

El contenido en español usa el archivo base (`_index.md`, `primera-entrada.md`). Las traducciones al inglés usan el sufijo `.en.md` en el mismo directorio (p. ej. `_index.en.md`). Consulta la [documentación de Hugo Multilingual](https://gohugo.io/content-management/multilingual/) para más detalles.

---

## Front matter de las entradas

Cada archivo Markdown comienza con un bloque de metadatos llamado **front matter** (delimitado por `---`). Estos son los campos disponibles:

| Campo | Descripción |
|-------|-------------|
| `title` | Título visible de la entrada |
| `date` | Fecha de publicación |
| `draft` | `true` = borrador (no se publica); `false` = publicado |
| `description` | Resumen breve que aparece en listados y SEO |
| `slug` | Fragmento final de la URL (p. ej. `slug: "intro"` → `/blog/intro/`). Si se omite, Hugo usa el nombre del archivo |
| `tags` | Etiquetas específicas del contenido (ej. `["linux", "bash"]`). Genera páginas en `/tags/<tag>/` |
| `categories` | Agrupación temática amplia (ej. `["tutoriales"]`). Genera páginas en `/categories/<cat>/` |
| `series` | Agrupa entradas relacionadas en una serie (ej. `["Mi serie"]`) |
| `authors` | Autor/es de la entrada |

### Diferencia entre `tags` y `categories`

- **`tags`**: etiquetas granulares y específicas. Pueden ser varias por entrada. Ejemplo: `bash`, `nmap`, `ctf`.
- **`categories`**: agrupación general. Normalmente 1-2 por entrada. Ejemplo: `writeups`, `tutoriales`.

> En este proyecto las secciones (`blog/`, `writeups/`) ya agrupan el contenido por tipo, por lo que `categories` es opcional. Los `tags` sí son útiles para relacionar entradas por tema.

### Ejemplo completo

```yaml
---
title: "Mi post"
date: 2025-01-01
draft: false
description: "Breve descripción del contenido."
slug: "mi-post"
tags: ["hugo", "blog"]
categories: ["tecnología"]
series: ["Mi serie"]
authors: ["datadelico"]
---
```

---

## Tema: hugo-coder

El tema se gestiona como **git submodule**:

```bash
# Actualizar al último commit del tema
git submodule update --remote themes/hugo-coder

# Ver opciones del tema
cat themes/hugo-coder/README.md
```

⚠️ No modifiques los archivos dentro de `themes/hugo-coder/` directamente. Si necesitas personalizar el tema, usa [Hugo's template lookup order](https://gohugo.io/templates/lookup-order/) copiando el archivo a la carpeta raíz del proyecto con la misma ruta.

---

## Despliegue (Cloudflare Pages)

El sitio se despliega automáticamente en Cloudflare Pages al hacer push a la rama principal.

Configuración recomendada en el dashboard de Cloudflare Pages:

| Campo | Valor |
|-------|-------|
| Framework preset | Hugo |
| Build command | `hugo --gc --minify` |
| Build output directory | `public` |
| Environment variable `HUGO_VERSION` | `0.161.1` |
| Environment variable `HUGO_ENV` | `production` |

---

## Recursos útiles

- [Documentación de Hugo](https://gohugo.io/documentation/)
- [Repositorio hugo-coder](https://github.com/luizdepra/hugo-coder)
- [Cloudflare Pages + Hugo](https://developers.cloudflare.com/pages/framework-guides/deploy-a-hugo-site/)
- [Hugo Multilingual](https://gohugo.io/content-management/multilingual/)

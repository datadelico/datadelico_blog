# Datadelico Blog

Blog personal multilingüe construido con [Hugo](https://gohugo.io/) y desplegado en [Cloudflare Pages](https://pages.cloudflare.com/).

---

## Tecnologías y dependencias

| Herramienta | Versión | Rol |
|-------------|---------|-----|
| [Hugo](https://gohugo.io/) | ≥ 0.110 (Extended recomendado) | Generador de sitio estático |
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
│   ├── _index.md         # Página de inicio
│   ├── posts/            # Entradas del blog
│   └── quien soy/        # Página "About"
├── resources/            # Assets procesados por Hugo Pipes
├── static/               # Archivos estáticos copiados tal cual al output
│   └── images/           # Imágenes (avatar, favicon, etc.)
├── themes/
│   └── hugo-coder/       # Tema (git submodule)
├── .github/
│   └── copilot-instructions.md
├── public/               # ⚠️ Output generado — no editar a mano
└── .gitmodules
```

---

## Primeros pasos (Getting Started)

### 1. Requisitos previos

Instala **Hugo Extended**:

```bash
# macOS (Homebrew)
brew install hugo

# Linux (apt)
sudo apt install hugo

# Windows (Scoop)
scoop install hugo-extended

# Verificar versión Extended
hugo version   # debe mostrar +extended
```

Asegúrate también de tener `git` instalado.

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
hugo new posts/mi-nueva-entrada.md
```

El archivo se crea en `content/posts/` con `draft: true`. Edita el contenido y cambia `draft: false` cuando esté listo para publicar.

### 5. Build de producción

```bash
hugo --gc --minify
```

El sitio generado queda en `public/`. Cloudflare Pages lo despliega automáticamente desde la rama configurada.

---

## Configuración del sitio (`config.toml`)

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

El sitio está configurado para tres idiomas en `config.toml`:

| Código | Idioma | Estado |
|--------|--------|--------|
| `es` | Español (por defecto) | ✅ Activo |
| `en` | English | 🚧 Menú definido |
| `pt-br` | Português (Brasil) | 🚧 Menú definido |

El contenido en español se ubica en `content/`. Para añadir traducciones, crea archivos con sufijo de idioma (p. ej. `primera-entrada.en.md`) o usa directorios de idioma siguiendo la [documentación de Hugo Multilingual](https://gohugo.io/content-management/multilingual/).

---

## Taxonomías disponibles

Definidas en `config.toml`. Úsalas en el front matter de tus posts:

```yaml
---
title: "Mi post"
date: 2025-01-01
draft: false
type: post
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
| Environment variable `HUGO_VERSION` | `0.147.0` (o la versión deseada) |
| Environment variable `HUGO_ENV` | `production` |

---

## Recursos útiles

- [Documentación de Hugo](https://gohugo.io/documentation/)
- [Repositorio hugo-coder](https://github.com/luizdepra/hugo-coder)
- [Cloudflare Pages + Hugo](https://developers.cloudflare.com/pages/framework-guides/deploy-a-hugo-site/)
- [Hugo Multilingual](https://gohugo.io/content-management/multilingual/)

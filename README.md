# Deployando una app React* a GitHub Pages

\* creada usando `create-react-app`

# Introducción

En este tutorial te enseñaré cómo puedes crear una app React y deployarla a GitHub Pages.

Para crear la app React usaré [`create-react-app`](https://create-react-app.dev/), una herramienta que las personas pueden usar para crear aplicaciones React desde cero. Para deployar la app React usaré [`gh-pages`](https://github.com/tschaub/gh-pages), un paquete en npm que las personas pueden usar para hacer deploys a [GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages), un servicio gratuito de hosting provisto por GitHub.

Si sigues este tutorial paso a paso tendrás una nueva app React —alojada en GitHub Pages—la cual puedes personalizar luego.

## Traducciones

Este tutorial ha sido traducido de su [versión original en Inglés](https://github.com/gitname/react-gh-pages) a los siguientes idiomas:
- [Traditional Chinese](https://github.com/gitname/react-gh-pages/issues/167#issuecomment-1925551338) (créditos: [@creaper9487](https://github.com/creaper9487))

# Tutorial

## Prerequisitos

1. [Node y npm](https://nodejs.org/en/download/) deben estar instalados. Estas son las versiones que usaré en este tutorial:

    ```shell
    $ node --version
    v16.13.2

    $ npm --version
    8.1.2
    ```
    > La instalación de npm agrega dos comandos al sistema—`npm` y `npx`—los cuales usaré luego en este tutorial.

2. [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) debe estar instalado. Esta es la versión que usaré en este tutorial:

    ```shell
    $ git --version
    git version 2.29.1.windows.1
    ```

3. Una cuenta de [GitHub](https://github.com/signup). :octocat:

## Procedimientos

### 1. Crear un repositorio **vacío** en GitHub

1. Accede a tu cuenta de GitHub.
2. Ingresa al formulario en la sección [Create a new repository](https://github.com/new).
3. Completa el formulario de la siguiente manera:
    - **Repository name:** Puedes ingresar el nombre que quieras\*.

        > \* Para un [project site](https://pages.github.com/#project-site), puedes ingresar el nombre que quieras. Para un [user site](https://pages.github.com/#user-site), GitHub [requiere](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages#types-of-github-pages-sites) que el nombre del repositorio tenga el siguiente formato: `{username}.github.io` (eejemplo: `gitname.github.io`)
        
        > El nombre que ingreses se mostrará en algunos lugares: (a) referencias al repositorio en GitHub, (b) en la URL del repositorio, y (c) en la URL de la app React deployada.

        > En este tutorial delpoyaré la app React como un project site.

        Ingresaré: `react-gh-pages`
        
   - **Repository privacy:** Seleccionar _Public_ (o _Private_\*).

        > \* Para [usuarios gratuitos de GitHub](https://docs.github.com/en/get-started/learning-about-github/githubs-products#github-free-for-user-accounts), el único tipo de repositorio que puede usarse con GitHub Pages es _Public_. Para [usuarios de GitHub Pro](https://docs.github.com/en/get-started/learning-about-github/githubs-products#github-pro) (y otros usuarios de pago), ambos tipos de repositorio _Public_ y _Private_ pueden usarse con GitHub Pages.

        Seleccionaré: _Public_

   - **Initialize repository:** Dejar todas las casillas vacías.

        > Esto hará que GitHub cree un repositorio vacío en vez de pre agregarle los archivos `README.md`, `.gitignore`, y/o `LICENSE`.
4. Enviar el formulario.

A estas alturas tu cuenta de GitHub contiene un repositorio vacío con el nombre y el tipo de privacidad que estableciste.

### 2. Crear una aplicación React

1. Crea una aplicación React llamada `my-app`:

    > En este caso quieres usar un nombre diferente a `my-app` (ejemplo: `web-ui`), puedes hacerlo reemplazando todas las ocurrencias de `my-app` en este tutorial con el nombre que elijas (ejemplo: `my-app` --> `web-ui`).
  
    ```shell
    $ npx create-react-app my-app
    ```

    > Este comando creará una aplicación React basada en JavaScript. Para crear una usando [TypeScript](https://create-react-app.dev/docs/adding-typescript/#installation), puedes usar este comando en su lugar:
    > ```shell
    > $ npx create-react-app my-app --template typescript
    > ```

    El comando creará una nueva carpeta llamada `my-app` y contendrá el código fuente de tu aplicación React.

    > Además de contener el código fuente de la aplicación React, esa carpeta es también un repositorio Git. Esa característica de la carpeta entrará en juego en el Paso 6.

    > #### Ramas: `master` vs. `main`
    > 
    > El repositorio Git tendrá una rama llamada (a) `master`, por defecto para una instalación reciente de Git; o (b) el valor de la variable configurada en `init.defaultBranch` en el caso en que tu ordenador tenga la versión de Git 2.28 o más nueva _y_ hayas [configurado la variable](https://github.blog/2020-07-27-highlights-from-git-2-28/#introducing-init-defaultbranch) en tu configuración Git (por ejemplo vía `$ git config --global init.defaultBranch main`).
    >
    > Ya que yo no configuré esa variable en mi instalación de Git, la rama en mi repositorio se llamará `master`. En el caso en que la rama en tu repositorio tenga un nombre diferente (puedes revisarlo corriendo `$ git branch`), como `main`; puedes **reemplazar** todas las ocurrencias de `master` en lo que resta del tutorial (ejemplo: `master` → `main`).

2. Ingresa a la nueva carpeta:
  
    ```shell
    $ cd my-app
    ```

A estas alturas en tu ordenador hay una aplicación React con su código fuente. Los comandos usados en lo que resta del tutorial pueden ser ejectutados desde esa carpeta.

### 3. Instalar el paquete `gh-pages`

1. Instalar el paquete [`gh-pages`](https://github.com/tschaub/gh-pages) de npm y designarlo como una [dependencia de desarrollo](https://docs.npmjs.com/specifying-dependencies-and-devdependencies-in-a-package-json-file):
 
    ```shell
    $ npm install gh-pages --save-dev
    ```

En este punto son instalados en tu ordenador el paquete de npm `gh-pages` y las dependencias de la aplicación React documentadas en su archivo `package.json`.

### 4. Agregar una propiedad de `homepage` al archivo `package.json`

1. Abrir el archivo `package.json` en un editor de texto.
   
    ```shell
    $ vi package.json
    ```

    > En este tutorial el editor de texto que usaré será [vi](https://www.vim.org/). Tu puedes usar el que desees; por ejemplo, [Visual Studio Code](https://code.visualstudio.com/).

2. Agregar la propiedad `homepage` en este formato\*: `https://{username}.github.io/{repo-name}`

    > \* Para un [project site](https://pages.github.com/#project-site) ese es el formato. Para un [user site](https://pages.github.com/#user-site) el formato es: `https://{username}.github.io`. Puedes leer más sobre la propiedad `homepage` en la [sección "GitHub Pages"](https://create-react-app.dev/docs/deployment/#github-pages) de la documentación de `create-react-app`.

    ```diff
    {
      "name": "my-app",
      "version": "0.1.0",
    + "homepage": "https://gitname.github.io/react-gh-pages",
      "private": true,
    ```
Ahora el archivo `package.json` de nuestra aplicación React incluye una propiedad llamada `homepage`.

### 5. Agrega scripts de despliegue al archivo `package.json`

1. Abre el archivo `package.json` en un editor de texto (si aún no está abierto en uno).
   
    ```shell
    $ vi package.json
    ```

2. Agrega una propiedad `predeploy` y `deploy` al objeto `scripts`:

    ```diff
    "scripts": {
    +   "predeploy": "npm run build",
    +   "deploy": "gh-pages -d build",
        "start": "react-scripts start",
        "build": "react-scripts build",
    ```

Ahora el archivo `package.json` de la aplicación React incluye scripts de despliegue.

### 6. Agrega un "remote" que apunte al repositorio en GitHub

1. Agrega un "[remote](https://git-scm.com/docs/git-remote)" al repositorio local en Git.

    Puedes hacerlo ejecutando un comando en este formato: 
    
    ```shell
    $ git remote add origin https://github.com/{username}/{repo-name}.git
    ```
    
    Para personalizar el comando según tu situación reemplaza `{username}` con tu nombre de usuario de GitHub y reemplaza `{repo-name}` con el nombre del repositorio que creaste en GitHub en el paso 1.

    En mi caso ejecutaré:

    ```shell
    $ git remote add origin https://github.com/gitname/react-gh-pages.git
    ```

    > Ese comando le indica a Git dónde quiero almacenar cosas cada vez que yo—o el paquete npm `gh-pages` lo haga por mi—ejecute `$ git push` desde ese repositorio local de Git.

En este punto el repositorio local tiene un "remote" cuya URL apunta al repositorio de GitHub que creaste en el Paso 1.

### 7. Agrega la aplicación React al repositorio en GitHub

1. Agrega la aplicación React al repositorio en GitHub

    ```shell
    $ npm run deploy
    ```

    > Eso ejecutará los scripts `predeploy` y `deploy` definidos en `package.json`.
    >
    > Por detrás el script `predeploy` creará una versión distribuible de la aplicación React y la almacenará en una carpeta llamada `build`. Luego el script `deploy` almacenará los contenidos de la carpeta a un nuevo commit en la rama `gh-pages` del repositorio en GitHub, creándola si aún no existe.

    > Por defecto el nuevo commit en la rama `gh-pages` tendrá un mensaje "Updates". Puedes [dejar un mensaje personalizado](https://github.com/gitname/react-gh-pages/issues/80#issuecomment-1042449820) usando la opción `-m` de esta manera:
    > ```shell
    > $ npm run deploy -- -m "Deploy React app to GitHub Pages"
    > ```

A estas alturas el repositorio en GitHub contiene una rama llamada `gh-pages` con los archivos que hacen a la versión distribuible de la aplicación React. Sin embargo aún no hemos configurado GitHub Pages para _mostrar_ esos archivos.

### 8. Configurar GitHub Pages

1. Dirígete a la página de opciones de **GitHub Pages**
   1. en tu navegador web, diígete al repositorio en GitHub
   1. Sobre el navegador de código, haz click en la pestaña "Settings"
   1. Al costado, en la sección "Code and automation", haz click en "Pages"
1. Configura las opciones de "Build and deployment" de la siguiente manera: 
   1. **Source**: Deploy from a branch
   2. **Branch**: 
      - Branch: `gh-pages`
      - Folder: `/ (root)`
1. Haz click en el botón "Save"

**Eso es todo!** La aplicación React ha sido desplegada a GitHub Pages! :rocket:

A estas alturas, la aplicación React es accesible para cualquiera que visite la URL de `homepage` que especificaste en el paso 4. Por ejemplo, la aplicación React que yo desplegué está disponible en https://gitname.github.io/react-gh-pages.

### 9. (Opcional) Almacena el _código fuente_ de la aplicación React en GitHub

En pasos anteriores el paquete npm `gh-pages` almacenaba la versión distribuible de la aplicación React a una rama llamada `gh-pages` en el repositorio de GitHub. Sin embargo, el _código fuente_ de la aplicación React aún no está almacenado en GitHub.

En este paso te mostraré cómo puedes almacear el código fuente de la aplicación React en GitHub.

1. Haz commit a la rama `master` de los cambios que hiciste mientras seguías este tutorial; luego almacena esa rama en la rama `master` del repositorio en GitHub.

    ```shell
    $ git add .
    $ git commit -m "Configure React app for deployment to GitHub Pages"
    $ git push origin master
    ```

    > Te recomiendo explorar el repositorio GitHub en este punto. Tendrá dos ramas: `master` y `gh-pages`. La rama `master` tendrá el códio fuente de la aplicación React, mientras que la rama `gh-pages` tendrá la versión distribuible de la aplicación React.

# Referencias

1. [The official `create-react-app` deployment guide](https://create-react-app.dev/docs/deployment/#github-pages)
2. [GitHub blog: Build and deploy GitHub Pages from any branch](https://github.blog/changelog/2020-09-03-build-and-deploy-github-pages-from-any-branch/)
3. [Preserving the `CNAME` file when using a custom domain](https://github.com/gitname/react-gh-pages/issues/89#issuecomment-1207271670)

# Notas

- Un especial agradecimiento a GitHub (la compañía) por proveernos el servicio de hosting gratuito de GitHub Pages.
- Y ahora es momento de convertir nuestra aplicación React por defecto en algo único!
- este repositorio consiste de dos ramas: 
    - `master` - the _source code_ of the React app
    - `gh-pages` - the React app _built from_ that source code

 # Contribuidores

Gracias a estas personas por contribuir al mantenimiento de este tutorial.

<!--

Template:
---------

<a href="https://github.com/____" target="_blank" title="____">
  <img src="https://github.com/____.png?size=40" height="40" width="40" alt="____" />
</a>

Instructions:
-------------

1. Copy the template and paste it below.
2. Replace the four "____" strings with the contributor's GitHub username.

Note: I specified the avatars using HTML because, when I did so using Markdown,
      only the _custom_ avatars appeared at the size I specified via the URL
      (e.g. 40px squared, for `https://github.com/gitname.png?size=40`);
      the GitHub-generated avatars seemed to ignore the size parameter and,
      instead, appear at their full size (approximately 420px squared).
      By using HTML, I can force _both_ types to appear at 40px squared.

-->

<a href="https://github.com/gitname" target="_blank" title="gitname">
  <img src="https://github.com/gitname.png?size=40" height="40" width="40" alt="gitname" />
</a>
<a href="https://github.com/rhulse" target="_blank" title="rhulse">
  <img src="https://github.com/rhulse.png?size=40" height="40" width="40" alt="rhulse" />
</a>
<a href="https://github.com/AbhishekCode" target="_blank" title="AbhishekCode">
  <img src="https://github.com/AbhishekCode.png?size=40" height="40" width="40" alt="AbhishekCode" />
</a>
<a href="https://github.com/adnjoo" target="_blank" title="adnjoo">
  <img src="https://github.com/adnjoo.png?size=40" height="40" width="40" alt="adnjoo" />
</a>
<a href="https://github.com/thebeatlesphan" target="_blank" title="thebeatlesphan">
  <img src="https://github.com/thebeatlesphan.png?size=40" height="40" width="40" alt="thebeatlesphan" />
</a>
<a href="https://github.com/valerio-pescatori" target="_blank" title="valerio-pescatori">
  <img src="https://github.com/valerio-pescatori.png?size=40" height="40" width="40" alt="valerio-pescatori" />
</a>
<a href="https://github.com/jackweyhrich" target="_blank" title="jackweyhrich">
  <img src="https://github.com/jackweyhrich.png?size=40" height="40" width="40" alt="jackweyhrich" />
</a>

Esta lista es mantenida manualmente-por el momento-e incluye (a) cada persona que envió un pull request y fue eventualmente fusionado en `master`, y (b) cada persona que contribuyó en alguna manera distinta (por ejemplo con feedback constructivo) y que me permitió incluirla en esta lista.
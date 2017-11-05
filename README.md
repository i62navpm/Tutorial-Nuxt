<p align="center"><img align="center" src="http://imgur.com/V4LtoII.png"/></p>

**Tutorial de NUXT**
====================
> Tutorial de [Nuxt](https://nuxtjs.org) en español

Índice
--------
1. [¿Qué es el SSR o aplicaciones universales?](#isomorfica)
    1. [Ventajas y desventajas](#isomorfica-ventajas)
    2. [Alternativas](#isomorfica-alternativas)
2. [¿Qué es Nuxt?](#que-nuxt)
    1. [¿Cómo funciona?](#que-nuxt-funciona)
    2. [Características](#que-nuxt-caracteristicas)
    3. [Ciclo de trabajo](#que-nuxt-ciclo)
    4. [Tipos de renderizado](#que-nuxt-renderizado)
3. [Guía de inicio](#guide)
    1. [Instalación](#guide-instalacion)
    2. [Estructura del directorio](#guide-directorio)
    3. [Jerarquía de vistas](#guide-jerarquia)
    3. [Routing](#guide-routing)
    4. [Store](#guide-store)
    5. [Plugins](#guide-plugin)
    6. [Assets](#guide-assets)
    7. [Despliegue](#guide-despliegue)
4. [Ejemplo](#ejemplo)
5. [Ejemplo AWS Serverless](#ejemplo-aws)

# <a id="isomorfica"></a>¿Qué es el SSR o aplicaciones universales?
Cualquier framework de JavaScript (*Vue, React, Angular, …*), generan componentes que por defecto producen y manipulan DOM en el navegador como salida. El ***Server Side Rendering*** o SSR, nos ofrece la posibilidad de convertir los mismos componentes en cadenas de HTML en el servidor, enviarlos directamente al navegador y generar una aplicación en el navegador del cliente.

Una aplicación JavaScript **isomórficas** o  **universal**, es aquella en la que su código puede ser interpretado, tanto en la parte de cliente (navegador) como en la parte de servidor (ex. [NodeJS](https://nodejs.org)).

En el caso de Nuxt, su principal ventaja es el renderizado UI (*interfaces de usuario*) abstrayendo al usuario la complejidad de saber si su código se está compilando en cliente o en servidor.

## <a id="isomorfica-ventajas"></a>Ventajas y desventajas
### 🙂 Ventajas

En comparación con un SPA tradicional (aplicación de una sola página), la ventaja de la SSR radica principalmente en:

- **Mejor SEO**, ya que los rastreadores de los motores de búsqueda verán directamente la página completamente renderizada.

    A partir de ahora motores de búsqueda como Google pueden indexar aplicaciones de JavaScript síncronas sin problemas.
    
    ¿Qué quiere decir aplicaciones síncronas?🤔
    
    Si la aplicación comienza con un proceso de carga de datos a través de una petición Ajax, el crawler no esperará a que termine. Esto significa que si tiene contenido obtenido de manera asíncrona en páginas donde SEO es importante, es muy posible que se necesite SSR.
Aunque Google es capaz de hacer scraping en las aplicaciones SPA (*más info [aquí](https://goralewicz.com/blog/javascript-seo-experiment/)*).

    > “Times have changed. Today, as long as you're not blocking Googlebot from crawling your JavaScript or CSS files, [we are generally able to render and understand your web pages like modern browsers.](https://webmasters.googleblog.com/2014/05/understanding-web-pages-better.html) To reflect this improvement, we recently [updated our technical Webmaster Guidelines](https://webmasters.googleblog.com/2014/10/updating-our-technical-webmaster.html) to recommend against disallowing Googlebot from crawling your site's CSS or JS files.”

- **Tiempo de carga de contenido más rápido**, especialmente en Internet lento o dispositivos lentos. La generación de HTML en el servidor no necesita esperar hasta que se haya descargado y ejecutado todo el JavaScript para que se muestre, de modo que su usuario verá una página completamente renderizada antes. En general, esto se traduce en una mejor experiencia del usuario.

### 🙁 Desventajas

También hay algunos inconvenientes que considerar al usar SSR:

- **Restricciones de desarrollo**. El código específico del navegador solo se puede usar dentro de ciertas etapas del ciclo de vida del component; algunas bibliotecas externas pueden necesitar un tratamiento especial para poder ejecutarse en una aplicación procesada por el servidor.

- **Requisitos de instalación y despliegue más complicados**. A diferencia de un SPA totalmente estática que se puede implementar en cualquier servidor de archivos estático, una aplicación procesada por servidor requiere un entorno donde se puede ejecutar un servidor Node.js.

- **Más carga en el servidor**. Es obvio que renderizar una aplicación completa en Node.js va a requerir más CPU que solo servir archivos estáticos, por lo que si espera mucho tráfico, hay que preparar el entorno para la carga del servidor correspondiente y manejar eficientemente las estrategias de almacenamiento en caché.

## <a id="isomorfica-alternativas"></a>Alternativas

Si quieres conseguir las ventajas sobre SEO que ofrece SSR pero sin todas las desventajas que éste ofrece, entonces probablemente quieras usar otra herramienta en su lugar, esta herramienta se llama ***Prerendering***.

El ***Prerender*** en lugar de utilizar un servidor web para compilar HTML sobre la marcha, simplemente genera archivos HTML estáticos para rutas específicas en tiempo de compilación. La ventaja de usar *prerender* es que la configuración inicial es mucho más simple y nos permite mantener nuestro frontal como un sitio totalmente estático.
 
Si estás utilizando [**webpack**](https://webpack.github.io/), puedes agregar fácilmente *prerendering* con [prerender-spa-plugin](https://github.com/chrisvfritz/prerender-spa-plugin).

# <a id="que-nuxt"></a>¿Qué es Nuxt?

> The 25th of October 2016, the team behind [zeit.co](https://zeit.co/), announced [Next.js](https://zeit.co/blog/next), a framework for server-rendered React applications. A few hours after the announcement, the idea of creating server-rendered Vue.js applications the same way as Next.js was obvious: **Nuxt.js** was born.


📚 Nuxt.js es un *framework* que nos permite crear aplicaciones universales con [Vue.js](https://vuejs.org/). Nos permite crear componentes UI si tener que concentrarse en las distribuciones de cliente y servidor.

Nuxt nos proporciona una capa de abstracción por encima de [Vue Server Renderer](https://ssr.vuejs.org/en/), haciéndonos más fácil y rápido el proceso de configuración y el desarrollo de aplicaciones isomórficas.

## <a id="que-nuxt-funciona"></a>¿Cómo funciona?
<p align="center"><img align="center" src="https://i.imgur.com/avEUftE.png"/></p>

Nuxt.js usa el siguiente stack tecnológico para la creación de aplicaciones:

- **[Vue 2](https://vuejs.org/)**

    Framework progresivo para la generación de SPA (*single page applications*).

    ¿Qué quiere decir progresivo?🤔

    Quiere decir que VUE está diseñado para ser adoptado incrementalmente, desde pequeñas aplicaciones simples hasta aplicaciones SPA más elaboradas.

- **[Vue Router](https://router.vuejs.org/)**
    
    Nos da el servicio de *routing* para el cambio de vistas en la aplicación.
- **[Vuex](https://vuex.vuejs.org/)**
    
    Gestión de estados centralizados de Vue.js
- **[Vue Server Renderer](https://ssr.vuejs.org)**
    
    Librería que nos permite ejecutar código en servidor y cliente.
- **[vue-meta](https://github.com/declandewet/vue-meta)**
    
    Librería que nos permite gestionar la meta información de la aplicación desde los componentes de Vue + SSR

El peso total del framework es de 57kB min+gzip (53kB con Vuex).

En el proceso de desarrollo Nuxt utiliza webpack con **vue-loader** y **babel-loader** para la generación de bundles.

## <a id="que-nuxt-caracteristicas"></a>Características

- **Escritura de código en ficheros VUE** (*.vue)

- **Separación automática de código** (*Code Splitting*)

    ¿Qué quiere decir esto?

    Es una característica muy potente que nos ofrece **webpack** que consiste en dividir el código en varios paquetes, que luego pueden cargarse bajo demanda o en paralelo.

- **Renderizado en servidor**

    Las vistas de la aplicación serán renderizadas en el servidor. Todos los cambios dinámicos son interceptados por Vue en cliente. Nuxt nos hace el SSR transparente.

- **Sistema de routing con sincronismo de datos**

    Nuxt configurará el routing de la aplicación dependiendo de la estructura de carpetas que hayamos creado, además nos proporciona herramientas para la carga síncrona de datos y vistas en servidor.

- **Servicio de ficheros estáticos**

    Ofrece capacidad de servir ficheros estáticos desde servidor.

- **Transpilación de ES6/ES7**

    Webpack junto con [Babel](https://babeljs.io) nos ofrece la posibilidad de transpilar el código escrito en un estándar moderno a código que el navegador pueda entender.

    Todos conocemos las ventajas de ES6 pero cuáles son las novedades de ES7 (ES2016):
	- Object Rest / Spread Properties

        Object Rest
        ```javascript
        const {a, b, c, ...x} = {a: 1, b: 2, c: 3, x: 4, y: 5, z: 6};

        console.log(a); // 1
        console.log(b); // 2
        console.log(c); // 3
        console.log(x); // { x: 4, y: 5, z: 6 }
        ```

        Spread properties
        ```javascript
        const a = 1, b = 2, c = 3
        const x = { x: 4, y: 5, z: 6 }
        const obj = { a, b, c, ...x }

        console.log(obj) //{a: 1, b: 2, c: 3, x: 4, y: 5, z: 6};
        ```
	- Observables
	
        Los observables son una nueva herramienta que nos permitirá adentrarnos en la programación reactiva, es decir, programar con flujos de datos (streams) asíncronos.
        ```javascript
        const resize = new Observable((o) => {
          window.addEventListener("resize", () => {
            let height = window.innerHeight
            let width = window.innerWidth
            o.next({ height, width })
          })
        })
        ```

	- Async Functions
	
        Nueva manera para resolver la asincronía en Javascript.
        ```javascript
        async function createEmployeeWorkflow() {
          try {
            let employee = await createEmployee()
            await saveEmployee(employee)
          } catch (err) {
            throw new Error(err)
          }
        }
        ```

- **Bundling y minificación de JS & CSS**

- **Pre-procesador: Sass, Less, Stylus, etc.**

- **Manejo de los elementos de la etiqueta \<head> (<title>, \<meta>, etc.)**

    Nos permite cambiar la meta información desde cualquier componente Vue

- **Reemplazo de módulos de desarrollo en caliente**

    Con esta opción sólo recargamos aquellos módulos donde hemos realizado cambios, evitando así cargar toda la página para mostrar los cambios de un único componente. Evitando así pérdida de tiempo en el desarrollo.
- **HTTP/2 push headers ready**

    Con esta funcionalidad, el servidor nos enviará los assets antes de que el navegador pregunte por ellos.

- **Entesión de Nuxt con arquitecturas modulares**

    Podemos extender la funcionalidad de Nuxt con arquitecturas modulares.


## <a id="que-nuxt-ciclo"></a>Ciclo de trabajo

Este esquema muestra que hace Nuxt.js cuando se llama al servidor o cuando el usuario navega a través de la aplicación usando <nuxt-link>:
<p align="center"><img align="center" src="https://nuxtjs.org/nuxt-schema.png"/></p>

1. Un usuario realiza una petición de una ruta determinada a servidor.
2. El servidor ejecuta la acción nuxtServerInit  del store principal si la tiene implementada. Esta acción nos permite cargar datos iniciales (*prefetching* de datos globales).
3. Se ejecutan todos aquellos middlewares que se encuentren en el fichero de configuración nuxt.config.js y los relacionados con el layout, la página raíz y las páginas hijas coincidentes que se hayan implementado.
4. Si existe un validador, se ejecuta. Si se resuelve con un true se sigue el proceso, si no se devuelve un 404 😭.
5. Se obtienen aquellos datos de la página para que sean renderizados.
6. Se renderiza en servidor y se sirve al usuario.
7. Si el usuario navega por la aplicación hacia otra ruta, se repite el ciclo.

    > *Info sacada del blog ["El abismo de null"](https://elabismodenull.wordpress.com/2017/07/25/vuejs-aplicaciones-universales-con-nuxt/)*

## <a id="que-nuxt-renderizado"></a>Tipos de renderizado

Nuxt.js puede configurarse de tres formas diferentes para generar aplicaciones:
- **Server Rendered** (*Universal SSR*)

    Podemos usar Nuxt.js como framework para manejar todo el renderizado UI de nuestro proyecto.

- **Single Page Applications** (*SPA*)

    Si por alguna razón no necesitamos el renderizado en servidor, podemos habilitar la opción de SPA y usar Nuxt como si trabajáramos con Vue.

- **Static Generated** (*Pre Rendering*)
	
    Una de las grandes innovaciones de Nuxt es esta funcionalidad, lo que nos permite generar los estáticos de nuestro proyecto y poder alojarlos en algún CDN (*content delivery network*) con toda la mejora de SEO que nos proporciona Nuxt.

# <a id="guide"></a>Guía de inicio
## <a id="guide-instalacion"></a>Instalación
Podemos comenzar a instalar nuestro proyecto Nuxt desde un template ya creado, este template ya nos dará todo el scaffolding básico necesario para crear nuestra aplicación. Tan sólo debemos de tener [vue-cli](https://github.com/vuejs/vue-cli) instalado en nuestra máquina.

> Prerequisitos: Tener instalado Node.js y npm

1. Instalamos vue-cli:

	``` bash
	$ npm install -g vue-cli
	```

2. Una vez instalado vue-cli comenzamos con la instalación del template predefinido:

	``` bash
	$ vue init nuxt-community/starter-template <nombre-proyecto>
	```

	Si queremos que nuestro proyecto ya tenga una librería de componentes acoplada en el template de Nuxt, como es el caso de **[Vuetify](https://vuetifyjs.com/)**, debemos hacer:

	``` bash
	$ vue init vuetifyjs/nuxt <nombre-proyecto>
	```

3. Una vez instalado el template, comenzamos a instalar las dependencias:

	``` bash
	$ cd <nombre-proyecto>
	$ npm install
	```

4. Por último lanzamos el proyecto en modo desarrollo:

	``` bash
	$ npm run dev
	```

	La aplicación estará ejecutándose 💻 en [http://localhost:3000](http://localhost:3000)

## <a id="guide-directorio"></a>Estructura del directorio

El *scaffolding* del template de Nuxt nos genera un total de 8 carpetas más 1 archivo de configuración junto con el archivo *package.json*:
### Carpetas

1. La carpeta de ***assets***

	La carpeta de assets contiene los archivos no compilados como *Less, Sass o JavaScript*.

2. La carpeta de **componentes**

	La carpeta de componentes contiene los componentes de Vue.js. Nuxt.js no sobrecarga el método *data* en estos componentes.

3. La carpeta de ***layouts***

	La carpeta de *layouts* contiene todos los *layouts* de la aplicación.

	###### *Este directorio no puede ser renombrado.*

4. La carpeta de ***middlewares***

	La carpeta de *middleware* contiene los middlewares de la aplicación. Un middleware nos permite definir funciones personalizadas que se pueden ejecutar antes de visualizar una página o un grupo de páginas (*layouts*).

5. La carpeta de **páginas**

	La carpeta de páginas contiene las vistas de aplicaciones y sus rutas. El framework lee todos los archivos .vue dentro de este directorio y crea el enrutador de la aplicación.

	###### *Este directorio no puede ser renombrado.*

6. La carpeta de ***plugins***

	La carpeta de *plugins* contiene los complementos de Javascript que se desean ejecutar antes de crear una instancia de la aplicación raíz Vue.js.

7. La carpeta de **estáticos**

	La carpeta de estáticos contiene tus archivos estáticos. Cada archivo dentro de este directorio está mapeado a /.

	> Ejemplo: /static/robots.txt se asigna como /robots.txt 🤖

	###### *Este directorio no puede ser renombrado.*

8. La carpeta de los ***stores***

	La carpeta de la *stores* contiene sus archivos de almacenes Vuex 🏭.

	###### *Este directorio no puede ser renombrado.*

### Archivos de configuración

1. El archivo ***nuxt.config.js***

	El archivo nuxt.config.js 🛠 contiene la configuración personalizada de Nuxt.js.

	###### *Este archivo no puede ser renombrado.*

2. El archivo ***package.json***

	El archivo package.json contiene las dependencias y scripts de la aplicación.

	###### *Este archivo no puede ser renombrado.*

## <a id="guide-jerarquia"></a>Jerarquía de vistas
<p align="center"><img align="center" src="https://nuxtjs.org/nuxt-views-schema.png"/></p>

En Nuxt.js existen tres niveles de jerarquía de vistas:
- Document
	- Layout
		- Page

### Document

Existe sólo un único *Document* del que parten el resto de vistas, este es el archivo `.nuxt/views/app.template.html`.

Podremos sobrecargar la funcionalidad de este documento añadiendo un archivo `app.html` situado en el directorio raíz del proyecto.

### Layout

Nuxt.js nos permite ampliar el *layout* principal o crear nuevos *layouts* personalizados insertándolos en la carpeta `layouts` de nuestro proyecto.

¿Qué es un *layout*?🤔

La layout o plantilla es un esquema de la distribución de los elementos dentro de una página web. Se compone de una serie de bloques de ciertas dimensiones en los que se colocará el contenido.

1. **Layout por defecto**

	Podemos ampliar la funcionalidad del *layout* principal agregando o modificando el archivo `layouts/default.vue`.

	Debemos asegurarnos de incorporar el componente `<nuxt />` al crear un *layout* para mostrar el componente de la página.

2. **Página de error**

	Al igual que el *layout* por defecto tambien podemos personalizar la página de error añadiendo o modificando el archivo `layouts/error.vue`.

3. **Nuevos *layouts***

	Podemos incluir nuevas plantilas de elementos. Cada archivo añadido en el directorio de *layouts* creará un *layout* personalizado y accesible con la propiedad *layout* al resto de páginas.

	Al igual que en el layout por defecto, no debemos olvidar el `<nuxt />` para mostrar las diferentes pginas que tengan asociado este nuevo layout.

### Page

Cada componente de tipo página es un componente de Vue en el que Nuxt.js añade funcionalidad especial para hacer el desarrollo de la aplicación universal lo más fácil posible

ATRIBUTO	| DESCRIPCIÓN
--------  | -----------
asyncData	| El atributo más importante, puede ser una propiedad asíncrona y recibe el contexto de la aplicación como argumento. Se usa para conseguir datos y renderizarlos en servidor sin usar un *store*. Esta propiedad se llama cada vez, antes de cargar el componente.
fetch	| Esta propiedad se usa para completar el *store* antes de renderizar la página.
head | Sete Meta Tags específicos para la página actual.
layout	| Especifica el *layout* al que corresponde la página.
transition	| Nombre de la animación de transición que usa el componente de página.
scrollToTop	| Booleano, para hacer scroll al inicio de la página antes de renderizarla.
validate | Función validadora usada para rutas dinámicas.
middleware | Uso de un middleware para la página, este middleware se llamara antes de renderizar la página.

## <a id="guide-routing"></a>Routing

Nuxt.js genera automáticamente la configuración de `vue-router` basándose en la estructura en árbol de los archivos del directorio `pages`.

Existen 4 tipos de enrutamiento:

### 1. Rutas básicas

El árbol de archivos:
```
pages/
--| user/
-----| index.vue
-----| one.vue
--| index.vue
```

genera la siguiente configuración:

```javascript
router: {
  routes: [
    {
      name: 'index',
      path: '/',
      component: 'pages/index.vue'
    },
    {
      name: 'user',
      path: '/user',
      component: 'pages/user/index.vue'
    },
    {
      name: 'user-one',
      path: '/user/one',
      component: 'pages/user/one.vue'
    }
  ]
}
```

### 2. Rutas dinámicas

Para definir una ruta dinámica con un parámetro, necesitamos definir un **archivo *.vue* o un directorio subrayado como prefijo**.

El árbol de archivos:
```
pages/
--| _slug/
-----| comments.vue
-----| index.vue
--| users/
-----| _id.vue
--| index.vue
```

genera la siguiente configuración:
	
```javasacript
router: {
  routes: [
    {
      name: 'index',
      path: '/',
      component: 'pages/index.vue'
    },
    {
      name: 'users-id',
      path: '/users/:id?',
      component: 'pages/users/_id.vue'
    },
    {
      name: 'slug',
      path: '/:slug',
      component: 'pages/_slug/index.vue'
    },
    {
      name: 'slug-comments',
      path: '/:slug/comments',
      component: 'pages/_slug/comments.vue'
    }
  ]
}
```
	
Como se puede ver, la ruta `users-id` tiene la variable `:id?`opcional, si queremos que sea obligatoria, debemos crear un archivo `index.vue`  en el directorio `users/_id`.

##### Función validadora de parámetros
Nuxt.js nos permite añadir un método de aprobación dentro de nuestro componente de ruta dinámica.

### 3. Rutas anidadas

Nuxt.js nos permite crear rutas anidadas usando las rutas hijas de `vue-router`.
Para definir el componente padre de una ruta anidad, necesitamos crear un archivo `.vue` con el mismo nombre que el directorio que contenga las vistas hijas. 

‼️**Importante** no olvidar incluir el componente `<nuxt-child/>` dentro del componente padre (archivo `.vue`)

El árbol de archivos:
```
pages/
--| users/
-----| _id.vue
-----| index.vue
--| users.vue
```

genera la siguiente configuración:

```javascript
router: {
  routes: [
    {
      path: '/users',
      component: 'pages/users.vue',
      children: [
	{
	  path: '',
	  component: 'pages/users/index.vue',
	  name: 'users'
	},
	{
	  path: ':id',
	  component: 'pages/users/_id.vue',
	  name: 'users-id'
	}
      ]
    }
  ]
}
```

### 4. Rutas anidadas dinámicas

Este escenario no suele darse mucho, pero sería posible teniendo vistas hijas dinámicas dentro de vistas dinámicas padre.

El árbol de archivos:
```
pages/
--| _category/
-----| _subCategory/
--------| _id.vue
--------| index.vue
-----| _subCategory.vue
-----| index.vue
--| _category.vue
--| index.vue
```

genera la siguiente configuración:

```javascript
router: {
  routes: [
    {
      path: '/',
      component: 'pages/index.vue',
      name: 'index'
    },
    {
      path: '/:category',
      component: 'pages/_category.vue',
      children: [
	{
	  path: '',
	  component: 'pages/_category/index.vue',
	  name: 'category'
	},
	{
	  path: ':subCategory',
	  component: 'pages/_category/_subCategory.vue',
	  children: [
	    {
	      path: '',
	      component: 'pages/_category/_subCategory/index.vue',
	      name: 'category-subCategory'
	    },
	    {
	      path: ':id',
	      component: 'pages/_category/_subCategory/_id.vue',
	      name: 'category-subCategory-id'
	    }
	  ]
	}
      ]
    }
  ]
}
```

#### Transiciones

Nuxt.js nos permite usar el componente `<transition>` para dejarnos crear diferentes animaciones de transición entre rutas.

#### Middleware

Los middlewares nos dejan definir funciones personalizadas que pueden ser ejecutadas antes de renderizar una página o un grupo de ellas.
Cada middleware estará situado dentro del directorio `middleware/`, el nombre del archivos será el nombre del middleware.

## <a id="guide-store"></a>Store

Para el manejo de estados usaremos Vuex, Nuxt.js implementa Vuex en su core.

### Activar Store

Para activar Vuex simplemente debe de existir la carpeta `store`dentro del directorio del proyecto, si no existe esta carpeta, entonces no se importa la librería Vuex.

### Formas de crear *stores*

Existen dos maneras de usar los *stores* en Nuxt:

#### 1. Clásico

Para activar el store con el modo clásico, simplemente tenemos que crear el archivo `store/index.js` el cual debe exportar un método que devuelve una instancia de Vuex.

#### 2. Módulos

Nuxt nos permite tener un conjunto de *stores* correspondiendo cada uno de los ficheros dentro de la carpeta `store` a un módulo. Si usamos esta opción, tendremos que exportar los estados como una función y las mutaciones y acciones como objetos, en vez de como una instancia Vuex tal y como se hace en el modo clásico

### La acción *nuxtServerInit*

Si la acción *nuxtServerInit* está definida en el store, Nuxt.js llamará a este método desde el contexto del servidor. Es muy útil cuando tenemos datos en el servidor que queremos mandar directamente al lado del cliente.

## <a id="guide-plugin"></a>Plugins

Nuxt.js nos permite definir plugins de Javascript para ser lanzados antes de antes de la instanciación de la aplicación Vue.js. Esto nos sirve de ayuda cuando usamos nuestras librerías o módulos externos.

> Es muy ‼️importante saber que en el ciclo de vida de una instancia de Vue, sólo los eventos ***beforeCreate*** y ***created*** pueden ser ejecutados tanto en el lado servidor como en el lado cliente, el resto de eventos sólo se llamarán desde el lado de cliente.

### Paquetes externos

Es muy frecuente que nos encontremos en la situación de querer usar un módulo en diferentes componentes de la aplicación.

Hay un **inconveniente** con esto y es que si volvemos a importar ese módulo en otro componente, se volverá a incluir el bundle completo de ese módulo. Si sólo queremos una instanciación de ese módulo por aplicación entonces debemos de incluir ese módulo en el apartado `build.vendor` dentro de nuestro fichero `nuxt.config.js`

### Vue Plugins

Si queremos usar plugins en nuestra aplicación, necesitamos configurar el plugin antes de lanzar la aplicación.

Esto se puede hacer siguiendo los siguientes pasos:

#### 1. Crear un archivo dentro de la carpeta `plugins`

```javascript
import Vue from 'vue'
import VueNotifications from 'vue-notifications'

Vue.use(VueNotifications)
```

#### 2. Añadimos el paquete dentro de la clave `plugins` del archivo `nuxt.config.js`

```javascript
module.exports = {
  plugins: ['~/plugins/vue-notifications']
}
```

> Podemos querer que ese módulo se encuentre dentro del bundle de la app, porque se trate de una librería que vamos a usar en toda la aplicación, para ello lo incluimos dentro del *vendor* de *bundle* para un mejor **cacheo de la librería**.

### Inyección en $root & context

Algunos plugins necesitan ser inyectados en el root de la aplicación para ser usados. Con Nuxt.js, podemos usar la `app` disponible dentro del contexto cuando exportemos el método.

#### 1. Crear un archivo dentro de la carpeta `plugins`

```javascript
import Vue from 'vue'
import VueI18n from 'vue-i18n'

Vue.use(VueI18n)

export default ({ app }, inject) => {
  // Set `i18n` instance on `app`
  // This way we can use it in middleware and pages `asyncData`/`fetch`
  app.i18n = new VueI18n({
    /* `VueI18n` options... */
  })
}
```

#### 2. Añadimos el paquete dentro de la clave `plugins` del archivo `nuxt.config.js`

```javascript
module.exports = {
  build: {
    vendor: ['vue-i18n']
  },
  plugins: ['~/plugins/i18n.js']
}
```

> Podemos querer que ese módulo se encuentre dentro del bundle de la app, porque se trate de una librería que vamos a usar en toda la aplicación, para ello lo incluimos dentro del *vendor* de *bundle* para un mejor **cacheo de la librería**.

### Sólo lado de cliente

Algunos plugins podrían funcionar sólo del lado servidor, bien por que accedan a la variable `window`, porque necesiten `localStorage` o almacenamiento en `cookies`, etc.

Si es el caso podemos configurar el plugin para que sólo funcione en el lado de cliente, esto se hace insertando una propiedad `ssr: false`dentro del archivo `config.nuxt.js`.

```javascript
module.exports = {
  plugins: [
    { src: '~/plugins/vue-notifications', ssr: false }
  ]
}
```

## <a id="guide-assets"></a>Assets

Por defecto, Nuxt usa los loaders `vue-loader`, `file-loader` y `url-loader` de *webpack* para el servicio de assets. Pero también podemos servir estáticos desde la carpeta `static`.

Estos dos servicios para servir estáticos son:

### 1. *Webpacked*
Los recursos son administrados por *webpack*.

### 2. *Static*
Subidos directamente a la carpeta `static` y servidos directamente desde ahí, sin la intervención de *webpack*

## <a id="guide-despliegue"></a>Despliegue

### Lista de comandos

COMANDO | DESCRIPCIÓN
--------  | -----------
nuxt | Lanza un servidor de desarrollo en *localhost:3000* con *hot-reloading*
nuxt build | Crear la aplicación con wepack y minifica el JS & CSS (para producción)
nuxt start | Arranca el servidor en modo producción (después de haber ejecutado *nuxt build*)
nuxt generate | Genera la aplicación y genera cada ruta como un archivo HTML (usado para la generación de estáticos)

### Despliegue en producción

Nuxt.js nos permite elegir entre tres modos de desplegar nuestra aplicación:

#### 1. Server rendered deployment (Universal)

Para desplegar, en lugar de ejecutar `nuxt`, probablemente queramos construir antes de tiempo. Por lo tanto construir y ejecutar son dos comandos separados.

```bash
nuxt build
nuxt start
```

#### 2. SPA

Para desplegar en modo SPA, debemos hacer lo siguiente:
	1. Cambiar el atributo `mode` en el archivo `nuxt.config.js` a `spa`.
2. Lanzar `npm run build`
3. Desplegar la carpeta `dist/` en el servidor web correspondiente.

#### 3. Generación de estáticos

Nuxt.js nos da la habilidad de servir nuestra aplicación web desde cualquier *hosting* estático.

Para generar nuestra aplicación web en archivos estáticos:

```bash
npm run generate
```

Con esto crearemos la carpeta `dist` que tendrá todo listo para ser desplegado en nuestro servidor estático.


# <a id="ejemplo"></a>Ejemplo

Vamos a hacer un ejercicio de *live coding*

# <a id="ejemplo-aws"></a>Ejemplo AWS Serverless

Comentaremos como hemos hecho una aplicación con Nuxt.js basándonos en una arquitectura **Serverless de AWS**.

Puede verse en el repositorio: [myImageProject](https://github.com/i62navpm/myImageProject)

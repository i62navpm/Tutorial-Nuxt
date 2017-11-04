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

Si quieres conseguir las ventajas sobre SEO que ofrece SSR pero sin todas las desventajas que éste ofrece, entonces probablemente quieras usar otra herramienta en su lugar, esta herramienta se llama *Prerendering*.

El *Prerender* en lugar de utilizar un servidor web para compilar HTML sobre la marcha, simplemente genera archivos HTML estáticos para rutas específicas en tiempo de compilación. La ventaja de usar *prerender* es que la configuración inicial es mucho más simple y nos permite mantener nuestro frontal como un sitio totalmente estático.
 
Si estás utilizando [**webpack**](https://webpack.github.io/), puedes agregar fácilmente *prerendering* con [prerender-spa-plugin](https://github.com/chrisvfritz/prerender-spa-plugin).

# <a id="que-nuxt"></a>¿Qué es Nuxt?
## <a id="que-nuxt-funciona"></a>¿Cómo funciona?
## <a id="que-nuxt-caracteristicas"></a>Características
## <a id="que-nuxt-ciclo"></a>Ciclo de trabajo
## <a id="que-nuxt-renderizado"></a>Tipos de renderizado
# <a id="guide"></a>Guía de inicio
## <a id="guide-instalacion"></a>Instalación
## <a id="guide-directorio"></a>Estructura del directorio
## <a id="guide-jerarquia"></a>Jerarquía de vistas
## <a id="guide-routing"></a>Routing
## <a id="guide-store"></a>Store
## <a id="guide-plugin"></a>Plugins
## <a id="guide-assets"></a>Assets
## <a id="guide-despliegue"></a>Despliegue
# <a id="ejemplo"></a>Ejemplo
# <a id="ejemplo-aws"></a>Ejemplo AWS Serverless

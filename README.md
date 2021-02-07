# composer-isastur
Mini curso interno de uso de composer

## Inicio
* clonar este repositorio en una carpeta
* El proyecto tiene dos carpetas
  * App: donde estarán los ficheros de nuestra aplicación
  * tests: donde alojaremos los unittests
* Además, tenemos dos ficheros que en este curso no estudiaremos
  * phpcs.xml: Configuración de phpcs "Code sniffer"
  * phpunit.xml: configuración de phpunit

## Por qué usar composer
* Gestión de paquetes es un estandar en la industria
* Permite instalar / actualizar fácilmente las librerías de forma automática
  * Arbol de dependencias. Intala las dependencias de una libreria
  * Sitio central donde están las librerías (Pakagist)
* Facilidad para pasar el proyecto a otra persona
  * Aseguramos que usamos las mismas libs, misma versión
* Permite instalar librerías solo en entorno de desarrollo y no en producción : phpunit, phpcs
* Generacion de autoload 


## Puntos a estudiar
* Instalación
  * Global o local al proyecto
  * Instalador windows / línea de comandos / copiar .phar
* .gitignore: añadir composer.phar, carpeta /vendor/
* Añadir librerías
  * composer init o mejor partir de un template
  * Desde cmd: require / o mejor editar composer.json
  * Por defecto se instalan en la carpeta /vendor pero se puede cambiar con clave en composer.json
  * Ver definición simple de un require
  * Require y require-dev
  * composer.lock
* Actualizar nuestras dependencias
  * Composer install / composer install --no-dev
  * composer update / composer update --no-dev
* Autoload
  * Composer permite generar un autoload para nuestra app. Deben usarse namespace
  * require_once 'vendor/autoload.php' en nuestros ficheros o nuestro prepend
  * Autoload y autoload-dev
* Scripts
  * Permite automatizar lanzar comandos de utilidad en el proyecto. Como unos alias
  * Por ejemplo lanzar los tests, phpcs, limpiar la carpeta applicationlogs
* Actualizar el propio composer: composer self-update
* Extra: crear un nuevo proyecto php en base a un template publico
  * composer.phar create-project
    * Ejemplo: composer create-project codelytv/php-bootstrap your-project-name
  * Podríamos crear librerías públicas en pakagist o tener nuestro propio repo privado compartido de librerías. Se puede registrar un repo privado en composer

## Pasos
* Clonar el repo base : https://github.com/huanhose/composer-isastur.git
* Instalar composer. Lo tenemos ya instalado pero podriamos tener uno .phar local al proyecto
* Crearemos un .gitignore en incluimos  
  * composer.phar
  * /vendor/
* COMMIT
* Creamos composer.json
  * Añadimos nuestro primer require : "php": "^7.1"
  * Así solo instalarán versiones de librerías compatibles con esa versión de php
* Autoload  en app/ y autoload-dev en tests/ 
```php  
      "autoload": {
        "psr-4": {
            "App\\": "app/"
        }
    },

    "autoload-dev": {
        "psr-4": {
            "Tests\\": "tests/"
        }
    },
```    
* 2 requires de devel
  * phpunit : Lo hacemos desde cmd: composer require --dev "phpunit/phpunit". Nos selecciona la ultima versión
  * phpcs : Lo escribimos en composer.json: "**squizlabs/php_codesniffer**": "*" la última que haya
  * Nos creará los comandos a usar en /vendor/bin 
    * Comprobar lo que hay bajo vendor. Todo lo que se ha bajado
    * Ver lo que hay disponibla bajo /vendor/bin
* COMMIT 
* lanzamos composer install
* A partir de ahora los comandos composer que lancemos desde esta carpeta leerán composer.json
* creamos clase en /app  metodo suma de 2 numeros y su test correspondiente
  * usar parámetros tipados asi como en valor devuelto
* lanzamos los test de la aplicación. Ya tenemos un phpunit.xml. No hablaremos de el por ahora
* COMMIT
* Añadimos la libreria guzzle , un require de producción
  * composer require guzzlehttp/guzzle
  * O meterlo a mano
  * composer update
  * codigo de ejemplo : metodo en nuestra clase que devuelva el getStatusCode : 
```php
    $client = new \GuzzleHttp\Client();
    $response = $client->request('GET', 'https://api.github.com/repos/guzzle/guzzle');

    echo $response->getStatusCode(); // 200
    echo $response->getHeaderLine('content-type'); // 'application/json; charset=utf8'
    echo $response->getBody(); // '{"id": 1420053, "name": "guzzle", ...}'
```    
 * Le añadimos un test chorras 
 * COMMIT
 * script en composer.json para ejecutar phpunit y phpcs
   * probamos a lanzar composer test y composer cs
 * COMMIT
 * Ejercicio: script deletelogs: delete de la carpeta /app/logs



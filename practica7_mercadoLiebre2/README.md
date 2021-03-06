# Mercado Liebre

## Journal

- MVC model
- Views are given
- Start with model on sequelize
-   npm install --save sequelize
-   npm install --save mysql2
- create .sequelizrc (config file):
-      const path = require('path')module.exports = { config: path.resolve('./database/config', 'config.js'), 'models-path': path.resolve('./database/models'), 'seeders-path': path.resolve('./database/seeders'), 'migrations-path': path.resolve('./database/migrations'),}

- check if sequelize-cli is installed
- run sequelize init
- check database/config.js. Db credencials
- export all database properties module.export = {developement, test, prouduction}
- create User and Product models
- User name, email, password etc, and for product same taht productsDataBase.json
- Models ready, I'll create the database with migrations and seeders

## Jesus migrations data

* SEQUELIZE: MIGRATIONS & SEEDERS
* JESÚS ARCE
* 13/07/2020
#### Documentación oficial: https://sequelize.org/master/manual/migrations.html
#### Documentación alternativa: https://rosolutions.com.mx/blog/index.php/2018/08/06/como-usar-un-orm-en-node-js/

## MIGRATIONS
0- Pre-requisitos: necesitamos tener la base de datos creada para ejecutar los pasos siguientes.

1- Procedemos a crear nuestro primer modelo:

$ sequelize model:create --name Producto --attributes nombre:string,descripcion:string,precio:float,archivado:date

	Nota: el comando anterior creará un archivo de modelo bajo el directorio /models y un archivo de migración bajo el directorio /migrations.

	Importante: Es preciso revisar el archivo de la migración porque seguramente se van a tener que modificar algunos campos, por ejemplo ponerle un "allowNull: 	false," despues del type, a los campos que consideremos que no aceptan valor nulo...

2- Inicializamos el servicio de base de datos.

3- Ejecutamos las migraciones:

$ sequelize db:migrate

	Nota: el comando anterior ejecuta todas las migraciones nuevas que hayamos creado.

	Nota 2: podemos deshacer la ejecución de la última migración mediante el siguiente comando: $ sequelize db:migrate:undo
	También podemos deshacer la ejecución de todas las migraciones con el siguiente comando: $ sequelize db:migrate:undo:all

4- Si queremos insertar datos de ejemplo de forma automática en nuestra DB entonces ejecutamos algunos Seeders (ver SEEDERS a continuación)


## SEEDERS
Vamos a agregar algunos seeders para crear datos de prueba de manera sencilla.

1- Primero creamos un nuevo seeder con el comando:
$ sequelize seed:generate --name Producto

2- Abrimos el archivo que se generó dentro de la carpeta database/seeders, y agregamos los datos de prueba:
--- 
'use strict';
module.exports = {
  up: (queryInterface, Sequelize) => {
   return queryInterface.bulkInsert('productos', [
     {
      nombre: 'Placa madre ASUS P5KPL-CM',
      descripcion: 'Placa base. Bla bla bla....',
      precio: 8.325,
      archivado: null,
      createdAt: new Date(),
      updatedAt: new Date()
      },
      {
      nombre: 'MSI GL62m 7RD',
      descripcion: 'Notebook Gamer',
      precio: 235.325,
      archivado: new Date(),
      createdAt: new Date(),
      updatedAt: new Date()
      },
   ], {});
  },

  down: (queryInterface, Sequelize) => {
    // cuidado! este método elimina TODOS los registros de la tabla (no solamente los datos de prueba)... 
    return queryInterface.bulkDelete('productos', null, {});
  }
};
--- 

3- Crear los seeders para las demás tablas

4- Ejecutar todos los seeders "up" con el siguiente comando:

$ sequelize db:seed:all

	Nota: si el comando anterior no funciona, probar indicarle dónde se encuentra sequelize, mediante el siguiente comando: 
	$ node_modules/.bin/sequelize db:seed:all

	NOTA: también se puede ejecutar un seeder "up" específico utilizando:
	$ node_modules/.bin/sequelize db:seed --seed database/seeders/nombre-del-seeder.js

5- Si queremos ejecutar el método "down" de todos los seeders, tenemos que usar el siguiente comando: 

$ sequelize db:seed:undo:all   ¡CUIDADO!: el comando anterior va a eliminar todos los registros de todas las tablas que tienen seeders! (todos los registros, no solamente los que agregamos a través del seeder). Para eliminar sólo los datos de prueba hay que especificarlos en la función down, de la siguiente manera:
---
down: (queryInterface, Sequelize) => {
    const Op = Sequelize.Op;
    return queryInterface.bulkDelete(
      'productos',
      {[Op.or]: [ // eliminamos sólo los datos de prueba
        {nombre: 'Placa madre ASUS P5KPL-CM'},
        {nombre: 'MSI GL62m 7RD'}
      ]}
    );
  }
---

Importante: para ejecutar un seeder "down" específico, usamos el siguiente comando:
$ sequelize db:seed:undo --seed database/seeders/nombre-del-seeder.js



ADICIONAL -> UMZUG

Umzug es una librería que permite ejecutar las migraciones y los seeders desde código JS (no por consola) al levantar la aplicación.
https://github.com/sequelize/umzug



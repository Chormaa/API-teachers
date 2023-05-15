# API-teachers

TypeScript Teacher API
Esta API está diseñada para gestionar profesores utilizando TypeScript, NodeJS, Express y Prisma. Sigue principios de arquitectura limpia y está construida usando programación orientada a objetos (POO) para los estudiantes de la UPB.

Características
TypeScript
NodeJS
Express
Swagger
Clean Architecture
Object-oriented programming (OOP)
Prisma ORM
Paso a paso
Crear una base de datos en ElephantSql ya que usaremos Postgres
Ejecutar el siguiente script para crear una tabla en la base de datos:
CREATE TABLE teacher (
  id uuid DEFAULT gen_random_uuid() PRIMARY key,
  name TEXT NOT NULL,
  description TEXT,
  email TEXT UNIQUE NOT NULL,
  birth_date DATE NOT NULL
);
Crear un repositorio y clonarlo. Este repositorio debe tener al menos un README.md creado desde el github.
Generar .gitignore recomendado haciendo uso de gitignore.io
Crear un archivo llamado .gitignore en el proyecto y pegar el contenido generado anteriormente.
Inicializar el proyecto con npm ejecutando el comando npm init -y
Instalar dependencias:
Dependencias del proyecto: npm install @prisma/client cors dotenv express morgan reflect-metadata
Dependencias de desarrollo: npm install -D @types/cors @types/express @types/morgan @types/node @types/swagger-jsdoc @types/swagger-ui-express npm-run-all prisma swagger-jsdoc swagger-ui-express ts-node ts-node-dev tsc-watch typescript
Ejecutar el comando tsc --init para inicializar un proyecto TypeScript. Al ejecutar este comando, se crea un archivo de configuración llamado tsconfig.json en el directorio actual del proyecto. Este archivo contiene la configuración predeterminada para el proyecto TypeScript y permite personalizar las opciones del compilador TypeScript según las necesidades del proyecto.
Modificar archivo tsconfig.json:
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "CommonJS",
    "outDir": "./dist",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "baseUrl": "./src",
    "paths": {
      "*": ["*", "node_modules/*"]
    },
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true
  },
  "include": ["src/**/*.ts"],
  "exclude": ["node_modules"]
}
Crea un archivo .env en el directorio raíz y configúralo con las credenciales de tu base de datos. Esta credencial debe llamarse DATABASE_URL="postgres://usuario:contraseña@servidor/baseDatos?schema=public"
Inicializa prisma ejecutando el comando: npx prisma init
Crear la siguiente estructura de carpetas y archivos para el proyecto
├── node_modules/
├── src/
│   ├── app/
│   │   ├── controllers/
│   │   │   └── TeacherController.ts
│   │   ├── middlewares/
│   │   │   ├── swagger/
│   │   │   │   ├── swagger.ts
│   │   │   │   └── swaggerConfig.ts
│   │   │   ├── cors.ts
│   │   │   └── logger-http.ts
│   │   └── routes/
│   │       └── TeacherRoutes.ts
│   ├── config/
│   │   └── index.ts
│   ├── domain/
│   │   ├── entities/
│   │   │   └── Teacher.ts
│   │   └── interfaces/
│   │       └── ITeacherRepository.ts
│   ├── infra/
│   │   └── repositories/
│   │       └── TeacherRepository.ts
│   ├── app.ts
│   ├── index.ts
│   ├── schema.prisma
│   └── server.ts
├── .env
├── .gitignore
├── package.json
├── README.md
└── tsconfig.json
Para crear esta encarpetado haz uso de los scripts

./scripts/create_directories.bat: en Windows haz doble clic en create_directories.bat para crear la estructura de directorios.
./scripts/create_directories.sh: en macOs o Linux ejecuta chmod +x create_directories.sh en la terminal para hacerlo ejecutable y luego ejecuta ./create_directories.sh para crear la estructura de directorios.
Tener en cuenta que en esta estructura, la carpeta que se creo automáticamente usando el comando para inicializar prisma se debe borrar y el archivo que estaba por dentro de esta carpeta debe pasar a ser parte de sel source, quedando en la siguiente ruta: ./src/schema.prisma
Agregar el siguiente contenido al archivo ./src/scheme.prisma:
// This is your Prisma schema file,
// learn more about it in the docs: https://pris.ly/d/prisma-schema

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model teacher {
  id          String  @id @default(dbgenerated()) @db.Uuid
  name        String
  description String?
  email       String  @unique
  birthDate   DateTime @map(name: "birth_date")
}
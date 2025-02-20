# Disclaimer

1.- Para hacer esto, he interactuado con el API de OPENAI, como es imposible tener el API en un repositorio público, les comparto el archivo .env

2.- Los audios generados, los pueden ubicar dentro del servidor, en la ruta generated/audios para ejecución en local y en la carpeta /tmp en AWS en la nube

3.- Para ver el desarrollo con el frontend ir a: https://github.com/Alanfermin/challenge-frontend-swapi.git

3.1.- Si desea probar el chatbot sin configuración, puede ir a:

https://reto.alanfermin.com/get-vehicles

Nota: No está optimizado para mobile. Es un breve desarrollo para el reto

![Chatbot con Node, Nest, AWS, y React JS](![Logo de GitHub](https://alanfermin.com/chatbot-Alan-Fermin.png)
)

4.- Para ver un video del backend y el frontend funcionando: https://www.youtube.com/watch?v=qvBTch-qzAw


# Documentación de Endpoints

Esta documentación describe cómo consumir los endpoints disponibles para interactuar con SWAPI y OpenAI para la generación y manipulación de datos de películas.

## Endpoints Disponibles

### Obtener Listas de Recursos

Puedes acceder a las listas de diferentes recursos disponibles en SWAPI usando los siguientes endpoints:

- **GET** `/swapi/get-films`  
  URL: `https://9wpykorpvl.execute-api.us-east-2.amazonaws.com/production/swapi/get-films`

- **GET** `/swapi/get-people`  
  URL: `https://9wpykorpvl.execute-api.us-east-2.amazonaws.com/production/swapi/get-people`

- **GET** `/swapi/get-planet`  
  URL: `https://9wpykorpvl.execute-api.us-east-2.amazonaws.com/production/swapi/get-planet`

- **GET** `/swapi/get-species`  
  URL: `https://9wpykorpvl.execute-api.us-east-2.amazonaws.com/production/swapi/get-species`

- **GET** `/swapi/get-starships`  
  URL: `https://9wpykorpvl.execute-api.us-east-2.amazonaws.com/production/swapi/get-starships`

- **GET** `/swapi/get-vehicles`  
  URL: `https://9wpykorpvl.execute-api.us-east-2.amazonaws.com/production/swapi/get-vehicles`

### Obtener un Elemento Específico

Para obtener un recurso específico, añade el parámetro `id` en los siguientes endpoints:

- **GET** `/swapi/get-films?id={id}`
- **GET** `/swapi/get-people?id={id}`
- **GET** `/swapi/get-planet?id={id}`
- **GET** `/swapi/get-species?id={id}`
- **GET** `/swapi/get-starships?id={id}`
- **GET** `/swapi/get-vehicles?id={id}`

### Crear una Nueva Historia con OpenAI

Para generar una nueva historia utilizando OpenAI y recursos de SWAPI, y guardarla en la base de datos:

La url: https://9wpykorpvl.execute-api.us-east-2.amazonaws.com/production/ es la generada por el vscode, esta se cambia según la que genere el servidor

**POST** `/new-film/new-film-with-sawapi-data`  
URL: `https://9wpykorpvl.execute-api.us-east-2.amazonaws.com/production/new-film/new-film-with-sawapi-data`

**Body Example:**
```json
{
  "idFilm": 5,
  "idPerson": "33",
  "idPlanet": "9",
  "idSpecies": "6",
  "idStarship": "5",
  "idVehicle": "7",
  "emocion": "drama de novela"
}
```

### Crear un audio de una Nueva Historia con OpenAI

Para generar una nueva historia utilizando OpenAI y recursos de SWAPI, y guardarla en la base de datos:

**POST** `/new-film/text-to-audio`  
URL: `https://9wpykorpvl.execute-api.us-east-2.amazonaws.com/production/new-film/text-to-audio`

**Body Example:**
```json
{
  "idFilm": 5,
  "idPerson": "33",
  "idPlanet": "9",
  "idSpecies": "6",
  "idStarship": "5",
  "idVehicle": "7",
  "emocion": "drama de novela",
  "selectedVoice": "echo"
}
```

### Obtener Todos los Films Generados por OpenAI

Esta URL es se va a cambiar por la que genere aws: https://9wpykorpvl.execute-api.us-east-2.amazonaws.com/production/

**GET** `/swapi/films`  
URL: `https://9wpykorpvl.execute-api.us-east-2.amazonaws.com/production/swapi/films`

## Consideraciones Importantes

### Endpoints Inaccesibles de SWAPI

Estos endpoints retornan errores y deben ser evitados:

- Starships: IDs `1`, `6`
- Vehicles: IDs `1`, `2`, `3`, `9`, `10`, `11`, `12`

Si se solicita un recurso desde `/swapi/get-starships` o `/swapi/get-vehicles` con estos IDs, se recibirá un error.

### Restricciones de Parámetros

- `idFilm`: debe ser un número entre `1` y `7`.
- `idVehicle`: los IDs `1`, `2`, `3`, `9`, `10`, `11`, `12` no están disponibles.
- `idStarship`: el ID `1` no está disponible.

### Ejemplos de Peticiones POST No Operativas

```json
{
  "idFilm": 12,
  "idPerson": "12",
  "idPlanet": "12",
  "idSpecies": "12",
  "idStarship": "1",
  "idVehicle": "1",
  "emocion": "amor"
}
```

## Despliegue de la Aplicación

1. Instalar `awscli`.
2. Ejecutar `aws configure` y ajustar las credenciales.
3. Revisar y modificar, si es necesario, la configuración de región en el archivo `serverless.yml`, línea 6.
4. npm build
5. Desplegar usando: `serverless deploy`
6. Para ver offline:
```bash
serverless offline
```

## Pruebas Unitarias

Para ejecutar las pruebas unitarias, primero corre la aplicación localmente:
```bash
npm run test:watch
```



Luego usa:

```bash
npm run test
```

Para modo observador:

```bash
npm run test:watch
```

Este markdown ahora ofrece una presentación más profesional y clara de la información. La estructura jerárquica y el formato mejorados ayudan a los desarrolladores a seguir fácilmente las instrucciones y entender los detalles importantes del servicio y sus restricciones.

# DATO IMPORTANTE
## Variables 
Las variables de entorno Están en el archivo .env que les he pasado. Por favor agregar .env al archivo raíz y al archivo serverless.yml en la clave: OPENAI_API_KEY
## Audios
Para escuchar y generar los audios en local:

Bien sea con npm:
```bash
npm run star:dev
```
Con serverless en local
```bash
sls offline
```

### Debe cambiar estas líneas en: src/new-film/use-cases/textToAudio.use-cases.ts

Debe descomentar la línea 25 y comentar la línea 26
  // const folderPath = path.resolve(__dirname, '../../../generated/audios/') //Para correr en local
  const folderPath = '/tmp'; // Para correr en AWS


### Si desea trabajar con AWS en la nube(Lamda), este archivo: src/new-film/use-cases/textToAudio.use-cases.ts

Debe estar tal cual:
// const folderPath = path.resolve(__dirname, '../../../generated/audios/') //Para correr en local
  const folderPath = '/tmp'; // Para correr en AWS

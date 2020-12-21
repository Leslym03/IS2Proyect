# IS2 Proyecto Final
## Proyecto Final: Arquitectura de Integración Continua
Implantar un entorno de construcción automática de software utilizando prácticas de integración continua (CI/CD):

## Proyecto de Software 
Usar un proyecto de software (preferentemente una web app o mobile app) relativamente complejo y popular disponible en GitHub: Se uso el proyecto [JSpaint](https://github.com/1j01/jspaint) es un remake de Paint basado en web y aplicacion de escritorio, incluye características poco conocidas ), mejorarlo y ampliar los tipos de imágenes que puede editar. Sus caracteristicas mas resaltantes son:
- Multiplataforma
- Undos / redos ilimitados
- El historial de deshacer no es lineal
- Mantiene una copia de seguridad 
- Edita imágenes transparentes
- Cambia de temas (modo oscuro)
- Modo Cuadro de color vertical 
- Modo Eye Gaze
- Modo de reconocimiento de voz
- Crea un GIF animado a partir del historial
- Posee estilo Asteroids
- Puede recortar la imagen
- Atajos de teclado para rotación
- Zoom a una escala arbitraria
- Relleno no contiguo
- Corrector ortográfico
- Soporte rudimentario para múltiples usuarios
- Carga muchos formatos de paleta diferentes
- Soporte táctil
  
## Repositorio  
Usar Git, GitHub o GitLab como repositório de Código Fuente: Se creo un [repositorio en Github](https://github.com/Leslym03/IS2Proyect) con nos colaboradores que son las integrantes del grupo:
- Lesly Mita Yagua [Lesly M](https://github.com/Leslym03)
- Kemely Castillo Caccire [kemely2018](https://github.com/kemely2018)

## Pipeline Jenkins
Para el Proyecto, implementar um pipeline de CI/CD en Jenkins: El codigo se puede ver en el archivo [Jenkinsfile](https://github.com/Leslym03/IS2Proyect/blob/main/JSPaint/Jenkinsfile) 
  ```
  
  ```
El pipeline contiene las siguientes tareas:
  

### Construccion automatica
El actual proyecto ya contaba con construccion automatica en [JSON](https://github.com/Leslym03/IS2Proyect/blob/main/JSPaint/package.json) para Javascript
  
```javascript
{
  "name": "jspaint",
  "productName": "JS Paint",
  "version": "1.0.0",
  "description": "Classic MS Paint clone with extra features",
  "keywords": [
    "paint",
    "jspaint",
    "mspaint",
    "drawing",
    "draw",
    "create",
    "image",
    "picture",
    "editor",
    "edit",
    "canvas",
    "app",
    "web-app",
    "recreation",
    "clone",
    "image-editing",
    "image-editor",
    "bitmap"
  ],
  "author": "Isaiah Odhner <isaiahodhner@gmail.com>",
  "main": "src/electron-main.js",
  "config": {
    "forge": {
      "packagerConfig": {},
      "makers": [
        {
          "name": "@electron-forge/maker-squirrel",
          "config": {
            "name": "jspaint"
          }
        },
        {
          "name": "@electron-forge/maker-zip",
          "platforms": [
            "darwin"
          ]
        },
        {
          "name": "@electron-forge/maker-deb",
          "config": {}
        },
        {
          "name": "@electron-forge/maker-rpm",
          "config": {}
        }
      ]
    }
  },
  "dependencies": {
    "electron-is-dev": "^1.1.0",
    "electron-squirrel-startup": "^1.0.0",
    "wallpaper": "4.4.1"
  },
  "devDependencies": {
    "@electron-forge/cli": "6.0.0-beta.45",
    "@electron-forge/maker-deb": "6.0.0-beta.45",
    "@electron-forge/maker-rpm": "6.0.0-beta.45",
    "@electron-forge/maker-squirrel": "6.0.0-beta.45",
    "@electron-forge/maker-zip": "6.0.0-beta.45",
    "concat-glob-cli": "^0.1.0",
    "cross-spawn": "^7.0.0",
    "cypress": "4.7.0",
    "cypress-image-snapshot": "3.1.1",
    "devtron": "^1.4.0",
    "electron": "6.0.10",
    "electron-debug": "^3.0.1",
    "eslint": "6.4.0",
    "live-server": "^1.2.1",
    "serve": "11.2.0",
    "start-server-and-test": "1.10.6"
  },
  "scripts": {
    "start": "electron-forge start",
    "debug-start": "electron-forge start --inspect-electron",
    "package": "electron-forge package",
    "make": "electron-forge make",
    "publish": "electron-forge publish",
    "lint": "eslint src/",
    "lint-cat": "concat-glob-cli --files \"src/**/!(electron*).js\" --output concatenated-source.js && eslint --rule \"no-undef: error\" --rule \"no-unused-vars: error\" concatenated-source.js",
    "lint-cat:NOTE": "Disable the eslint comment that disables ThisExpression to use this.",
    "dev": "live-server --ignorePattern=\"(node_modules|cypress|out)[/\\\\\\\\]|package\\.json|cypress\\.json\"",
    "dev:NOTE": "@XXX: the octuple backlash ends up meaning a single backslash on Linux, two backslashes on Windows. In this case it's fine because it's in a regexp character class so the extra is redundant and doesn't cause an error.",
    "test:start-server": "live-server --port=11822 --no-browser --ignorePattern=\"(node_modules|cypress|out)[/\\\\\\\\]|package\\.json|cypress\\.json\"",
    "test:start-server:NOTE": "@XXX: the octuple backlash ends up meaning a single backslash on Linux, two backslashes on Windows. In this case it's fine because it's in a regexp character class so the extra is redundant and doesn't cause an error.",
    "cy:open": "cypress open",
    "cy:run": "cypress run --record",
    "cy:accept": "cypress run --env updateSnapshots=true",
    "test": "start-server-and-test test:start-server http://localhost:11822 cy:run",
    "accept": "start-server-and-test test:start-server http://localhost:11822 cy:accept"
  },
  "repository": {
    "type": "git",
    "url": "https://github.com/1j01/jspaint.git"
  },
  "bugs": {
    "url": "https://github.com/1j01/jspaint/issues"
  }
}
```
  
### Analisis Estatico 
La herramienta a usa es SonarQube y El codigo se puede ver en el archivo 
- Cree un archivo de configuración en el directorio raíz del proyecto: 
       [sonar-project.properties](https://github.com/Leslym03/IS2Proyect/blob/main/JSPaint/sonar-project.properties)
       ```
       # must be unique in a given SonarQube instance
       sonar.projectKey=JSPaint
       # --- optional properties ---
       # defaults to project key
       #sonar.projectName=JSPaint
       # defaults to 'not provided'
       #sonar.projectVersion=1.0
       
       # Path is relative to the sonar-project.properties file. Defaults to .
       sonar.sources=.
       sonar.exclusions=*.gitignore, *.git
       
       # Encoding of the source code. Default is default system encoding
       
       #sonar.sourceEncoding=UTF-8
       ```
       
- Ejecute el siguiente comando desde el directorio base del proyecto para iniciar el análisis: ```sonar-scanner.bat```
- Visualizar resultados de SonarScanner en SonarQube: http://localhost:9000
  
### Pruebas Unitarias:
  
### Pruebas Funcionales:
  
### Despliegue Automatico:
  

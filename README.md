# IS2 Proyecto Final
## Proyecto Final: Arquitectura de Integración Continua
Implantar un entorno de construcción automática de software utilizando prácticas de integración continua (CI/CD):

## Proyecto de software
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
  pipeline {
    agent any

    stages {
        stage('Dependencies') {
            steps {
                sh 'npm i'
            }
        }
        stage('Build') {
            steps {
                sh 'npm run make'
            }
        }
        stage('Test') {
            steps {
                sh 'npm run lint'
                sh 'npm test'
            }  
        }

    }
  }
  
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
  
### Analisis Estatico: 
La herramienta a usa es SonarQube .
- Ejecutar SonarQube localmente
   </p>   
   <p align="center">
     <img width="40%" height="30%" src="https://github.com/Leslym03/IS2Proyect/blob/main/img/a.PNG">
   </p> 
- Ejecutar SonarScanner
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
 </p>   
 <p align="center">
    <img width="50%" height="30%" src="https://github.com/Leslym03/IS2Proyect/blob/main/img/b.PNG">
 </p> 
 </p>   
 <p align="center">
    <img width="50%" height="30%" src="https://github.com/Leslym03/IS2Proyect/blob/main/img/c.PNG">
 </p>          

- Visualizar resultados de SonarScanner en SonarQube: en este [PDF](https://drive.google.com/file/d/1Y6Zu-l-tHfvnWwEoWcqtWyengG05MMyT/view?usp=sharing)<br/>  
http://localhost:9000
</p>   
<p align="center">
 <img width="60%" height="50%" src="https://github.com/Leslym03/IS2Proyect/blob/main/img/d.PNG">
</p> 
  
  
### Pruebas Unitarias y Pruebas Funcionales
Para realizar las pruebas tanto unitarias como funcionales se utilizo la herramienta **Cypress** ya que es un framework incluye librerías de aserciones, de mocks y pruebas e2e automáticas sin utilizar Selenium. Detrás de Cypress se ejecuta un proceso Node que constantemente se comunica, sincroniza y ejecuta tareas, teniendo acceso tanto a la parte front como a la parte back de la aplicación y respondiendo a los eventos en tiempo real.

Primero se identifico los casos de prueba, los cuales se encuentra en un [Excel](https://docs.google.com/spreadsheets/d/1eWlFOBNvzTJae3vXMyRuuX7Wi5z7oQF6J-DPbBsw-vE/edit?usp=sharing) en el cual se dividen en las secciones:
- Tool test

```javascript
/// <reference types="Cypress" />

context('tool tests', () => {
	const roundedToolsCompareOptions = {
		failureThreshold: 13,
		failureThresholdType: 'pixel'
	};
	let before_first_real_test = true;
	it(`(fake test for setup)`, () => {
		cy.visit('/')
		cy.setResolution([800, 500]);
		cy.window().should('have.property', 'colors'); 
		before_first_real_test = false;
	});
	beforeEach(() => {
		if (before_first_real_test) return;
		cy.window().then({timeout: 60000}, async (win)=> {
			win.colors.foreground = "#000";
			win.colors.background = "#fff";
			win.brush_shape = win.default_brush_shape;
			win.brush_size = win.default_brush_size
			win.eraser_size = win.default_eraser_size;
			win.airbrush_size = win.default_airbrush_size;
			win.pencil_size = win.default_pencil_size;
			win.stroke_size = win.default_stroke_size;
			win.clear();
		});
	});

	const simulateGesture = (win, {start, end, shift, shiftToggleChance=0.01, secondary, secondaryToggleChance, target}) => {
		target = target || win.$(".main-canvas")[0];
		let startWithinRect = target.getBoundingClientRect();
		let canvasAreaRect = win.$(".canvas-area")[0].getBoundingClientRect();
	
		let startMinX = Math.max(startWithinRect.left, canvasAreaRect.left);
		let startMaxX = Math.min(startWithinRect.right, canvasAreaRect.right);
		let startMinY = Math.max(startWithinRect.top, canvasAreaRect.top);
		let startMaxY = Math.min(startWithinRect.bottom, canvasAreaRect.bottom);
		let startPointX = startMinX + start.x * (startMaxX - startMinX);
		let startPointY = startMinY + start.y * (startMaxY - startMinY);
		let endPointX = startMinX + end.x * (startMaxX - startMinX);
		let endPointY = startMinY + end.y * (startMaxY - startMinY);
	
		const $cursor = win.$(`<img src="images/cursors/default.png" class="user-cursor"/>`);
		$cursor.css({
			position: "absolute",
			left: 0,
			top: 0,
			opacity: 0,
			zIndex: 5, // @#: z-index
			pointerEvents: "none",
			transition: "opacity 0.5s",
		});
		$cursor.appendTo(".jspaint");
		let triggerMouseEvent = (type, point) => {
			
			const clientX = point.x;
			const clientY = point.y;

			const do_nothing = false;
			$cursor.css({
				display: "block",
				position: "absolute",
				left: clientX,
				top: clientY,
				opacity: do_nothing ? 0.5 : 1,
			});
			if (do_nothing) {
				return;
			}
	
			let event = new win.$.Event(type, {
				view: window,
				bubbles: true,
				cancelable: true,
				clientX,
				clientY,
				screenX: clientX,
				screenY: clientY,
				offsetX: point.x,
				offsetY: point.y,
				button: secondary ? 2 : 0,
				buttons: secondary ? 2 : 1,
				shiftKey: shift,
			});
			win.$(target).trigger(event);
		};
	
		let t = 0;
		const stepsInGesture = 3;
		let pointForTime = (t) => {
			return {
				x: startPointX + (endPointX - startPointX) * t,
				y: startPointY + (endPointY - startPointY) * Math.pow(t, 0.3),
			};
		};
		
		return new Promise((resolve)=> {
			triggerMouseEvent("pointerenter", pointForTime(t)); // so dynamic cursors follow the simulation cursor
			triggerMouseEvent("pointerdown", pointForTime(t));
			let move = () => {
				t += 1 / stepsInGesture;
				if (t > 1) {
					triggerMouseEvent("pointerup", pointForTime(t));
					
					$cursor.remove();
		
					resolve();
				} else {
					triggerMouseEvent("pointermove", pointForTime(t));
					/*gestureTimeoutID =*/ setTimeout(move, 10);
				}
			};
			triggerMouseEvent("pointerleave", pointForTime(t));
			move();
		});
	};

	it(`eraser tool`, () => {
		cy.get(`.tool[title='Eraser/Color Eraser']`).click();
		cy.window().then({timeout: 60000}, async (win)=> {
			for (let row=0; row<4; row++) {
				const secondary = !!(row % 2);
				const increaseSize = row >= 2;
				let $options = win.$(`.chooser > *`);
				for (let o=0; o<$options.length; o++) {
					$options[o].click();
					if (increaseSize) {
						for (let i = 0; i < 5; i++) {
							win.$('body').trigger(new win.$.Event("keydown", {key: "NumpadPlus", keyCode: 107, which: 107}));
						}
					}
					win.colors.background = "#f0f";
					const start = {x: 0.05 + o*0.05, y: 0.1 + 0.1*row};
					const end = {x: start.x + 0.04, y: start.y + 0.04};
					await simulateGesture(win, {shift: false, secondary: false, start, end});
					if (secondary) {
						win.colors.background = "#ff0";
						win.colors.foreground = "#f0f";
						const start = {x: 0.04 + o*0.05, y: 0.11 + 0.1*row};
						const end = {x: start.x + 0.03, y: start.y + 0.02};
						await simulateGesture(win, {shift: false, secondary: true, start, end});
					}
				}
			}
		});
		cy.get(".main-canvas").matchImageSnapshot();
	});

	["Brush", "Pencil", "Rectangle", "Rounded Rectangle", "Ellipse", "Line"].forEach((toolName)=> {
		it(`${toolName.toLowerCase()} tool`, () => {
			cy.get(`.tool[title='${toolName}']`).click();
			cy.get(".swatch:nth-child(22)").rightclick();
			cy.window().then({timeout: 60000}, async (win)=> {
				for (let row=0; row<4; row++) {
					const secondary = !!(row % 2);
					const increaseSize = row >= 2;
					let $options = win.$(`.chooser > *`);
					if ($options.length === 0) {
						$options = win.$("<dummy>");
					}
					for (let o=0; o<$options.length; o++) {
						$options[o].click();
						if (increaseSize && (o === 0 || toolName==="Brush" || toolName==="Line")) {
							for (let i = 0; i < 5; i++) {
								win.$('body').trigger(new win.$.Event("keydown", {key: "NumpadPlus", keyCode: 107, which: 107}));
							}
						}
						const start = {x: 0.05 + o*0.05, y: 0.1 + 0.1*row};
						const end = {x: start.x + 0.04, y: start.y + 0.04};
						await simulateGesture(win, {shift: false, secondary: !!secondary, start, end});
					}
				}
			});
			cy.get(".main-canvas").matchImageSnapshot(toolName.match(/Rounded Rectangle|Ellipse/) ? roundedToolsCompareOptions : undefined);
		});
	});
});
```

- Visual test

```javascript
/// <reference types="Cypress" />

context('visual tests', () => {

	const withTextCompareOptions = {
		failureThreshold: 0.05,
		failureThresholdType: 'percent' // not actually percent - fraction
	};
	const withMuchTextCompareOptions = {
		failureThreshold: 0.08,
		failureThresholdType: 'percent' // not actually percent - fraction
	};
	const toolboxCompareOptions = {
		failureThreshold: 40,
		failureThresholdType: 'pixel'
	};

	const selectTheme = (themeName) => {
		cy.contains(".menu-button", "Extras").click();
		cy.contains(".menu-item", "Theme").click();
		cy.contains(".menu-item", themeName).click();
		cy.get(".status-text").click(); // close menu (@TODO: menus should probably always be closed when you select a menu item)
		cy.wait(1000); // give a bit of time for theme to load
	};
```

  - Selection

```javascript

	it('main screenshot', () => {
		cy.visit('/');
		cy.setResolution([760, 490]);
		cy.window().should('have.property', 'get_tool_by_name'); // wait for app to be loaded
		cy.matchImageSnapshot(withTextCompareOptions);
	});

	it('brush selected', () => {
		cy.get('.tool[title="Brush"]').click();
		cy.get('.Tools-component').matchImageSnapshot(toolboxCompareOptions);
	});
	it('select selected', () => {
		cy.get('.tool[title="Select"]').click();
		cy.get('.Tools-component').matchImageSnapshot(toolboxCompareOptions);
	});
	it('magnifier selected', () => {
		cy.get('.tool[title="Magnifier"]').click();
		cy.get('.Tools-component').matchImageSnapshot(toolboxCompareOptions);
	});
	it('airbrush selected', () => {
		cy.get('.tool[title="Airbrush"]').click();
		cy.get('.Tools-component').matchImageSnapshot(toolboxCompareOptions);
	});
	it('eraser selected', () => {
		cy.get('.tool[title="Eraser/Color Eraser"]').click();
		cy.get('.Tools-component').matchImageSnapshot(toolboxCompareOptions);
	});
	it('line selected', () => {
		cy.get('.tool[title="Line"]').click();
		cy.get('.Tools-component').matchImageSnapshot(toolboxCompareOptions);
	});
	it('rectangle selected', () => {
		cy.get('.tool[title="Rectangle"]').click();
		cy.get('.Tools-component').matchImageSnapshot(toolboxCompareOptions);
	});
```

  - Window
  
  
```javascript
beforeEach(()=> {
		if (Cypress.$('.window:visible')[0]) {
			cy.get('.window:visible .window-close-button').click();
			cy.get('.window').should('not.be.visible');
		}
	});

	it('image attributes window', () => {
		cy.get('body').type('{ctrl}e');
		cy.get('.window:visible').matchImageSnapshot(withMuchTextCompareOptions);
	});

	it('flip and rotate window', () => {
		// @TODO: make menus more testable, with IDs
		cy.get('.menus > .menu-container:nth-child(4) > .menu-button > .menu-hotkey').click();
		cy.get('.menus > .menu-container:nth-child(4) > .menu-popup > table > tr:nth-child(1)').click();
		cy.get('.window:visible').matchImageSnapshot(withMuchTextCompareOptions);
	});

	it('stretch and skew window', () => {
		// @TODO: make menus more testable, with IDs
		cy.get('.menus > .menu-container:nth-child(4) > .menu-button > .menu-hotkey').click();
		cy.get('.menus > .menu-container:nth-child(4) > .menu-popup > table > tr:nth-child(2)').click();
		// @TODO: wait for images to load and include images?
		cy.get('.window:visible').matchImageSnapshot(Object.assign({}, withTextCompareOptions, { blackout: ["img"] }));
	});

	it('help window', () => {
		// @TODO: make menus more testable, with IDs
		cy.get('.menus > .menu-container:nth-child(6) > .menu-button > .menu-hotkey').click();
		cy.get('.menus > .menu-container:nth-child(6) > .menu-popup > table > tr:nth-child(1)').click();
		cy.get('.window:visible .folder', {timeout: 10000}); // wait for sidebar contents to load
		// @TODO: wait for iframe to load
		cy.get('.window:visible').matchImageSnapshot(Object.assign({}, withTextCompareOptions, { blackout: ["iframe"] }));
	});

	it('about window', () => {
		// @TODO: make menus more testable, with IDs
		cy.get('.menus > .menu-container:nth-child(6) > .menu-button > .menu-hotkey').click();
		cy.get('.menus > .menu-container:nth-child(6) > .menu-popup > table > tr:nth-child(3)').click();
		cy.get('.window:visible').matchImageSnapshot(Object.assign({}, withMuchTextCompareOptions, { blackout: ["img", "#maybe-outdated-line"] }));
	});
```

  - Mode and Theme
  
```javascript
it('eye gaze mode', () => {
		cy.get('.tool[title="Select"]').click();
		cy.contains(".menu-button", "Extras").click();
		cy.contains(".menu-item", "Eye Gaze Mode").click();
		cy.wait(100);
		// cy.contains(".menu-button", "View").click();
		// cy.get("body").trigger("pointermove", { clientX: 200, clientY: 150 });
		cy.get(".status-text").click();
		cy.wait(100);
		cy.matchImageSnapshot(withTextCompareOptions);
	});

	it('modern theme eye gaze mode', () => {
		selectTheme("Modern");
		// cy.contains(".menu-button", "View").click();
		// cy.get("body").trigger("pointermove", { clientX: 200, clientY: 150 });
		cy.wait(100);
		cy.matchImageSnapshot(withTextCompareOptions);
	});

	it('modern theme', () => {
		cy.contains(".menu-button", "Extras").click();
		cy.contains(".menu-item", "Eye Gaze Mode").click();
		cy.wait(100);
		// cy.contains(".menu-button", "View").click();
		// cy.get("body").trigger("pointermove", { clientX: 200, clientY: 150 });
		cy.get(".status-text").click();
		cy.wait(100);
		cy.matchImageSnapshot(withTextCompareOptions);
	});

	const test_edit_colors_dialog = (expand=true) => {
		cy.contains(".menu-button", "Colors").click();
		cy.contains(".menu-item", "Edit Colors").click();
		cy.wait(100);
		if (expand) {
			cy.contains("button", "Define Custom Colors >>").click();
		}
		cy.get('.window:visible').matchImageSnapshot(Object.assign({}, withTextCompareOptions));
	};
	it('modern theme edit colors dialog (expanded)', test_edit_colors_dialog);

	it('winter theme', () => {
		selectTheme("Winter");
		// cy.contains(".menu-button", "View").click();
		// cy.get("body").trigger("pointermove", { clientX: 200, clientY: 150 });
		cy.wait(100);
		cy.matchImageSnapshot(withTextCompareOptions);
	});

	it('winter theme edit colors dialog (expanded)', test_edit_colors_dialog);

	it('winter theme vertical color box', () => {
		cy.wait(500);
		cy.contains(".menu-button", "Extras").click();
		cy.contains(".menu-item", "Vertical Color Box").click();
		cy.wait(500);
		cy.get(".status-text").click();
		cy.wait(100);
		cy.matchImageSnapshot(withTextCompareOptions);
	});

	it('classic theme vertical color box', () => {
		selectTheme("Classic");
		cy.matchImageSnapshot(withTextCompareOptions);
	});

	it('classic theme edit colors dialog', ()=> {
		test_edit_colors_dialog(false);
	});

	it('modern theme vertical color box', () => {
		selectTheme("Modern");
		cy.matchImageSnapshot(withTextCompareOptions);
	});

});

```
  
  
### Despliegue Automatico:
  

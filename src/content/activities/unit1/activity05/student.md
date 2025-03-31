# 🎨 Generador de Patrones en p5.js

## Codigo 1 Base

Este código en **p5.js** genera un patrón de líneas diagonales en una cuadrícula. La dirección de las líneas es aleatoria y el grosor de cada una depende de la posición del mouse.

### 🖥 Descripción del Código

1. **Genera una cuadrícula de 20x20 celdas.**
2. **Cada celda contiene una línea diagonal**, con dirección aleatoria.
3. **El grosor de las líneas varía** según la posición del mouse (`mouseX` y `mouseY`).
4. **Con un clic cambia la disposición de las líneas.**
5. **Con las teclas `1`, `2`, y `3` cambia la terminación de las líneas** (`ROUND`, `SQUARE`, `PROJECT`).
6. **Con `S` se guarda la imagen del patrón generado.**

---

### ⚙ Parámetros y Variables

| **Variable**      | **Función** |
|------------------|------------|
| `tileCount`      | Número de divisiones en la cuadrícula (20x20 por defecto). |
| `actRandomSeed`  | Controla la aleatoriedad del patrón. |
| `actStrokeCap`   | Define la terminación de las líneas (`ROUND`, `SQUARE`, `PROJECT`). |
| `mouseX`, `mouseY` | Controlan el grosor de las líneas. |

---

### 🖥 Código Original en p5.js

```javascript
'use strict';

var tileCount = 20;
var actRandomSeed = 0;
var actStrokeCap;

function setup() {
  createCanvas(600, 600);
  actStrokeCap = ROUND;
}

function draw() {
  clear();
  strokeCap(actStrokeCap);
  randomSeed(actRandomSeed);

  for (var gridY = 0; gridY < tileCount; gridY++) {
    for (var gridX = 0; gridX < tileCount; gridX++) {

      var posX = width / tileCount * gridX;
      var posY = height / tileCount * gridY;

      var toggle = int(random(0, 2));

      if (toggle == 0) {
        strokeWeight(mouseX / 20);
        line(posX, posY, posX + width / tileCount, posY + height / tileCount);
      }
      if (toggle == 1) {
        strokeWeight(mouseY / 20);
        line(posX, posY + width / tileCount, posX + height / tileCount, posY);
      }
    }
  }
}

function mousePressed() {
  actRandomSeed = random(100000);
}

function keyReleased() {
  if (key == 's' || key == 'S') saveCanvas('pattern', 'png');

  if (key == '1') actStrokeCap = ROUND;
  if (key == '2') actStrokeCap = SQUARE;
  if (key == '3') actStrokeCap = PROJECT;
}
```
## Codigo 1 Mejorado

Este código en **p5.js** genera un patrón dinámico de líneas diagonales en una cuadrícula, con características mejoradas:

1. **Colores dinámicos** que cambian suavemente con el tiempo.
2. **Grosor animado** para crear un efecto de pulsación.
3. **Control de densidad de la cuadrícula** con las teclas `+` y `-`.
4. **Interacción con el mouse**: al hacer clic, se genera una nueva disposición de líneas.
5. **Cambio del estilo de línea** con las teclas `1`, `2`, y `3` (`ROUND`, `SQUARE`, `PROJECT`).
6. **Opción para guardar la imagen del patrón** presionando `S`.

---

### ⚙ Parámetros y Variables

| **Variable**      | **Función** |
|------------------|------------|
| `tileCount`      | Número de divisiones en la cuadrícula (20x20 por defecto, ajustable con `+` y `-`). |
| `actRandomSeed`  | Controla la aleatoriedad del patrón. |
| `actStrokeCap`   | Define la terminación de las líneas (`ROUND`, `SQUARE`, `PROJECT`). |
| `mouseX`, `mouseY` | Afectan la interacción, generando nuevos patrones con el clic. |
| `frameCount`     | Se usa para animar el color y el grosor de las líneas. |

---

### 🖥 Código Mejorado en p5.js

```javascript
'use strict';

var tileCount = 20;
var actRandomSeed = 0;
var actStrokeCap;

function setup() {
  createCanvas(600, 600);
  actStrokeCap = ROUND;
}

function draw() {
  clear();
  strokeCap(actStrokeCap);
  randomSeed(actRandomSeed);

  for (var gridY = 0; gridY < tileCount; gridY++) {
    for (var gridX = 0; gridX < tileCount; gridX++) {

      var posX = width / tileCount * gridX;
      var posY = height / tileCount * gridY;

      var toggle = int(random(0, 2));

      // 🎨 Color animado con seno y coseno
      let r = sin(frameCount * 0.01) * 127 + 128;
      let g = cos(frameCount * 0.02) * 127 + 128;
      let b = sin(frameCount * 0.015) * 127 + 128;
      stroke(r, g, b);

      // 🔄 Grosor animado
      let weight = map(sin(frameCount * 0.05), -1, 1, 1, 10);
      strokeWeight(weight);

      if (toggle == 0) {
        line(posX, posY, posX + width / tileCount, posY + height / tileCount);
      }
      if (toggle == 1) {
        line(posX, posY + width / tileCount, posX + height / tileCount, posY);
      }
    }
  }
}

// 🎲 Cambia el patrón con un clic
function mousePressed() {
  actRandomSeed = random(100000);
}

// ⌨ Controles del teclado
function keyReleased() {
  if (key == 's' || key == 'S') saveCanvas('pattern', 'png');

  if (key == '1') actStrokeCap = ROUND;
  if (key == '2') actStrokeCap = SQUARE;
  if (key == '3') actStrokeCap = PROJECT;

  if (key == '+') tileCount = min(tileCount + 2, 50);
  if (key == '-') tileCount = max(tileCount - 2, 5);
}
```

# 🎨 Generador de Formas Circulares en p5.js
## Codigo 2 Base

Este código en **p5.js** permite dibujar polígonos que cambian según la posición del mouse. Al hacer clic izquierdo, se genera una figura con lados dinámicos y un radio que depende del mouse.

### 🖥 Descripción del Código

1. **Dibuja polígonos centrados en la pantalla** con un número de lados variable.
2. **El número de lados depende de la posición vertical del mouse (`mouseY`).**
3. **El tamaño del polígono depende de la posición horizontal del mouse (`mouseX`).**
4. **Permite cambiar el color del trazo** con las teclas `1`, `2`, y `3`.
5. **Presionar `DELETE` o `BACKSPACE` limpia la pantalla.**
6. **Presionar `S` guarda una imagen del patrón generado.**

---

### ⚙ Parámetros y Variables

| **Variable**      | **Función** |
|------------------|------------|
| `strokeColor`    | Define el color del trazo. |
| `circleResolution` | Controla el número de lados del polígono. |
| `radius`         | Define el tamaño del polígono según `mouseX`. |
| `angle`          | Calcula la separación angular entre vértices. |

---

### 🖥 Código Original en p5.js

```javascript
'use strict';

var strokeColor;

function setup() {
  createCanvas(720, 720);
  colorMode(HSB, 360, 100, 100, 100);
  noFill();
  strokeWeight(2);
  strokeColor = color(0, 10);
}

function draw() {
  if (mouseIsPressed && mouseButton == LEFT) {
    push();
    translate(width / 2, height / 2);

    var circleResolution = int(map(mouseY + 100, 0, height, 2, 10));
    var radius = mouseX - width / 2;
    var angle = TAU / circleResolution;

    stroke(strokeColor);

    beginShape();
    for (var i = 0; i <= circleResolution; i++) {
      var x = cos(angle * i) * radius;
      var y = sin(angle * i) * radius;
      vertex(x, y);
    }
    endShape();

    pop();
  }
}

function keyReleased() {
  if (keyCode == DELETE || keyCode == BACKSPACE) background(0, 0, 100);
  if (key == 's' || key == 'S') saveCanvas('pattern', 'png');

  if (key == '1') strokeColor = color(0, 10);
  if (key == '2') strokeColor = color(192, 100, 64, 10);
  if (key == '3') strokeColor = color(52, 100, 71, 10);
}
```
# 🎨 Generador de Formas Circulares Animadas en p5.js
## Codigo 2 Mejorado

Este código en **p5.js** crea polígonos dinámicos centrados en la pantalla con efectos de animación. Al presionar el clic izquierdo del mouse, se dibujan figuras con características interactivas.

### 🖥 Mejoras en el Código

✅ **Colores dinámicos**: El trazo cambia de color con el tiempo usando `sin()` y `cos()`.  
✅ **Grosor del trazo variable**: Se anima suavemente para generar un efecto de pulsación.  
✅ **Rotación automática**: Las figuras giran a medida que se dibujan.  
✅ **Mayor resolución de polígonos**: Se permiten hasta 20 lados para formas más suaves.  
✅ **Cambio de fondo**: Se alterna entre blanco y negro con la tecla `B`.  
✅ **Opción de guardar el patrón**: Se puede capturar una imagen con `S`.  

---

### ⚙ Parámetros y Variables

| **Variable**       | **Función** |
|-------------------|------------|
| `strokeColor`     | Color de las líneas, animado en tiempo real. |
| `backgroundColor` | Color de fondo, alterna entre negro y blanco. |
| `circleResolution` | Define la cantidad de lados del polígono. |
| `radius`          | Controla el tamaño del polígono según `mouseX`. |
| `angle`           | Determina la separación angular entre los vértices. |

---

### 🖥 Código Mejorado en p5.js

```javascript
'use strict';

var strokeColor;
var backgroundColor = 100; // Fondo dinámico

function setup() {
  createCanvas(720, 720);
  colorMode(HSB, 360, 100, 100, 100);
  noFill();
  strokeWeight(2);
  strokeColor = color(0, 10);
}

function draw() {
  background(backgroundColor, 10);
  
  if (mouseIsPressed && mouseButton == LEFT) {
    push();
    translate(width / 2, height / 2);
    rotate(frameCount * 0.02); // 🔄 Rotación animada

    var circleResolution = int(map(mouseY + 100, 0, height, 3, 20)); // Mayor resolución
    var radius = mouseX - width / 2;
    var angle = TAU / circleResolution;

    // 🎨 Color animado
    let r = sin(frameCount * 0.01) * 127 + 128;
    let g = cos(frameCount * 0.02) * 127 + 128;
    let b = sin(frameCount * 0.015) * 127 + 128;
    stroke(r, g, b, 50);

    // 🔄 Grosor animado
    let weight = map(sin(frameCount * 0.05), -1, 1, 1, 5);
    strokeWeight(weight);

    beginShape();
    for (var i = 0; i <= circleResolution; i++) {
      var x = cos(angle * i) * radius;
      var y = sin(angle * i) * radius;
      vertex(x, y);
    }
    endShape();

    pop();
  }
}

function keyReleased() {
  if (keyCode == DELETE || keyCode == BACKSPACE) background(backgroundColor);
  if (key == 's' || key == 'S') saveCanvas('pattern', 'png');
  if (key == 'b' || key == 'B') backgroundColor = backgroundColor === 100 ? 0 : 100; // 🔄 Cambia fondo

  if (key == '1') strokeColor = color(0, 10);
  if (key == '2') strokeColor = color(192, 100, 64, 50);
  if (key == '3') strokeColor = color(52, 100, 71, 50);
}
```
# 🟩 Generador de Cuadrículas Interactivas en p5.js

## Codigo 3 Base

Este código en **p5.js** genera una cuadrícula de rectángulos que cambian de tamaño dependiendo de la distancia del mouse. El efecto visual crea una sensación de profundidad y movimiento.

## 🖥 Descripción del Código

1. **Dibuja una cuadrícula de rectángulos en la pantalla.**  
2. **El tamaño de cada rectángulo depende de la distancia con el mouse.**  
3. **Si el mouse se acerca, los rectángulos crecen; si se aleja, disminuyen.**  
4. **Permite guardar la imagen con la tecla `S`.**  

---

## ⚙ Parámetros y Variables

| **Variable**       | **Función** |
|-------------------|------------|
| `tileCount`       | Define la cantidad de filas y columnas en la cuadrícula. |
| `moduleColor`     | Color de los rectángulos. |
| `moduleAlpha`     | Opacidad de los rectángulos. |
| `maxDistance`     | Define el rango máximo de influencia del mouse. |

---

## 🖥 Código Original en p5.js

```javascript
'use strict';

var tileCount = 20;

var moduleColor;
var moduleAlpha = 180;
var maxDistance = 500;

function setup() {
  createCanvas(600, 600);
  noFill();
  strokeWeight(3);
  moduleColor = color(0, 0, 0, moduleAlpha);
}

function draw() {
  clear();

  stroke(moduleColor);

  for (var gridY = 0; gridY < width; gridY += 25) {
    for (var gridX = 0; gridX < height; gridX += 25) {
      var diameter = dist(mouseX, mouseY, gridX, gridY);
      diameter = diameter / maxDistance * 40;
      push();
      translate(gridX, gridY, diameter * 5);
      rect(0, 0, diameter, diameter); // also nice: ellipse(...)
      pop();
    }
  }
}

function keyReleased() {
  if (key == 's' || key == 'S') saveCanvas('pattern', 'png');
}
```
## Codigo 3 Mejorado

Este código en **p5.js** genera una cuadrícula de rectángulos que cambian de tamaño y rotación dependiendo de la distancia del mouse. Se han agregado mejoras para hacer la animación más fluida y visualmente atractiva.

### 🚀 Mejoras Implementadas

1. **Colores dinámicos**: Los rectángulos cambian de color según su tamaño.  
2. **Rotación interactiva**: Cada rectángulo gira basado en la posición del mouse.  
3. **Mayor fluidez**: Se usa `lerp()` para transiciones más suaves en el tamaño.  
4. **Mejor escala de tamaño**: Se normaliza con `map()` y `constrain()` para evitar valores extremos.  
5. **Fondo intercambiable**: Con la tecla `B`, se puede cambiar entre blanco y negro.  
6. **Mayor control sobre el grosor**: Se ajusta el `strokeWeight()` según la proximidad del mouse.  
7. **Efecto de profundidad**: Se usa `push()` y `pop()` para transformar cada rectángulo sin afectar el resto de la cuadrícula.  

---

### ⚙ Parámetros y Variables

| **Variable**       | **Función** |
|-------------------|------------|
| `tileCount`       | Define la cantidad de filas y columnas en la cuadrícula. |
| `maxDistance`     | Define el rango máximo de influencia del mouse. |
| `backgroundColor` | Permite alternar entre fondo blanco y negro. |

---

### 🖥 Código Mejorado en p5.js

```javascript
'use strict';

var tileCount = 20;
var moduleAlpha = 180;
var maxDistance = 500;
var backgroundColor = 255;

function setup() {
  createCanvas(600, 600);
  noFill();
  strokeWeight(2);
}

function draw() {
  background(backgroundColor);

  for (var gridY = 0; gridY < width; gridY += 25) {
    for (var gridX = 0; gridX < height; gridX += 25) {
      
      // Calcular distancia al mouse y normalizar tamaño
      var distance = dist(mouseX, mouseY, gridX, gridY);
      var size = map(distance, 0, maxDistance, 40, 5);
      size = constrain(size, 5, 40); // Evita valores fuera de rango
      
      // Color dinámico basado en el tamaño
      var dynamicColor = color(map(size, 5, 40, 0, 255), 100, 255, moduleAlpha);
      stroke(dynamicColor);
      
      push();
      translate(gridX, gridY);
      
      // Rotación basada en la posición del mouse
      var angle = map(mouseX, 0, width, -PI / 4, PI / 4);
      rotate(angle);

      rectMode(CENTER);
      rect(0, 0, size, size);
      pop();
    }
  }
}

function keyReleased() {
  if (key == 's' || key == 'S') saveCanvas('pattern', 'png');
  if (key == 'b' || key == 'B') backgroundColor = backgroundColor === 255 ? 0 : 255; // Cambia fondo
}
```

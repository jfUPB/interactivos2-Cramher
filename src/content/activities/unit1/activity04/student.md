# Codigo 1
## Descripcion 

El codigo genera una esprial que va cambiando de color de forma suave y se va moviendo desde el "centro" del canvas generado.
``` js
let angle = 0;

function setup() {
  createCanvas(400, 400);
  noFill();
  strokeWeight(2);
}

function draw() {
  background(255);

  // Generar espiral
  translate(width / 2, height / 2);  // Centra el origen
  let r = map(sin(angle), -1, 1, 0, 255);
  let g = map(cos(angle), -1, 1, 0, 255);
  stroke(r, g, 255); // Gradiente basado en el ángulo
  // Color aleatorio
  beginShape();
  for (let i = 0; i < 500; i++) {
    let x = cos(angle) * i;
    let y = sin(angle) * i;
    vertex(x, y);
    angle += 0.2;  // Modifica la velocidad de rotación
  }
  endShape();
}
```
## Modificaciones

Ahora el codigo genera una segunda espiral y las espirales salen desde las esquinas inferiores del canvas generado, la linea es mas gruesa y hay mas espacio entre vuelta.
``` js
let angle1 = 0;
let angle2 = 0;

function setup() {
  createCanvas(400, 400);
  noFill();
  strokeWeight(4);
}

function draw() {
  background(255);

  // Espiral 1 (primera espiral)
  translate(width / width, height / 1);  // Desplaza la espiral 1 a la izquierda
  let r = map(sin(angle1), -1, 1, 0, 255);
  let g = map(cos(angle1), -1, 1, 0, 255);
stroke(r, g, 255); // Gradiente basado en el ángulo  // Color aleatorio
  beginShape();
  for (let i = 0; i < 500; i++) {
    let x = cos(angle1) * i;
    let y = sin(angle1) * i;
    vertex(x, y);
    angle1 += 0.05;  // Modifica la velocidad de rotación
  }
  endShape();

  // Espiral 2 (segunda espiral)
  translate(width / 1, height / height);  // Mueve el origen para la segunda espiral
  let r1 = map(sin(angle2), -1, 1, 0, 255);
  let g1 = map(cos(angle2), -1, 1, 0, 255);
stroke(r1, g1, 255); // Gradiente basado en el ángulo  // Color aleatorio
  beginShape();
  for (let i = 0; i < 500; i++) {
    let x = cos(angle2) * i;
    let y = sin(angle2) * i;
    vertex(x, y);
    angle2 += 0.05;  // Modifica la velocidad de rotación para la segunda espiral
  }
  endShape();
}
```
# Codigo 2
## Descripcion
El codigo genera circulos que tienen su propio movimiento y son de diferentes tamaños.
``` js
let circles = [];

function setup() {
  createCanvas(400, 400);
  for (let i = 0; i < 10; i++) {
    circles.push(new Circle(random(width), random(height)));
  }
}

function draw() {
  background(255);
  for (let c of circles) {
    c.update();
    c.display();
  }
}

class Circle {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.radius = random(10, 30);
    this.speedX = random(-2, 2);
    this.speedY = random(-2, 2);
    this.color = color(random(255), random(255), random(255));
  }

  update() {
    this.x += this.speedX;
    this.y += this.speedY;
    if (this.x > width || this.x < 0) this.speedX *= -1;
    if (this.y > height || this.y < 0) this.speedY *= -1;
  }

  display() {
    fill(this.color);
    noStroke();
    ellipse(this.x, this.y, this.radius * 2);
  }
}

```
## Modificaciones
Ahora el codigo genera mas circulos y de mayor tamaño ya que se cambio el limite del radio
``` js
let circles = [];

function setup() {
  createCanvas(400, 400);
  for (let i = 0; i < 50; i++) {
    circles.push(new Circle(random(width), random(height)));
  }
}

function draw() {
  background(255);
  for (let c of circles) {
    c.update();
    c.display();
  }
}

class Circle {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.radius = random(20, 60);
    this.speedX = random(-2, 2);
    this.speedY = random(-2, 2);
    this.color = color(random(255), random(255), random(255));
  }

  update() {
    this.x += this.speedX;
    this.y += this.speedY;
    if (this.x > width || this.x < 0) this.speedX *= -1;
    if (this.y > height || this.y < 0) this.speedY *= -1;
  }

  display() {
    fill(this.color);
    noStroke();
    ellipse(this.x, this.y, this.radius * 2);
  }
}
```

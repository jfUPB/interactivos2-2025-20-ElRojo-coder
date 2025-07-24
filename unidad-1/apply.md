# Unidad 1

## 🛠 Fase: Apply


### fase 1 seleccionar 

#### código y enlace originales 
``` js

'use strict';

function setup() {
  createCanvas(550, 550);
  strokeCap(SQUARE);
}

function draw() {
  background(255);
  translate(width / 2, height / 2);

  var circleResolution = int(map(mouseY, 0, height, 2, 80));
  var radius = mouseX - width / 2;
  var angle = TAU / circleResolution;

  strokeWeight(mouseY / 20);

  for (var i = 0; i <= circleResolution; i++) {
    var x = cos(angle * i) * radius;
    var y = sin(angle * i) * radius;
    line(0, 0, x, y);
  }
}

function keyPressed() {
  if (key == 's' || key == 'S') saveCanvas(gd.timestamp(), 'png');
}

```
http://www.generative-gestaltung.de/2/sketches/?01_P/P_2_0_01

#### fase 2 describir
Este codigo crea un canva que dependiendo de la posición del mouse crea una linea o un punto, hacia las esquinas inferiores el punto se hace mas grande haciendo un ciruclo y hacia las esquinas superiores crea lineas, en los intermedios se crea un circulo con lineas que salen del mismo, una vez mas según la posición del mouse 


#### fase 3 describir 

el código en si es sencillo, crea el espacio de trabajo, pone un circulo centrado, segun la posición en Y del mouse crea lineas (arriba pocas y abajo muchas) y el grosor de las lineas (entre mas abajo mas gruesas)y el radio del mouse depende de el movimiento del mouse en X 

```
for (var i = 0; i <= circleResolution; i++) {
  var x = cos(angle * i) * radius;
  var y = sin(angle * i) * radius;
  line(0, 0, x, y);
}

```
Este bucle dibuja líneas radiales desde el centro hacia un punto en el borde del círculo.

Usa funciones trigonométricas cos() y sin() para calcular las posiciones X e Y alrededor del círculo.


#### fase 4 convertir 
#### código nuevo 
``` js
function setup() {
  createCanvas(550, 550);
  angleMode(RADIANS); // Asegura que use radianes
  strokeCap(SQUARE);
}

function draw() {
  background(255);
  translate(width / 2, height / 2);

  let cantidadLineas = int(map(mouseY, 0, height, 2, 80));
  let radio = constrain(mouseX - width / 2, 0, width / 2);
  let angulo = TWO_PI / cantidadLineas;
  let grosor = map(mouseY, 0, height, 1, 20);

  strokeWeight(grosor);
  stroke(0); // negro

  for (let i = 0; i < cantidadLineas; i++) {
    let x = cos(angulo * i) * radio;
    let y = sin(angulo * i) * radio;
    line(0, 0, x, y);
  }
}

```


#### fase 5 explorar 
#### versión 1.0

``` js
let anguloRotacion = 0;

function setup() {
  createCanvas(550, 550);
  angleMode(RADIANS);
  strokeCap(SQUARE);
}

function draw() {
  background(255);
  translate(width / 2, height / 2);

  // Cantidad de líneas (más si el mouse está abajo)
  let cantidadLineas = int(map(mouseY, 0, height, 2, 80));

  // Radio desde el centro
  let radio = constrain(mouseX - width / 2, 0, width / 2);

  // Grosor dinámico según altura del mouse
  let grosor = map(mouseY, 0, height, 1, 20);
  strokeWeight(grosor);
  stroke(0);

  // Velocidad de rotación según mouseY
  // Más arriba => más rápido (positivo), más abajo => más lento (negativo)
  let velocidad = map(mouseY, 0, height, 0.1, -0.1);
  anguloRotacion += velocidad;

  // Ángulo entre líneas
  let angulo = TWO_PI / cantidadLineas;

  for (let i = 0; i < cantidadLineas; i++) {
    let x = cos(angulo * i + anguloRotacion) * radio;
    let y = sin(angulo * i + anguloRotacion) * radio;
    line(0, 0, x, y);
  }
}

```
#### cambio
cada que el mouse cambie su valor en Y+ va a girar mas rapido, cada que el mouse cambie su posición en Y- este va a girar mas lento y en sentido opuesto 

#### fase 6 explotar 

#### código 2.0 
``` js
let circulos = []; // Lista de círculos

function setup() {
  createCanvas(550, 550);
  angleMode(RADIANS);
  strokeCap(SQUARE);
  // Crear un primer círculo en el centro
  circulos.push(new Circulo(width / 2, height / 2, 1));
}

function draw() {
  background(255);
  for (let c of circulos) {
    c.actualizar();
    c.dibujar();
  }
}

function keyPressed() {
  if (key === 's' || key === 'S') {
    let x = random(width);
    let y = random(height);
    let direccion = random([1, -1]); // 50% probabilidad de invertir rotación
    circulos.push(new Circulo(x, y, direccion));
  }

  if (key === 'd' || key === 'D') {
    for (let c of circulos) {
      c.cambiarColor();
    }
  }
}

class Circulo {
  constructor(x, y, direccion) {
    this.x = x;
    this.y = y;
    this.direccion = direccion;
    this.anguloRotacion = 0;
    this.color = color(random(255), random(255), random(255));
  }

  actualizar() {
    // Velocidad basada en mouseY
    let velocidad = map(mouseY, 0, height, 0.1, -0.1);
    this.anguloRotacion += velocidad * this.direccion;
  }

  dibujar() {
    push();
    translate(this.x, this.y);

    let cantidadLineas = int(map(mouseY, 0, height, 2, 80));
    let radio = constrain(mouseX - width / 2, 0, width / 2);
    let grosor = map(mouseY, 0, height, 1, 20);

    strokeWeight(grosor);
    stroke(this.color);

    let angulo = TWO_PI / cantidadLineas;
    for (let i = 0; i < cantidadLineas; i++) {
      let x = cos(angulo * i + this.anguloRotacion) * radio;
      let y = sin(angulo * i + this.anguloRotacion) * radio;
      line(0, 0, x, y);
    }

    pop();
  }

  cambiarColor() {
    this.color = color(random(255), random(255), random(255));
  }
}
```

<img width="404" height="370" alt="image" src="https://github.com/user-attachments/assets/5352eb4d-a583-4b4c-a60a-b5d5d7f179c9" />

<img width="452" height="428" alt="image" src="https://github.com/user-attachments/assets/6c3f80e7-eaf2-42ce-abee-524726ad6d3e" />

<img width="460" height="426" alt="image" src="https://github.com/user-attachments/assets/7baa0192-5711-442a-8219-28e07b025357" />

<img width="459" height="476" alt="image" src="https://github.com/user-attachments/assets/ab9b8f27-d1aa-4d21-8c67-4e7b882d5089" />

#### cambios 

ahora cada que se presiona la letra S se genera un nuevo punto de creación, cada uno cuando se genera tiene el 50% de probabilidad de conservar la rotación del original o de invertirse, adicionalmente cadaa que se presiona la letra D los circulos creados cambian a otro color aleatorio 

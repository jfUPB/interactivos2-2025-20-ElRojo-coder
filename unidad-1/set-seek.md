# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 1 

#### pregunta 1 ¿qué es el diseño generactivo?
 
R/: es un tipo de arte o diseño generado con unos parametros estrictos dados desde antes. mientras que un diseñador tiene "libre albedrio" sobre el que crear, el arte generativo no tiene esa livertad, aparte de dar la posibilidad de que un pequeño cambio haga todo el cambio de diseño con solo mover un parametro 

#### pregunta 2 que es el arte generativo 
R/: el arte generativo es aquel que es interferido por un sistema o computadora el cual se encarga de generarlo de forma parcial o general donde la aleatoridad prima y da cavida a diseños unicos e irrepetibles 

#### Pregunta 3 arte generativo vs arte tradicional 

R/: la primera gran diferencia es claramente el hecho de que uno se vea mas intervenido por maquinas y el otro se suele centrar mas en la creación unica del ser humano donde prima su diseño y creatividad unica, donde claramente influyen los pensamientos de él y gustos. Si es realizado de forma generativa con una maquina esta aunque pueda tener componentes aleatorios de cierta forma sigue patrones y se pueden identificar y modificar a gusto. El arte tradicional usa cosas tangibles como pinceles, lapices y demás. Mientras que el arte generativo tiene cosas como inteligencia artificial y otros y el arte generativo puede hacer grandes cambios en cuestión de segundos, sacrificando por ahora calidad o detalles que si se pueden tener en el arte tradicional pero sacricando el tiempo 

#### pregunta 4 ¿Qué posibilidades crees que esto puede ofrecer a tu perfil profesional?

R/: bastante, la verdad el hecho de arte generativo, generación de patrones, visuales y demás cosas relacionadas me llaman la atención a la hora de el ambito profesional y me puedo idiealizar a trabajar con este campo 

### Actividad 2 

#### pregunta 1 ¿Qué pensabas que hacía un Ingeniero en diseño de entretenimiento digital con énfasis en experiencias interactivas?

R/: pensaba que se centraba mas en la parte de creación de objetos fisicos para que usuarios interctuaran con este pero ahora me doy cuenta que puden ser muchas cosas que aparte de objetos fisicos, puede ser generación de particulas, arte generativo, modelados y demás 

#### pregunta 2 ¿Qué potencial le ves al diseño e implementación de experiencias inmersivas colectivas?
R/: demasiadas, viendo ejemplos como el propuesto por el profesor de alguien haciendo patrones y generando arte mientras un cantante hace su parte del show puede hacer cosas muy increibles, pues si ya los visuales son un campo muy utilizado en la industria de la musica y los conciertos el que pueda ser creado en tiempo real y de cierta forma mas intimo al artista brinda muchas posibilidades como para que el usuario lo viva de muchas formas, llegando al punto en que inlcuso los que van a disfrutar del concierto interfieran en los visuales del mismom 

#### pregunta 3 ¿Cómo te ves profesionalmente en este escenario?
R/: eso que menciono anteriormente me llama mucho la atención pues la musica es una parte muy importante en mi vida y me gustaria trabajar en ese ambito, siento que puede ser mas maleable, mientras un artista de musica suele casarse con un genero o tipo de musica, el arte generativo y el ser la imagen de la musica me da la posibilidad de conocer mucha musica y de que la misma musica la represente de diferentes formas según el artista me lo pida y lo podamos lograr 

### Actividad 3 

#### Código original 

``` Js
'use strict';

var shape;

var joints = 5;
var lineLength = 100;
var speedRelation = 2;
var center;
var pendulumPath;
var angle = 0;
var maxAngle = 360;
var speed;

var showPendulum = true;
var showPendulumPath = true;

function setup() {
  createCanvas(windowWidth, windowHeight);
  colorMode(HSB, 360, 100, 100, 100);
  noFill();
  strokeWeight(1);

  center = createVector(width / 2, height / 2);

  startDrawing();
}

function startDrawing() {
  pendulumPath = [];
  // new empty array for each joint
  for (var i = 0; i < joints; i++) {
    pendulumPath.push([]);
  }

  angle = 0;
  speed = (8 / pow(1.75, joints - 1) / pow(2, speedRelation - 1));
}

function draw() {
  background(0, 0, 100);

  angle += speed;

  // each frame, create new positions for each joint
  if (angle <= maxAngle + speed) {
    // start at the center position
    var pos = center.copy();

    for (var i = 0; i < joints; i++) {
      var a = angle * pow(speedRelation, i);
      if (i % 2 == 1) a = -a;
      var nextPos = p5.Vector.fromAngle(radians(a));
      nextPos.setMag((joints - i) / joints * lineLength);
      nextPos.add(pos);

      if (showPendulum) {
        noStroke();
        fill(0, 10);
        ellipse(pos.x, pos.y, 4, 4);
        noFill();
        stroke(0, 10);
        line(pos.x, pos.y, nextPos.x, nextPos.y);
      }

      pendulumPath[i].push(nextPos);
      pos = nextPos;
    }
  }

  // draw the path for each joint
  if (showPendulumPath) {
    strokeWeight(1.6);
    for (var i = 0; i < pendulumPath.length; i++) {
      var path = pendulumPath[i];

      beginShape();
      var hue = map(i, 0, joints, 120, 360);
      stroke(hue, 80, 60, 50);
      for (var j = 0; j < path.length; j++) {
        vertex(path[j].x, path[j].y);
      }
      endShape();
    }
  }
}

function keyPressed() {
  if (key == 's' || key == 'S') saveCanvas(gd.timestamp(), 'png');

  if (keyCode == DELETE || keyCode == BACKSPACE) startDrawing();

  if (keyCode == UP_ARROW) {
    lineLength += 2;
    startDrawing();
  }
  if (keyCode == DOWN_ARROW) {
    lineLength -= 2;
    startDrawing();
  }
  if (keyCode == LEFT_ARROW) {
    joints--;
    if (joints < 1) joints = 1;
    startDrawing();
  }
  if (keyCode == RIGHT_ARROW) {
    joints++;
    if (joints > 10) joints = 10;
    startDrawing();
  }

  if (key == '+') {
    speedRelation += 0.5;
    if (speedRelation > 5) speedRelation = 5;
    startDrawing();
  }
  if (key == '-') {
    speedRelation -= 0.5;
    if (speedRelation < 2) speedRelation = 2;
    startDrawing();
  }

  if (key == '1') showPendulum = !showPendulum;
  if (key == '2') showPendulumPath = !showPendulumPath;
}
```

#### código modificado 

``` Js
'use strict';

var joints = 5;
var lineLength = 100;
var speedRelation = 2;
var center;
var pendulumPath;
var angle = 0;
var maxAngle = 360;
var speed;

var showPendulum = true;
var showPendulumPath = true;

var isPaused = false; // NUEVO: estado de pausa

function setup() {
  createCanvas(windowWidth, windowHeight);
  colorMode(HSB, 360, 100, 100, 100);
  noFill();
  strokeWeight(1);

  center = createVector(width / 2, height / 2);
  startDrawing();
}

function startDrawing() {
  pendulumPath = [];
  for (var i = 0; i < joints; i++) {
    pendulumPath.push([]);
  }

  angle = 0;
  speed = (8 / pow(1.75, joints - 1) / pow(2, speedRelation - 1));
}

function draw() {
  background(mouseX, mouseX, 100);

  if (isPaused) return; // NUEVO: si está pausado, no dibuja nada

  angle += speed;

  if (angle <= maxAngle + speed) {
    var pos = center.copy();

    for (var i = 0; i < joints; i++) {
      var a = angle * pow(speedRelation, i);
      if (i % 2 == 1) a = -a;
      var nextPos = p5.Vector.fromAngle(radians(a));
      nextPos.setMag((joints - i) / joints * lineLength);
      nextPos.add(pos);

      if (showPendulum) {
        noStroke();
        fill(0, 10);
        ellipse(pos.x, pos.y, 4, 4);
        noFill();
        stroke(0, 10);
        line(pos.x, pos.y, nextPos.x, nextPos.y);
      }

      pendulumPath[i].push(nextPos);
      pos = nextPos;
    }
  }

  if (showPendulumPath) {
    strokeWeight(1.6);
    for (var i = 0; i < pendulumPath.length; i++) {
      var path = pendulumPath[i];

      beginShape();
      var hue = map(i, 0, joints, 120, 360);
      stroke(hue, 80, 60, 50);
      for (var j = 0; j < path.length; j++) {
        vertex(path[j].x, path[j].y);
      }
      endShape();
    }
  }
}

function mousePressed() {
  if (mouseButton === LEFT) {
    if (!isPaused) {
      isPaused = true;
    } else {
      center = createVector(mouseX, mouseY); // NUEVO: reinicia desde aquí
      isPaused = false;
      startDrawing();
    }
  }
}

function keyPressed() {
  if (key == 's' || key == 'S') saveCanvas(gd.timestamp(), 'png');

  if (keyCode == DELETE || keyCode == BACKSPACE) startDrawing();

  if (keyCode == UP_ARROW) {
    lineLength += 2;
    startDrawing();
  }
  if (keyCode == DOWN_ARROW) {
    lineLength -= 2;
    startDrawing();
  }
  if (keyCode == LEFT_ARROW) {
    joints--;
    if (joints < 1) joints = 1;
    startDrawing();
  }
  if (keyCode == RIGHT_ARROW) {
    joints++;
    if (joints > 10) joints = 10;
    startDrawing();
  }

  if (key == '+') {
    speedRelation += 0.5;
    if (speedRelation > 5) speedRelation = 5;
    startDrawing();
  }
  if (key == '-') {
    speedRelation -= 0.5;
    if (speedRelation < 2) speedRelation = 2;
    startDrawing();
  }

  if (key == '1') showPendulum = !showPendulum;
  if (key == '2') showPendulumPath = !showPendulumPath;
}

```
## Actividad 4
``` js
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Generador de Figuras Geométricas</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.5.0/p5.min.js"></script>
</head>
<body>
  <script>
    let shapes = [];
    const shapeTypes = ['cuadrado', 'circulo', 'triangulo', 'rombo'];
    const numShapes = 20;

    function setup() {
      createCanvas(800, 600);
      for (let i = 0; i < numShapes; i++) {
        shapes.push(new Shape());
      }
    }

    function draw() {
      background(240);
      for (let s of shapes) {
        s.update();
        s.display();
      }
    }

    function mouseClicked() {
      // Cambiar color y tamaño de todas las figuras
      for (let s of shapes) {
        // Nuevo color diferente
        let newColor;
        do {
          newColor = color(random(255), random(255), random(255));
        } while (newColor.toString() === s.col.toString());
        s.col = newColor;
        // Nuevo tamaño aleatorio
        s.size = random(20, 60);
      }
    }

    class Shape {
      constructor() {
        this.type = random(shapeTypes);
        this.size = random(20, 60);
        this.pos = createVector(random(width), random(height));
        this.vel = p5.Vector.random2D().mult(random(1, 3));
        this.col = color(random(255), random(255), random(255));
      }

      update() {
        this.pos.add(this.vel);

        let hitX = false;
        let hitY = false;
        if (this.pos.x <= this.size / 2 || this.pos.x >= width - this.size / 2) {
          this.vel.x *= -1;
          hitX = true;
        }
        if (this.pos.y <= this.size / 2 || this.pos.y >= height - this.size / 2) {
          this.vel.y *= -1;
          hitY = true;
        }

        if (hitX && hitY) {
          this.vel = p5.Vector.random2D().mult(random(1, 3));
        }

        this.pos.x = constrain(this.pos.x, this.size / 2, width - this.size / 2);
        this.pos.y = constrain(this.pos.y, this.size / 2, height - this.size / 2);
      }

      display() {
        fill(this.col);
        noStroke();
        const s = this.size;
        const x = this.pos.x;
        const y = this.pos.y;

        switch (this.type) {
          case 'cuadrado':
            rectMode(CENTER);
            rect(x, y, s, s);
            break;
          case 'circulo':
            ellipseMode(CENTER);
            ellipse(x, y, s, s);
            break;
          case 'triangulo':
            push();
            translate(x, y);
            rotate(frameCount * 0.01);
            triangle(-s/2, s/2, 0, -s/2, s/2, s/2);
            pop();
            break;
          case 'rombo':
            push();
            translate(x, y);
            rotate(PI/4);
            rectMode(CENTER);
            rect(0, 0, s, s);
            pop();
            break;
        }
      }
    }
  </script>
</body>
</html>
```

este codigo genera figuras geometricas que al llegar a las esquinas rebotan con nuevas direcciones y angulos aleatorios, al dar clic los colores de las figuras cambian de forma aleatoria y el tamaño de ellas también se ve afectado 

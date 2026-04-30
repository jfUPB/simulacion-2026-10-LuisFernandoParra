# Unidad 7

## Bitácora de proceso de aprendizaje


## Bitácora de aplicación 
# Documentación de Proyecto: SMOKE (Instalación Interactiva)

## 1. Palabra Elegida
**SMOKE** (Humo)

## 2. Justificación Conceptual
La pieza explora la naturaleza efímera de la materia y la comunicación. El humo actúa como un velo que revela y oculta la información (la palabra), simbolizando cómo las ideas se disipan si no hay una acción constante (el aliento del usuario) para mantenerlas presentes.

## 3. Análisis de Significado Visual y Comportamental
*   **Visual:** Se utiliza una estética de partículas con transparencias degradadas y crecimiento cónico para imitar el comportamiento fluido del gas.
*   **Comportamental:** El sistema utiliza un **motor de físicas (Matter.js)** donde las letras tienen masa y fricción, reaccionando a fuerzas externas simuladas por el viento y el aliento.

## 4. Moodboard y Referencias
*   **Referencia Cinematográfica:** La atmósfera visual se inspira en el expresionismo y el cine fantástico, analizando texturas de directores como Guillermo del Toro.
*   **Referencia Técnica:** Instalaciones de arte generativo que utilizan el sonido como input físico para modificar partículas en tiempo real.

## 6. Bocetos y Mapa de Decisiones
*   **Decisión Crítica 1:** Uso de **FFT (Fast Fourier Transform)** para filtrar ruido ambiente. Se decidió priorizar frecuencias agudas (`highMid` y `treble`) para diferenciar el soplido de la voz humana en entornos ruidosos como el museo.
*   **Decisión Crítica 2:** Implementación de la letra "E" con un impulso vertical extra para garantizar una limpieza total del lienzo al final del ciclo.

## 7. Mapa de Interpretación
1.  **Entrada (Input):** Aliento del usuario captado por micrófono.
2.  **Proceso:** Análisis de espectro de audio -> Aplicación de fuerzas en Matter.js.
3.  **Salida (Output):** Revelación de letras formadas por partículas y partículas de humo en movimiento.

## 8. Relación entre Audio y Comportamiento
*   **Soplido Suave:** Genera nubes de transición y revela las letras de forma secuencial.
*   **Soplido de Cierre:** Al completar la palabra, el sistema detecta un soplido final que activa el audio `tos.mp3`, simbolizando la saturación, y limpia físicamente todas las letras del canvas.

## 9. Evidencia del Uso de IA
*   **Asistente:** Gemini (Google).
*   **Colaboración:** Optimización de algoritmos de filtrado de audio, resolución de conflictos en el motor de físicas Matter.js y refinamiento de la lógica de tracking de partículas para la instalación en el Museo Juan del Corral.

## 10. Código Fuente 
```JS
let engine, world;
let Engine, World, Bodies, Body, Constraint, Composite;
let mic, fft, myFont, soundTos;

// --- FILTRO DE MICROFONO CALIBRADO ---
let energyThreshold = 2.0; // El valor que detectamos que te funcionó
let consistencyCount = 0;   
let requiredConsistency = 2; 

let word = "SMOKE";
let wordOrder = [4, 3, 2, 1, 0]; 
let currentStep = -1; 
let smokeParticles = []; 
let lastSpawnTime = 0; 
let isResetting = false; 
let wipeX = -1000; 

function preload() {
  myFont = loadFont('https://raw.githubusercontent.com/google/fonts/main/ofl/modak/Modak-Regular.ttf');
  soundTos = loadSound('tos.wav', null, () => soundTos = null);
}

function setup() {
  createCanvas(windowWidth, windowHeight);
  Engine = Matter.Engine;
  World = Matter.World;
  Bodies = Matter.Bodies;
  Body = Matter.Body;
  Constraint = Matter.Constraint;
  Composite = Matter.Composite;
  
  engine = Engine.create();
  world = engine.world;
  world.gravity.y = 0; 
  
  mic = new p5.AudioIn();
  mic.start();
  fft = new p5.FFT();
  fft.setInput(mic);
  
  noCursor();
}

function draw() {
  background(10, 15, 25); 
  if (!myFont) return;

  Engine.update(engine);
  
  // ANALISIS CON EL NUEVO FILTRO
  fft.analyze();
  let high = fft.getEnergy("highMid");
  let treble = fft.getEnergy("treble");
  let soplidoReal = (high + treble) / 2; 

  let ahora = millis(); 
  let soplidoDetectado = false;

  if (soplidoReal > energyThreshold) {
    consistencyCount++;
  } else {
    consistencyCount = 0;
  }

  if (consistencyCount >= requiredConsistency) {
    soplidoDetectado = true;
  }

  // DISPARO BASADO EN EL FILTRO
  if (soplidoDetectado && (ahora - lastSpawnTime > 3500)) { 
    if (currentStep >= wordOrder.length - 1) {
      isResetting = true; 
      currentStep = -1; 
      lastSpawnTime = ahora;
      wipeX = -1000; 
      spawnResetSmoke(); 
    } else if (!isResetting) {
      currentStep++;
      moveExistingLetters(); 
      let letterIndex = wordOrder[currentStep];
      spawnRevealingLetter(word[letterIndex]);
      lastSpawnTime = ahora; 
    }
  }

  // --- LOGICA VISUAL ORIGINAL (MATTER.JS + HUMO CONO) ---
  let currentFront = -1000;
  let transitionFront = -1000;
  
  for (let p of smokeParticles) {
    if (p.isResetSmoke) {
      if (p.body.position.x > currentFront) currentFront = p.body.position.x;
    } else if (p.isTransitionCloud) {
      if (p.body.position.x > transitionFront) transitionFront = p.body.position.x;
    }
  }

  if (isResetting) {
    let targetWipe = (currentFront > -1000) ? currentFront - 30 : wipeX + (width * 0.05);
    if (targetWipe > wipeX) wipeX = targetWipe;
  }

  for (let i = smokeParticles.length - 1; i >= 0; i--) {
    let p = smokeParticles[i];
    let tiempoVivo = ahora - p.bornTime;

    if (p.isTransitionCloud) {
      Body.applyForce(p.body, p.body.position, { x: 0.0003, y: 0 }); 
      p.size = map(p.body.position.x, -350, width, 80, height * 1.3, true);
      
      let limitDistance = p.isResetSmoke ? width + 200 : width * 0.70;
      if (p.body.position.x > limitDistance) {
        p.life -= p.isResetSmoke ? 2.5 : 8; 
      } else if (tiempoVivo > 2500) {
        p.life -= 4;
      }

      push();
      noStroke();
      fill(255, 255, 255, 45 * (p.life/255)); 
      ellipse(p.body.position.x, p.body.position.y, p.size);
      pop();

      if (p.life <= 0) {
        Composite.remove(world, p.body);
        smokeParticles.splice(i, 1);
      }
    } 
    else {
      if (p.anchor && !isResetting) p.anchor.pointA.x = lerp(p.anchor.pointA.x, p.targetAnchorX, 0.05);
      
      if (isResetting) {
        if (p.body.position.x < wipeX) {
          p.visibleAlpha -= 7; 
          p.size += 0.6; 
          if (p.anchor) {
            World.remove(world, p.anchor);
            p.anchor = null; 
            Body.setVelocity(p.body, { x: p.body.velocity.x, y: random(-6, -9) }); 
          }
          Body.applyForce(p.body, p.body.position, { x: 0, y: -0.0004 });
        }
      } 
      else {
        p.size = lerp(p.size, p.targetSize, 0.04);
        if (transitionFront > p.body.position.x - 100) {
          p.visibleAlpha = lerp(p.visibleAlpha, 160, 0.08);
          p.body.frictionAir = 0.25; 
          Body.setVelocity(p.body, { x: p.body.velocity.x * 0.85, y: p.body.velocity.y * 0.85 });
        }
      }
      push();
      noStroke();
      fill(255, 255, 255, p.visibleAlpha * 0.3);
      ellipse(p.body.position.x, p.body.position.y, p.size * 1.5); 
      fill(255, 255, 255, p.visibleAlpha);
      ellipse(p.body.position.x, p.body.position.y, p.size);
      pop();

      if (isResetting && p.visibleAlpha <= 0) {
        Composite.remove(world, p.body);
        smokeParticles.splice(i, 1);
      }
    }
  }
  if (smokeParticles.length === 0) isResetting = false;
}

function moveExistingLetters() {
  for (let p of smokeParticles) {
    if (!p.isTransitionCloud) p.targetAnchorX += width * 0.18; 
  }
}

function spawnRevealingLetter(char) {
  let fontSize = height * 0.22; 
  let pts = myFont.textToPoints(char, 0, 0, fontSize, { sampleFactor: 0.15 }); 
  let bounds = myFont.textBounds(char, 0, 0, fontSize);
  let xOffset = (width * 0.15) - bounds.w / 2;
  let yOffset = height / 2 + bounds.h / 2;
  for (let p of pts) {
    let targetX = p.x + xOffset;
    let targetY = p.y + yOffset;
    let body = Bodies.circle(targetX, targetY, 6, { frictionAir: 0.1, isSensor: true });
    let anchor = Constraint.create({ pointA: { x: targetX, y: targetY }, bodyB: body, stiffness: 0.1, length: 0 });
    smokeParticles.push({ body: body, anchor: anchor, targetAnchorX: targetX, life: 255, visibleAlpha: 0, isTransitionCloud: false, size: 2, targetSize: random(18, 28), bornTime: millis() });
    World.add(world, [body, anchor]);
  }
  for (let j = 0; j < 95; j++) { 
    let cloudBody = Bodies.circle(random(-350, -20), height/2 + random(-200, 200), 10, { frictionAir: 0.025, isSensor: true });
    Body.setVelocity(cloudBody, { x: width * 0.018 + random(-2, 4), y: random(-4, 4) });
    smokeParticles.push({ body: cloudBody, isTransitionCloud: true, life: 255, size: 60, bornTime: millis() });
    World.add(world, cloudBody);
  }
}

function spawnResetSmoke() {
  if (soundTos) soundTos.play();
  for (let j = 0; j < 160; j++) { 
    let cloudBody = Bodies.circle(random(-500, -20), height/2 + random(-350, 350), 10, { frictionAir: 0.025, isSensor: true });
    Body.setVelocity(cloudBody, { x: width * 0.025 + random(-3, 6), y: random(-5, 5) });
    smokeParticles.push({ body: cloudBody, isTransitionCloud: true, isResetSmoke: true, life: 255, size: 60, bornTime: millis() });
    World.add(world, cloudBody);
  }
}

function mousePressed() {
  userStartAudio();
  if (getAudioContext().state !== 'running') getAudioContext().resume();
}

function keyPressed() {
  if (key === 'f' || key === 'F') fullscreen(!fullscreen());
}

function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
}
```
## 11. Enlace al sketch:
*   https://editor.p5js.org/LuisFernandoParra/full/taw2zLyiN
## 12 capturas o registros
<img width="717" height="583" alt="image" src="https://github.com/user-attachments/assets/0cda45da-e274-43ca-8723-0a60ce05b922" />
<img width="1177" height="583" alt="image" src="https://github.com/user-attachments/assets/5d9f34fb-791d-4578-a168-e9310f276ffc" />
<img width="1220" height="572" alt="image" src="https://github.com/user-attachments/assets/b419e313-9d2d-4b47-9870-49934fc76842" />
<img width="1133" height="400" alt="image" src="https://github.com/user-attachments/assets/0ce4af17-1726-4037-b840-fa3239f18a76" />
<img width="1216" height="575" alt="image" src="https://github.com/user-attachments/assets/8a613c48-ed31-4697-be73-08474a9d26a7" />

## Bitácora de reflexión

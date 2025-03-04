# Solución

## ¿Qué es p5LiveMedia?

P5LiveMedia es una biblioteca para p5.js que facilita la transmisión en tiempo real de audio, video, el lienzo de p5 (canvas) y datos entre usuarios utilizando WebRTC. Permite a los desarrolladores crear aplicaciones interactivas que comparten medios en vivo de manera eficiente y sencilla. 

## ¿Qué posibilidades ofrece p5LiveMedia?

Esta biblioteca ofrece diversas funcionalidades que enriquecen las aplicaciones interactivas:​

- **Transmisión de video en vivo:** Permite compartir secuencias de video capturadas desde la cámara del usuario con otros participantes en tiempo real.​

- **Transmisión de audio en vivo:** Facilita la compartición de audio capturado desde el micrófono del usuario, habilitando comunicaciones de voz en directo.​

- **Compartir el lienzo de p5 (canvas):** Es posible transmitir el contenido del canvas de p5.js como si fuera un video, lo que permite compartir gráficos generados por código en tiempo real.​

- **Intercambio de datos:** Ofrece la capacidad de enviar y recibir datos personalizados (actualmente solo cadenas de texto), lo que es útil para sincronizar estados, enviar mensajes o cualquier otra información relevante entre usuarios.

## Ejemplos

- **Compartir video en vivo:**

  ```js
    let myVideo = null;
  let otherVideo = null;

  function setup() {
    createCanvas(400, 400);

    // Captura de video desde la cámara
    myVideo = createCapture(VIDEO, function(stream) {
      let p5lm = new p5LiveMedia(this, "CAPTURE", stream, "roomName");
      p5lm.on('stream', gotStream);
    });

    myVideo.elt.muted = true; // Evitar retroalimentación de audio
  }

  function draw() {
    background(220);

    // Mostrar el video propio
    if (myVideo !== null) {
      image(myVideo, 0, 0, width / 2, height);
      text("Mi Video", 10, 10);
    }

    // Mostrar el video de otro usuario
    if (otherVideo !== null) {
      image(otherVideo, width / 2, 0, width / 2, height);
      text("Video de Otro Usuario", width / 2 + 10, 10);
    }
  }

  function gotStream(stream, id) {
    otherVideo = stream;
  }
  ```
- **Compartir el lienzo de P5 (canvas):**

  ```js
    let otherCanvas;

  function setup() {
    let myCanvas = createCanvas(400, 400);
    let p5lm = new p5LiveMedia(this, "CANVAS", myCanvas, "roomName");
    p5lm.on('stream', gotStream);
  }

  function draw() {
    background(220);
    fill(255, 0, 0);
    ellipse(mouseX, mouseY, 100, 100);
  }

  function gotStream(stream) {
    otherCanvas = stream;
  }
  ```

- **Compartir datos:**
 
  ```js
    let otherX = 0;
  let otherY = 0;
  let p5lm;

  function setup() {
    createCanvas(400, 400);

    // Inicializar p5LiveMedia para compartir datos
    p5lm = new p5LiveMedia(this, "DATA", null, "roomName");
    p5lm.on('data', gotData);
  }

  function draw() {
    background(220);

    // Dibujar elipse en la posición del mouse
    fill(255, 0, 0);
    ellipse(mouseX, mouseY, 50, 50);

    // Dibujar elipse en la posición recibida de otro usuario
    fill(0, 255, 0);
    ellipse(otherX, otherY, 50, 50);
  }

  function mouseMoved() {
    // Enviar posición del mouse a otros usuarios
    let dataToSend = { x: mouseX, y: mouseY };
    p5lm.send(JSON.stringify(dataToSend));
  }

  function gotData(data, id) {
    // Actualizar posición recibida de otro usuario
    let receivedData = JSON.parse(data);
    otherX = receivedData.x;
    otherY = receivedData.y;
  }
  ```
  ## Hallazgos y concideraciones
- **Configuración inicial:** Es esencial incluir las bibliotecas simplepeer y socket.io en el HTML de la aplicación para que p5LiveMedia funcione correctamente.
- **Gestión de streams:** Dependiendo de la funcionalidad deseada (audio, video, canvas), se debe especificar el tipo de stream al inicializar p5LiveMedia.
- **Manejo de eventos:** Implementar callbacks para eventos como stream y data es crucial para gestionar la recepción de medios y datos de otros usuarios.
- **Sincronización y latencia:** Al compartir datos en tiempo real, es importante considerar posibles retrasos y optimizar la transmisión para una experiencia fluida.
  

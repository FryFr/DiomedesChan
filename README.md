# 🪗 Diomedes Chan — OLED + Música + Servos con Arduino

<div align="center">

![Arduino](https://img.shields.io/badge/Arduino-00979D?style=for-the-badge&logo=arduino&logoColor=white)
![C++](https://img.shields.io/badge/C%2FC%2B%2B-00599C?style=for-the-badge&logo=cplusplus&logoColor=white)
![Dificultad](https://img.shields.io/badge/Dificultad-2%2F10-brightgreen?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)
![Version](https://img.shields.io/badge/Version-1.0.0-blue?style=for-the-badge)

**Proyecto de electrónica creativa que muestra una imagen bitmap en un display OLED 128×64, reproduce una melodía con un zumbador pasivo y anima dos servomotores y LEDs al ritmo de la música — todo con Arduino Uno.**

[¿Cómo funciona?](#-cómo-funciona) · [Hardware](#-hardware-requerido) · [Conexiones](#-diagrama-de-conexiones) · [Instalación](#-instalación) · [Personalización](#-personalización)

</div>

---

## 📋 Tabla de Contenidos

- [¿Qué es este proyecto?](#-qué-es-este-proyecto)
- [¿Cómo funciona?](#-cómo-funciona)
- [Hardware requerido](#-hardware-requerido)
- [Stack tecnológico](#-stack-tecnológico)
- [Diagrama de conexiones](#-diagrama-de-conexiones)
- [Instalación](#-instalación)
- [Lógica del código](#-lógica-del-código)
- [Personalización](#-personalización)
- [Simulación online](#-simulación-online)
- [Estructura del proyecto](#-estructura-del-proyecto)
- [Errores comunes](#-errores-comunes)
- [Roadmap](#-roadmap)
- [Contribuciones](#-contribuciones)
- [Créditos](#-créditos)
- [Licencia](#-licencia)

---

## 🤖 ¿Qué es este proyecto?

**Diomedes Chan** es un proyecto de electrónica creativa de dificultad **2/10**. Al encenderse, el Arduino muestra una imagen bitmap personalizada en un display OLED I2C de 128×64 píxeles durante 5 segundos. Luego, reproduce una melodía convertida desde MIDI usando un zumbador pasivo, mientras dos servomotores oscilan y dos LEDs parpadean al compás de cada nota — creando una pequeña animación sincronizada con la música.

El proyecto introduce tres conceptos clave en un solo circuito: **visualización de imágenes en OLED**, **generación de tonos con `tone()`** y **control de servos en tiempo real**.

---

## ⚙️ ¿Cómo funciona?

```
  ┌─────────────────────────────────────────────────┐
  │               CICLO PRINCIPAL                   │
  │                                                 │
  │  1. Mostrar imagen bitmap en OLED (5 segundos)  │
  │              │                                  │
  │              ▼                                  │
  │  2. playMusic() — ejecuta la melodía nota a nota│
  │              │                                  │
  │              ▼                                  │
  │  3. Por cada nota → beep(frecuencia, duración)  │
  │       ├── tone() en el zumbador                 │
  │       ├── Servo 1 y Servo 2 oscilan             │
  │       ├── LED alterna (par / impar)             │
  │       └── noTone() al terminar la nota          │
  │              │                                  │
  │              ▼                                  │
  │  4. Vuelve al paso 1 → loop infinito            │
  └─────────────────────────────────────────────────┘
```

La función `beep()` usa un contador para alternar entre dos estados en cada nota: posición A de los servos + LED 1, o posición B de los servos + LED 2. Esto crea el efecto visual de "baile" sincronizado con la melodía.

---

## 🔩 Hardware Requerido

| Componente | Modelo | Cantidad | Función |
|---|---|---|---|
| Microcontrolador | Arduino UNO | 1 | Control general |
| Display OLED I2C | SSD1306 128×64 | 1 | Mostrar imagen bitmap |
| Zumbador pasivo | Buzzer pasivo | 1 | Reproducir melodía con `tone()` |
| Servomotor | SG90 | 2 | Animación al ritmo de la música |
| LED | Cualquier color | 2 | Parpadeo sincronizado con la música |
| Protoboard | — | 1 | Conexionado |
| Jumpers M-M | — | ~15 | Conexiones |

> ⚠️ **Zumbador pasivo vs activo:** Este proyecto requiere un **zumbador pasivo**. El zumbador activo solo emite un pitido fijo y no responde a `tone()`. Si el tuyo emite siempre el mismo sonido sin importar la frecuencia, es activo — debes cambiarlo.

---

## 🛠 Stack Tecnológico

| Capa | Tecnología | Notas |
|---|---|---|
| Microcontrolador | Arduino UNO | — |
| IDE | Arduino IDE | `1.8+` |
| Lenguaje | C / C++ (Arduino) | — |
| Librería OLED | Adafruit_SSD1306 | Requiere instalación |
| Librería gráfica | Adafruit_GFX | Requiere instalación |
| Control de servos | Servo.h | Incluida en Arduino IDE |
| Generación de tonos | `tone()` nativo | Incluida en Arduino IDE |
| Comunicación display | I2C / Wire.h | Incluida en Arduino IDE |
| Imágenes | Bitmap PROGMEM | Generado con image2cpp |
| Música | MIDI → Arduino | Convertido con MidiToArduino |

---

## 📐 Diagrama de Conexiones

### Tabla de pines

| Componente | Pin del componente | Pin Arduino UNO |
|---|---|---|
| OLED SSD1306 | SDA | Analógico **A4** |
| OLED SSD1306 | SCL | Analógico **A5** |
| OLED SSD1306 | VCC | 3.3V o 5V |
| OLED SSD1306 | GND | GND |
| Zumbador pasivo | `+` | Digital **6** |
| Zumbador pasivo | `-` | GND |
| Servomotor 1 | Señal | Digital **5** (PWM) |
| Servomotor 2 | Señal | Digital **3** (PWM) |
| Servomotor 1 y 2 | VCC (rojo) | 5V |
| Servomotor 1 y 2 | GND (negro) | GND |
| LED 1 | Ánodo (+) | Digital **12** |
| LED 2 | Ánodo (+) | Digital **13** |
| LED 1 y 2 | Cátodo (−) | GND (con resistencia) |

### Esquema del circuito

![Esquema de conexiones](https://user-images.githubusercontent.com/79547422/222854365-495245af-8845-4cc5-8894-7aa49c662605.JPG)

---

## 🚀 Instalación

### Paso 1 — Instalar Arduino IDE

🔗 [Descargar Arduino IDE](https://www.arduino.cc/en/software)

### Paso 2 — Instalar las librerías necesarias

Abre Arduino IDE y ve a **Herramientas → Administrar bibliotecas**. Busca e instala las siguientes:

| Librería | Autor |
|---|---|
| `Adafruit SSD1306` | Adafruit |
| `Adafruit GFX Library` | Adafruit |

> Las librerías `Servo.h` y `Wire.h` ya vienen incluidas con Arduino IDE.

### Paso 3 — Descargar el código

```bash
git clone https://github.com/FryFr/DiomedesChan.git
```

O descárgalo desde GitHub con **Code → Download ZIP**.

### Paso 4 — Subir el sketch

Abre `Codigo/DiomedesChan.ino` en Arduino IDE, conecta el Arduino por USB, selecciona el puerto en **Herramientas → Puerto** y haz clic en **Subir**. A los 5 segundos de encendido, empezará la música. 🪗

---

## 🧠 Lógica del Código

### Renderizado del bitmap en OLED

La imagen se almacena como un array de bytes en memoria flash (`PROGMEM`) para no consumir RAM:

```cpp
const unsigned char epd_bitmap_Proyecto_nuevo[] PROGMEM = {
    0x00, 0x00, ... // 128x64 px = 1024 bytes
};

// Dibujarlo en el display:
display.drawBitmap(0, 0, epd_bitmap_Proyecto_nuevo, 128, 64, 1);
display.display();
```

### Función `beep()` — nota + animación sincronizada

```cpp
void beep(int note, int duration) {
    tone(tonePin, note, duration);   // Toca la nota

    if (counter % 2 == 0) {
        // Estado A: servo en 90°/110°, LED 1 encendido
        servoMotor1.write(90);
        servoMotor2.write(110);
        digitalWrite(ledPin1, HIGH);
        delay(duration);
        digitalWrite(ledPin1, LOW);
    } else {
        // Estado B: servo en 45°/135°, LED 2 encendido
        servoMotor1.write(45);
        servoMotor2.write(135);
        digitalWrite(ledPin2, HIGH);
        delay(duration);
        digitalWrite(ledPin2, LOW);
    }

    noTone(tonePin);
    delay(50);
    counter++;   // Alterna el estado en la próxima nota
}
```

### Ciclo principal

```cpp
void loop() {
    display.clearDisplay();
    display.drawBitmap(0, 0, epd_bitmap_Proyecto_nuevo, 128, 64, 1);
    display.display();
    delay(5000);    // Muestra imagen 5 segundos
    playMusic();    // Toca la melodía con animación
}
```

---

## 🎨 Personalización

### Cambiar la imagen del OLED

Usa el conversor online **image2cpp** para convertir cualquier imagen PNG/JPG a un array de bytes compatible con Arduino:

🔗 [image2cpp — Conversor de imagen a bitmap](https://javl.github.io/image2cpp/)

Configura el canvas en **128×64 px**, genera el array y reemplaza el contenido de `epd_bitmap_Proyecto_nuevo[]` en el código.

![Configuración image2cpp](https://user-images.githubusercontent.com/79547422/222855593-53550f38-a455-41f2-ae12-2a10b7876e73.JPG)

### Cambiar la melodía

Convierte cualquier archivo MIDI a código Arduino con **MidiToArduino**:

🔗 [MidiToArduino — Conversor MIDI](https://extramaster.net/tools/midiToArduino/)

El conversor entrega código con `tone(tonePin, nota, duracion)`. Adáptalo al formato `beep()` del proyecto eliminando el `tonePin`:

```cpp
// Salida del conversor:
tone(tonePin, 1046, 741.27);

// Formato del proyecto:
beep(1046, 823.64);
delay(100);
```

Luego reemplaza el contenido de `playMusic()` con las nuevas notas.

### Cambiar los ángulos de los servos

Ajusta los valores en `beep()` para cambiar el rango de movimiento:

```cpp
// Estado A
servoMotor1.write(90);    // 0° a 180° según el efecto deseado
servoMotor2.write(110);

// Estado B
servoMotor1.write(45);
servoMotor2.write(135);
```

---

## 🖥 Simulación Online

Puedes probar el proyecto sin hardware en Wokwi:

🔗 [Simular en Wokwi](https://wokwi.com/projects/357220558913843201)

---

## 📁 Estructura del Proyecto

```
DiomedesChan/
│
├── Codigo/
│   └── DiomedesChan.ino              # Sketch principal
│
├── Achivos/
│   ├── 40-408870_cute-smiley(...).png  # Imagen fuente del bitmap
│   └── y2mate.com - Sin medir (...).mp3  # Referencia de la melodía
│
└── README.md                         # Documentación
```

---

## ⚠️ Errores Comunes

| Error | Causa | Solución |
|---|---|---|
| OLED no muestra nada | Dirección I2C incorrecta | Cambia `0x3C` por `0x3D` en `display.begin()` |
| El zumbador solo hace un pitido fijo | Es un zumbador activo, no pasivo | Reemplázalo por un zumbador pasivo |
| Servos vibran sin moverse | Alimentación insuficiente del Arduino | Alimenta los servos desde una fuente externa de 5V |
| Error `Adafruit_SSD1306.h not found` | Librería no instalada | Instala `Adafruit SSD1306` y `Adafruit GFX` desde el gestor de bibliotecas |
| Imagen invertida o con ruido | Canvas mal configurado en image2cpp | Configura exactamente 128×64 px y modo "Horizontal" |
| Error al subir | Puerto COM incorrecto | Ve a **Herramientas → Puerto** y selecciona el correcto |

---

## 🗺 Roadmap

- [ ] Soporte para múltiples imágenes con animación frame a frame en el OLED
- [ ] Botón para cambiar entre varias melodías almacenadas
- [ ] Sincronización más precisa nota-servo usando interrupciones
- [ ] Versión con potenciómetro para controlar el volumen / velocidad
- [ ] Carcasa 3D imprimible para montar el circuito

---

## 🤝 Contribuciones

¿Tienes una melodía diferente o una imagen chévere para el OLED? ¡Súbela!

1. **Fork** este repositorio
2. Crea tu rama:
   ```bash
   git checkout -b feature/nueva-melodia
   ```
3. Commitea con mensajes claros:
   ```bash
   git commit -m "feat: agregar melodía de Carlos Vives"
   ```
4. Abre un **Pull Request**

### Convención de commits

| Prefijo | Uso |
|---|---|
| `feat:` | Nueva funcionalidad o melodía |
| `fix:` | Corrección de bug |
| `art:` | Nueva imagen bitmap |
| `docs:` | Solo documentación |

---

## 👨‍💻 Créditos

**Juan Silva Medina**
- GitHub: [@FryFr](https://github.com/FryFr)
- LinkedIn: [linkedin.com/in/jsilva-medina](https://www.linkedin.com/in/jsilva-medina/?skipRedirect=true)
- YouTube: [@juansilva4256](https://www.youtube.com/@juansilva4256)

**Inspiración del proyecto**
- TikTok original: [@donielsaurio](https://www.tiktok.com/@donielsaurio/video/7198698230345911557)

---

## 📄 Licencia

Distribuido bajo la licencia **MIT**. Consulta el archivo [`LICENSE`](./LICENSE) para más detalles.

```
MIT License — Copyright (c) 2023 Juan Silva Medina
```

---

<div align="center">
  <sub>Hecho con ❤️, bytes en PROGMEM y mucho vallenato · ¿Te gustó? Dale una ⭐ al repo</sub>
</div>

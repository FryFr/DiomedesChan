### Diomedes Chan

Este es un proyecto de dificultad 2/10 *facil*, en el cual vamos a utilizar matrices de bits para visualizar imagenes en un display OLED de comunicacion I2C mezclandola con la funcion tone() con un Arduino Uno.

Sientete libre de mejorar el codigo o proveer de diferentes melodias para este proyecto!

Vamos a iniciar con los materiales a utilizar como las conexiones a realizar, en este caso es un circuito bastante sencillo:

- Zumbador pasivo
- Sevomootor sg90 (X2)
- Display OLED I2C de 128x64
- Arduino Uno
- Protoboard
- Cables 
- soporte

Para las conexiones vamos a seguir el siguiente esquema:
![Captura](https://user-images.githubusercontent.com/79547422/222854365-495245af-8845-4cc5-8894-7aa49c662605.JPG)

Para poder utilizar la musica en nuestro zumbador es importante transformar nuestro archivo MIDI a codigo que C# pueda leer, para ello vamos a utilizar un conversor online, [Click aqui](https://extramaster.net/tools/midiToArduino/), este nos va a entregar el siguiente tipo de codigo:

```arduino
void midi() {
    tone(tonePin, 1046, 741.279069767);
    delay(823.643410853);
    delay(261.627906977);
    tone(tonePin, 932, 156.976744186);
    delay(174.418604651);
}
void loop() {
    // Play midi
    midi();
}
```
Este codigo nos entrega directamente la funcion donde nos entrega el pin del zumbador, el tono y la duracion del tono, nuestra funcion beep solo utilizara los ultimos datos, la funcion tone la podemos cambiar por la funcion beep que declaramos en nuestro codigo y eliminamos el "tonePin" que no vamos a utilizar directamente: 

```arduino
void playMusic() {
	  beep( 1046, 823.643410853);
	  delay(100);
	  beep( 932, 174.418604651);
	  delay(50);
	  beep( 932, 300.387596899);
	  delay(50);
	  beep( 830, 184.108527132);
	  delay(50);
	  beep( 830, 184.68992248062);
	  delay(50);
	  beep( 784, 184.68992248062);
	  delay(50);
	  beep( 830, 300.178294574);
	  delay(319);
  }
```
Para el mapa de bits podemos usar igualmente un conversor online [Click aqui](https://javl.github.io/image2cpp/) en este podemos configurar nuestro mapa de bits, al generarlo nos entregara un array de cual podemos extraer solamente los datos dentro de el y reemplazarlo en nuestro codigo.

![Captura](https://user-images.githubusercontent.com/79547422/222855593-53550f38-a455-41f2-ae12-2a10b7876e73.JPG)

# Links:
> - https://wokwi.com/projects/357220558913843201
> - https://extramaster.net/tools/midiToArduino/
> - https://javl.github.io/image2cpp/

#Referencia del proyecto

- https://www.tiktok.com/@donielsaurio/video/7198698230345911557
Tiktok: @donielsaurio

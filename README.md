# Autonomos en VEX IQ
En este tutorial práctico, te sumergirás en la creación de autonomos de manera profesional. A través de la aplicación de los principios de programación orientada a objetos, aprenderás a reutilizar código de manera efectiva, evitando la redundancia y optimizando al máximo los algoritmos involucrados.

## Requisitos
Para seguir este tutorial práctico, es recomendable tener instalados los siguientes programas:

- `Visual Studio Code (vscode)`: Es el editor de código más popular en la actualidad.
- `VEXCode IQ`: Este es el compilador del lenguaje de programación exclusivo para VEX IQ.

## Entidades
Las "entidades" son como bloques de construcción que representan cosas del mundo real o ideas abstractas. Imagina que estás creando un juego de computadora. En este juego, podrías tener entidades como "jugador", "enemigo", "arma", "objeto recolectable", etc.

Cada una de estas entidades tiene dos tipos de características:

- Atributos: Son como características que describen a la entidad. Por ejemplo, un jugador podría tener atributos como "puntos de vida", "nivel", "nombre", etc.

- Métodos: Son como acciones que la entidad puede realizar. Siguiendo con el ejemplo del jugador, podría tener métodos como "atacar", "moverse", "recoger objeto", etc.

La programación orientada a objetos nos permite organizar nuestro código de manera más fácil y comprensible, ya que podemos agrupar estos atributos y métodos relacionados dentro de cada entidad. Esto hace que sea más fácil de entender y mantener nuestro código a medida que el proyecto crece.

## Entidades en VEXCode IQ
Para definir entidades en VEXCode IQ, primero crearemos una carpeta para organizar nuestros archivos. Por ejemplo, podemos llamar a esta carpeta `autonomos`.

Luego, importaremos esta carpeta en vscode y crearemos un archivo para cada entidad que queramos definir. El nombre de cada archivo debe reflejar el tipo de robot con el que estamos trabajando. Por ejemplo, si estamos trabajando con un robot de tipo rodillo, podríamos llamar al archivo `RobotRodillo.iqcpp`, si estamos trabajando con un robot de tipo canasta, pues llamaremos al archivo `RobotCanasta.iqcpp`. Para este ejemplo práctico usaremos un robot de tipo rodillo.

Lo primero que haremos sera abrir el documento `RobotRodillo.iqcpp`, e importaremos las siguientes librerias:

```cpp
// Incluimos la libreria de IQ
#include "iq_cpp.h"

// Permitimos para un facil uso de la libreria VEX
using namespace vex;
```

Despues lo que haremos sera definir los **atributos** del robot. Como mencionamos anteriormente, los atributos son "características que describen a la entidad". En este caso lo que describe nuestro robot son sus motores, así que vamos a definir que motores llevara el robot. Para ello seguiremos el siguiente proceso para crear las variables que seran los atributos del robot:

1. Establecemos el tipo de dato de la variable como `motor`.
2. Asignamos un nombre a la variable, como por ejemplo `ChasisIzq`, que representa el motor.
3. Inicializamos la variable con un valor inicial. Aunque el valor inicial no sea crucial en este momento, es una buena práctica inicializar las variables con algún valor. Por lo tanto, asignaremos a todos los motores el puerto PORT1 de la siguiente manera: `motor ChasisIzq = PORT1`.


```cpp
// Definimos los motores que queremos para el robot sin importar el puerto
motor ChasisIzq = PORT1;
motor ChasisDer = PORT1;
motor TorreIzq = PORT1;
motor TorreDer = PORT1;
motor Rodillo = PORT1;
```

Lo siguiente que haremos sera programar un `constructor`. En la programación orientada a objetos, un constructor es una especie de "método especial" que se utiliza para crear e inicializar objetos de un programa. El constructor es como la línea de ensamblaje que toma la entidad y crea una instancia específica del objeto. Más adelante veremos como utilizarlo para inicializar los motores con sus puertos correctos.

Para generar nuestro constructor, crearemos una función de tipo `void` (osea que no retorna nada), con el nombre de nuestra entidad, en este caso `RobotRodillo`, y como parametros de la función, le pasaremos el numero de motores que tiene el robot, que en este caso son 5. Para esto entre parentesis escribiremos el tipo de dato, y el motor con su numero: `(motor motor1, ..., motor motor5)`.

Ya dentro de las llaves `{}` del constructor, le asignaremos cada motor a su respectivo atributo, por ejemplo: `ChasisIzq = motor1;`.

```cpp
// Definimos el constructor de la Entidad
void RobotRodillo (motor motor1, motor motor2, motor motor3, motor motor4, motor motor5) 
{
    ChasisIzq = motor1;
    ChasisDer = motor2;
    TorreIzq = motor3;
    TorreDer = motor4;
    Rodillo = motor5;
}
```

### Métodos de la entidad
Ahora programaremos los métodos de nuestra entidad. Recordemos que los métodos son "acciones que la entidad puede realizar", en este caso son las acciones que nuestro robot puede realizar, como girar el rodillo, subir y bajar brazos con las torres, avanzar, retroceder, etc... En este caso nos enfocaremos en hacer métodos genericos para cada sección del robot, y le definiremos velocidad, dirección, tiempo, etc...

Primero crearemos un método para el chasis. Para ello definiremos una función de tipo `void`, y le pondremos un nombre representativo de la acción que hara el chasis, yo le puse `useChasis`. Despues le asignaremos entre parentesís los siguientes parametros:
- `directionType direccion`: Dirección del sentido de giro (forward o reverse).
- `int vel1`: Velocidad del chasis izquierdo (de 0 a 100).
- `int vel2`: Velocidad del chasis derecho (de 0 a 100).
- `bool detener = false`: Con esto podremos validar si queremos que el chasis se detenga una vez pase el tiempo que le asignemos, o si queremos que giré indefinidamente hasta que más adelante le digamos que se detenga. Por defecto es falso, así haremos que por defecto gire indefinidamente y el parametro sea opcional.
- `int tiempo = 10`: Le asignamos cuanto tiempo queremos que gire el chasis en milisegundos. Le asignamos de una vez 10 milisegundos para hacer opcional este valor.

Así debera de lucir los parametros ya juntos: `(directionType direccion, int vel1, int vel2, bool detener, int tiempo = 10)`

Dentro de las llaves `{}` de la función escribiremos la lógica de nuestro método de la siguiente manera:
- Le decimos a los motores del chasis que giren a la dirección y velocidad que le asignamos en los parametros: `ChasisIzq.spin(direccion, vel1, percent);`
- Le decimos en base al parametro de tiempo, por cuanto tiempo queremos que haga esto: `wait(tiempo, msec);`
- Validamos de que si el tiempo es mayor a 0, y que el parametro "detener" es verdadero (true), se detengan los motores una vez haya pasado ese tiempo. Si el parametro "detener" es false, el robot girará indefinidamente hasta que más adelante lo cambiemos a true.

```cpp
// Creamos el método para controlar el chasis
void useChasis(directionType direccion, int vel1, int vel2, bool detener = false, int tiempo = 10)
{
  ChasisIzq.spin(direccion, vel1, percent);
  ChasisDer.spin(direccion, vel2, percent);
  wait(tiempo, msec);

  // Validamos de que si el tiempo es mayor a 0, y que el parametro "detener" es verdadero (true)
  if (tiempo > 0 && detener)
  {
    ChasisIzq.stop();
    ChasisDer.stop();
  }
}
```

Para usar este método en nuestro código principal que desarrollaremos en VEXCode IQ, les mostraré una pequeña guía de como mandarlo a llamar y que acciones puede hacer:

```cpp
  /*     direccion, vel1, vel2, detener, tiempo */
  useChasis(forward, 100, 100, true, 1000); // Avanza por un segundo y se detiene

  useChasis(reverse, 100, 100, true, 1000); // Retrocede por un segundo y se detiene

  useChasis(forward, 100, -100, true, 1000); // Gira por un segundo

  useChasis(forward, -100, 100, true, 1000); // Gira al otro lado por un segundo

  useChasis(foward, 100, 100); // Gira indefinidamente

  useChasis(forward, 0, 0, true, 100); // Lo detenemos por completo
```

Más adelante te enseñare a como combinar otros métodos junto con este para por ejemplo, el rodillo gire al mismo tiempo que el chasis para recolectar cubos. (O lo que sea que recolecte el robot en su actual temporada).

Ahora programaremos el método para controlar las torres. Francamente es basicamente hacer lo mismo que hicimos en `useChasis`, con la diferencia de que en vez de 2 velocidades (vel1, y vel2), solo ocuparemos una, ya que los dos motores de las torres siempre girarán al mismo sentido, no como el chasis que para girar ocupan girar en sentidos opuestos.

```cpp
// Creamos funcion para controlar las torres
void useTorres(directionType direccion, int vel, bool detener = false, int tiempo = 10)
{
  TorreIzq.spin(direccion, vel, percent);
  TorreDer.spin(direccion, vel, percent);
  wait(tiempo, msec);

  if (tiempo > 0 && detener)
  {
    TorreIzq.stop();
    TorreDer.stop();
  }
}
```

Como reto personal, escribe el código para controlar el rodillo.

Algo así debera lucir tu entidad en vscode:

```cpp
// Incluimos la librerya de IQ
#include "iq_cpp.h"

// Permitimos para un facil uso de la libreria VEX
using namespace vex;

// Definimos los motores que queremos para el robot sin importar el puerto
motor ChasisIzq = PORT1;
motor ChasisDer = PORT1;
motor TorreIzq = PORT1;
motor TorreDer = PORT1;
motor Rodillo = PORT1;

// Definimos la funcion constructora, mejor conocida como constructor
void RobotRodillo (motor motor1, motor motor2, motor motor3, motor motor4, motor motor5) 
{
    ChasisIzq = motor1;
    ChasisDer = motor2;
    TorreIzq = motor3;
    TorreDer = motor4;
    Rodillo = motor5;
}

// Creamos funcion para controlar el chasis
void useChasis(directionType direccion, int vel1, int vel2, bool detener = false, int tiempo = 10)
{
  ChasisIzq.spin(direccion, vel1, percent);
  ChasisDer.spin(direccion, vel2, percent);
  wait(tiempo, msec);

  if (tiempo > 0 && detener)
  {
    ChasisIzq.stop();
    ChasisDer.stop();
  }
}

// Creamos funcion para controlar el rodillo
void useRodillo(directionType direccion, int vel, bool detener = false, int tiempo = 10)
{
  Rodillo.spin(direccion, vel, percent);
  wait(tiempo, msec);

  if (tiempo > 0 && detener)
  {
    Rodillo.stop();
  }
}

// Creamos funcion para controlar las torres
void useTorres(directionType direccion, int vel, bool detener = false, int tiempo = 10)
{
  TorreIzq.spin(direccion, vel, percent);
  TorreDer.spin(direccion, vel, percent);
  wait(tiempo, msec);

  if (tiempo > 0 && detener)
  {
    TorreIzq.stop();
    TorreDer.stop();
  }
}
```

## Código principal
Ya ahora si en VEXCode IQ, crearemos un archivo llamado `Autonomo.iqcpp`, y lo guardaremos en la misma carpeta de donde esta nuestra entidad.

Definiremos los puertos de los motores como común mente se hace en VEXCode IQ, abriendo el menú que esta arriba a la derecha. Toma nota de que los nombres de los motores no pueden ser iguales a los nombres de los atributos de la entidad, en este caso por ejemplo no puedes llamar a ningún motor como `ChasisIzq` o como los demas atributos. Busca otro nombre parecido para hacer referencia al atributo, así como: `ChIzq`.

Despues importaremos la entidad RobotRodillo en nuestro archivo, justo abajo de la linea `#include "iq_cpp.h"`

```cpp
// Include the IQ Library
#include "iq_cpp.h"

// Importamos nuestra entidad
#include <C:\Users\franc\OneDrive\Escritorio\Róbotica\autonomos\RobotRodillo.iqcpp>
```

Ya dentro del método `main()` podremos iniciar a programar nuestro autonomo. lo primero que haremos sera programar nuestro constructor. Para ellos mandaremos a llamar la función de nuestro constructor, y le pasaremos los motores que asignaste anteriormente en el órden en que pusiste los atributos:

```cpp
RobotRodillo(ChIzq, ChDer, TIzq, TDer, Rod);
```

La ventaja de utilizar una entidad es que puedes reutilizar el mismo archivo para cada robot que tengas. Imagina que tienes 10 robots de rodillo, y cada uno tiene los motores en diferentes puertos. Con el constructor automaticamente podrás reutilizar la entidad en los 10 robots sin importar que tengan los motores en diferentes puertos, ya que el constructor asignara los métodos a sus respectivos motores sin importar el puerto del motor.

Abajo del constructor iniciaremos a programar nuestro autonomo, en este caso te dejare algunos ejemplos de como combinar los diferentes metodos para obtener distintos resultados. Te recomiendo probar uno por uno para ver que funcionen, y por ejemplo, evita programar que motores como los de la torre giren indefinidamente ya que puedes dañar el robot.

```cpp
  useChasis(forward, 100, 100, true, 1000); // Avanza por un segundo y se detiene
  useTorres(forward, 100, true, 700); // Sube las torres por 700 milisegundos y se detiene
  useRodillo(forward, 100, true, 2000); // Gira el rodillo por 2 segundos y se detiene

  useTorres(reverse, 100, true, 700); // Bajamos las torres por 700 milisegundos y se detiene
  useRodillo(reverse, 100); // Giramos el rodillo indefinidamente
  useChasis(forward, 60, 60, true, 2000); // Avanza el robot por dos segundos
  useRodillo(forward, 0, true); // Detiene el rodillo

  useChasis(forward, 100, -100, true, 1000); // Gira el robot 1 segundo y se detiene
  useChasis(forward, 100, 100); // Avanza el robot indefinidamente
  useTorres(forward, 100, true, 700); // Sube las torres por 700 milisegundos
  useChasis(forward, 100, 100, true, 2000); // Avanza el robot 2 segundos más y se detiene
  useRodillo(forward, 100, true, 3000); // Giramos el rodillo 3 segundos y se detiene
```

Algo así debera lucir tu código en VEXCode IQ.

```cpp
#pragma region VEXcode Generated Robot Configuration
// Make sure all required headers are included.
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>
#include <math.h>
#include <string.h>


#include "vex.h"

using namespace vex;

// Brain should be defined by default
brain Brain;


// START IQ MACROS
#define waitUntil(condition)                                                   \
  do {                                                                         \
    wait(5, msec);                                                             \
  } while (!(condition))

#define repeat(iterations)                                                     \
  for (int iterator = 0; iterator < iterations; iterator++)
// END IQ MACROS


// Robot configuration code.
motor ChIzq = motor(PORT1, false);
motor ChDer = motor(PORT2, true);
motor TIzq = motor(PORT3, false);
motor TDer = motor(PORT4, true);
motor Rod = motor(PORT5, false);

#pragma endregion VEXcode Generated Robot Configuration

//----------------------------------------------------------------------------
//                                                                            
//    Module:       main.cpp                                                  
//    Author:       {author}                                                  
//    Created:      {date}                                                    
//    Description:  IQ project                                                
//                                                                            
//----------------------------------------------------------------------------

// Include the IQ Library
#include "iq_cpp.h"
#include </Users/fgonzalezv006/personal/autonomo/RobotRodillo.iqcpp>

// Allows for easier use of the VEX Library
using namespace vex;

int main() {
  RobotRodillo(ChIzq, ChDer, TIzq, TDer, Rod);  

  useChasis(forward, 100, 100, true, 1000); // Avanza por un segundo y se detiene
  useTorres(forward, 100, true, 700); // Sube las torres por 700 milisegundos
  useRodillo(forward, 100, true, 2000); // Gira el rodillo por 2 segundos

  useTorres(reverse, 100, true, 700); // Bajamos las torres por 700 milisegundos
  useRodillo(reverse, 100); // Giramos el rodillo indefinidamente
  useChasis(forward, 60, 60, true, 2000); // Avanza el robot por dos segundos
  useRodillo(forward, 0, true); // Detiene el rodillo

  useChasis(forward, 100, -100, true, 1000); // Gira el robot 1 segundo y se detiene
  useChasis(forward, 100, 100); // Avanza el robot indefinidamente
  useTorres(forward, 100, true, 700); // Sube las torres por 700 milisegundos
  useChasis(forward, 100, 100, true, 3000); // Avanza el robot 2 segundos más y se detiene
  useRodillo(forward, 100, true, 3000); // Giramos el rodillo 3 segundos y se detiene
}
```

---
layout: post
title: 'Mac Flooding Attack, ¿Que es y como se acontece?'
date: 2023-07-25
categories: redes 
tags: redes lan hacking
cover: '/assets/img/mac-flooding-attack.jpg'
---

# ADVERTENCIA

<img src="/assets/img/advertencia2.png" alt="advertencia" width="500" height="400">

**El siguente contenido no promueve el cibercrimen ni la delincuencia atraves de las redes, se ha creado con el unico proposito de enseñar acerca de la ciberseguridad de manera ofensiva y de forma etica. El autor de este blog no hace responsable por el mal uso que le den a la información, debido a que el conocimiento es libre y cada quien se hace responsable de si mismo. Una vez entendido esto puedes continuar leyendo.** 




# ¿Que es Mac Flooding Attack?

MAC Flooding Attack o ataque de inundación MAC es un tipo de ataque cibernético dirigido a redes locales con el objetivo de comprometer su seguridad. En este tipo de ataque, el perpetrador busca saturar la tabla CAM (Content Addressable Memory) presente en los switches de la red. Esta tabla es responsable de almacenar las asociaciones entre las direcciones MAC de los dispositivos conectados y los puertos físicos a los que están vinculados.

El atacante inunda la red con un gran volumen de tráfico conteniendo direcciones MAC falsas, lo que lleva a la tabla CAM a alcanzar su capacidad máxima. Como resultado, cuando un nuevo paquete de datos llega al switch y no se encuentra espacio disponible en la tabla CAM para almacenar su asociación de dirección MAC-puerto, el switch se ve en la obligación de tomar una acción por defecto. Esta acción por defecto implica enviar el tráfico recibido a todos los puertos de la red, convirtiendo la red en un dominio de broadcast. En un dominio de broadcast, el tráfico es retransmitido a todos los dispositivos conectados, sin importar si el paquete está destinado a un dispositivo específico o no.


## ¿Que es la tabla CAM?

En el contexto de las redes informáticas, la tabla CAM (**Content Addressable Memory**) es un componente vital presente en los switches, que desempeña un papel fundamental en el proceso de reenvío de datos en una red local. Su función principal es almacenar las asociaciones entre las direcciones MAC (**Media Access Control**) de los dispositivos conectados a la red y los puertos físicos a los que están conectados.

Cuando un paquete de datos llega a un switch, el switch utiliza la dirección MAC de destino contenida en el paquete para buscar en su tabla CAM la correspondiente asociación entre esa dirección y el puerto de salida al que debe enviar el paquete. Esta búsqueda se realiza de manera muy rápida gracias a la naturaleza especializada de la memoria CAM, que permite acceder directamente a la dirección buscada sin tener que recorrer toda la tabla.

La tabla CAM se va llenando a medida que los dispositivos se conectan y se comunican en la red. Una vez que la tabla está completa, el switch tiene la capacidad de dirigir eficientemente los paquetes de datos solo al puerto específico donde se encuentra el dispositivo de destino, lo que reduce el tráfico innecesario y mejora el rendimiento de la red.

![Tabla-cam](/assets/img/tabla-cam.jpg)

## ¿Que es el dominio broadcast?

El término "dominio broadcast" se refiere a una situación particular en la que los paquetes de datos transmitidos en una red son reenviados a todos los dispositivos conectados a ella. Es decir, cuando un dispositivo envía un mensaje en modo broadcast, todos los otros dispositivos dentro de esa misma red local lo recibirán, independientemente de si el mensaje es relevante para ellos o no.

# ¿Como se hace el MAC Flooding Attack?

El MAC Flooding Attack se hace desde un equipo GNU/Linux, debido a que GNU/Linux tiene mayor facilidad para realizar ataques de este tipo o incluso otro tipos de ataques. Una vez en el equipo GNU/Linux descargaremos con nuestro gestor de paquetes el programa "**dsniff**" y atacaremos a switch con la utilidad "**macof**". Seria de la siguente forma:

Para instalar el programa pondremos en nuestra terminal

```bash
sudo apt install dsniff

```

Una vez instalado "**dsniff**" Tendriamos la utilidad que se utilizaria de la siguente forma

```bash
sudo macof -i <interfaz-de-red> -n <cantidad-de-MACs>

```

Por ejemplo

```bash
sudo macof -i enp1s0 - 2000

```

El ataque se realiza en cuestion de segundos, y una vez hecho el ataque la red quedaria con dominio **broadcast** y todo el trafico se dispersaria por toda la red. Solo seria cuestion de utilizar un sniffer como **WireShark** o cualquier otro para poder hacer sniffing y ver que estan haciendo los demas usuarios en la red.

# ¿Como se mitiga esto?

Para evitar un MAC Flooding Attack se debe utilizar **PORT-SEGURITY** PORT-SEGURITY es una medida que traen los switches que permite controlar este tipo de ataque poniendo politicas de seguridad. Podemos ser estrictos con esas politicas de seguridad de tal punto que en caso de recibir este ataque el switch apague esta interfaz.

## Configuracion para PORT-SEGURITY


```
interface Gi0/0
switchport mode acess
switchport port-segurity maximum 2
switchport violation shutdown
```

### En caso de que no funcione port-segurity ejecutamos:

```
switchport mode host
```

## Distintas restricciones en caso de abuso a la red:


| RESTRICCION | EXPLICACION |
|-------------|-------------|
| shutdown    | La interfaz se desactiva (err-disable) cuando hay un abuso de seguridad y se notifica (Default) |
| restrict    | Las direcciones MAC que no cumplan la politica son dropeadas y se notifica.   |
| protect     | Las direcciones MAC que no cumplan la politica son dropeadas y no se notifica. |

# Conclusión

En conclusión, el ataque de MAC flooding y la presencia de dominios broadcast representan dos aspectos críticos a considerar en la seguridad de las redes locales. La saturación de la tabla CAM en switches puede abrir la puerta a posibles intrusiones y vulnerar la confidencialidad de los datos. Al mismo tiempo, un uso excesivo del dominio broadcast puede afectar negativamente el rendimiento y la eficiencia de la red. Para mantener la integridad y proteger la información sensible, es fundamental implementar medidas de seguridad proactivas y controles adecuados en nuestras infraestructuras de red. Con una combinación de concienciación, mejores prácticas y soluciones de mitigación, podemos enfrentar estos desafíos y fortalecer nuestras redes contra las amenazas cibernéticas actuales. En caso de que te haya gustado sigueme por favor en instagram, estare pendiente de sus comentarios alla, Muchas gracias por leer.

Instagram: [https://instagram.com/j4f3th/](https://instagram.com/j4f3th/)

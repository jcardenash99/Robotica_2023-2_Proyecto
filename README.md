
# Robotica_2023-2_Proyecto
## Integrantes

- Wilfer Armando Fiquitiva Mendez.
- Johan Leonardo Castellanos Ruiz.
- Juan Pablo Cardenas Higuera.

## Aplicación
Realizar el despacho de un pedido de 3 items organizados al azar en una repisa de 3 filas y 2 columnas, considerando
que cualquier objeto podría estar en cualquier espacio.
Para considerar varios tipos de geometría, el manipulador debe poder tomar cajas, tarros y tubos de crema de la
repisa.

## DESARROLLO
## Diseño de griper.
Para el desarrollo de la aplicación iniciamos determinando las geometrías y dimensiones de los objetos que vamos a manipular, con base a esto y haciendo una revisión de los actuadores disponibles en el laboratorio definimos alternativas de solución al diseño del griper más adecuado para la manipulación de los objetos.

[Cilindro neumático].![Cilindro neumatico](https://github.com/jcardenash99/Robotica_2023-2_Proyecto/assets/143892609/96a0d4c6-70ed-4c84-aaaa-deb1d579d2e2)

Identificando los actuadores disponibles encontramos un cilindro neumático de doble efecto el cual nos permite hacer un desplazamiento lineal por ende pensamos en hacerlo parte del sistema de cierre del griper.
Con base a esto se plantea un diseño que permita aprovechar el recorrido total del vástago (50 mm) como apertura total del griper, se realiza el modelado mediante el software solidworks de una mordaza fija y una mordaza móvil que mediante el desplazamiento del vástago bien sea hacia adentro o hacia afuera permita la apertura del griper.

[Modelado mordaza fija].![Mordaza fija](https://github.com/jcardenash99/Robotica_2023-2_Proyecto/assets/143892609/ddd506c8-f69f-4df9-b5bb-150d93bdf28b)

[Mordaza movil].![Mordaza movil](https://github.com/jcardenash99/Robotica_2023-2_Proyecto/assets/143892609/ae269348-f2be-4948-9f59-d09fdcafe0aa)

Para permitir el deslizamiento de la mordaza móvil esta desliza sobre una guía lineal de 5/16”

[Guia lineal].![Guia lineal](https://github.com/jcardenash99/Robotica_2023-2_Proyecto/assets/143892609/7ff12eed-9446-4507-aeb2-b401a5ecb8ed)

Para el montaje del cilindro en el gripper se coloca un soporte, en el cual la sujeción es mediante una tuerca aprovechando el extremo roscado del cilindro, el vástago del cilindro es acoplado mediante una brida a un esparrago roscado que conecta con la mordaza fija; y funciona como elemento transmisor del movimiento lineal entre el cilindro y la mordaza fija causando un efecto de apertura y cierre del gripper. 

[Sitema desplazamiento lineal].![Montaje sistema lineal](https://github.com/jcardenash99/Robotica_2023-2_Proyecto/assets/143892609/b0fadd8d-b5ed-4a1a-ad1e-635305627ee2)

Finalmente, en la parte superior del gripper se coloca un acople cilíndrico con brida en uno de sus extremos que tiene 4 agujeros de amarre, el cual se encarga de acoplar el gripper en general a la muñeca del brazo robótico. Quedando completo el modelado de cada parte del gripper, posterior a ello se hace el ensamble y se revisa su funcionamiento.

[Modelado ensamble gripper].![Ensamble Gripper](https://github.com/jcardenash99/Robotica_2023-2_Proyecto/assets/143892609/35a65497-31df-4701-af4a-80de20e51cbf)

Una vez modelado el gripper se decide fabricar cada pieza en acrílico, mediante corte laser empleando los modelos realizados en solidworks, una vez cortadas las piezas se hacen unas operaciones de taladrado y roscado para el respectivo ensamble de las piezas mediante tornillos como elementos de sujeción. 
Finalmente, al ensamblar el gripper se colocan unos recubrimientos de caucho en las mordazas con el fin de que permita tener mayor sujeción entre la mordaza y las piezas a ser manipuladas por el brazo robótico.


[Gripper final].![Gripper 1](https://github.com/jcardenash99/Robotica_2023-2_Proyecto/assets/143892609/34e79934-07d5-40fa-97d0-df7b2a4075b8)

[Gripper final].![Gripper 2](https://github.com/jcardenash99/Robotica_2023-2_Proyecto/assets/143892609/1fe344f2-9c2b-4cdb-878e-6054e6e24031)




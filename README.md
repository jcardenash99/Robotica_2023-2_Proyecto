
# Robotica_2023-2_Proyecto
## Integrantes

- Wilfer Armando Fiquitiva Mendez.
- Johan Leonardo Castellanos Ruiz.
- Juan Pablo Cardenas Higuera.

## Aplicación
Realizar el despacho de un pedido de 3 ítems organizados al azar en una repisa de 3 filas y 2 columnas, considerando
que cualquier objeto podría estar en cualquier espacio.
Para considerar varios tipos de geometría, el manipulador debe poder tomar cajas, tarros y tubos de crema de la
repisa.

### Repisa
Será un estante metálico de 4 secciones de una altura de 1,16 metros y 0,6 metros de ancho. La altura de las secciones es de 35 centímetros y en estas se colocarán es su respecto orden los elementos con los que se van a trabajar.

![](Captura%20estante.PNG)

## DESARROLLO
## Diseño de gripper.
Para el desarrollo de la aplicación iniciamos determinando las geometrías y dimensiones de los objetos que vamos a manipular, con base a esto y haciendo una revisión de los actuadores disponibles en el laboratorio definimos alternativas de solución al diseño del gripper más adecuado para la manipulación de los objetos.

[Cilindro neumático].![Cilindro neumatico](https://github.com/jcardenash99/Robotica_2023-2_Proyecto/assets/143892609/96a0d4c6-70ed-4c84-aaaa-deb1d579d2e2)

Identificando los actuadores disponibles encontramos un cilindro neumático de doble efecto el cual nos permite hacer un desplazamiento lineal por ende pensamos en hacerlo parte del sistema de cierre del gripper.
Con base a esto se plantea un diseño que permita aprovechar el recorrido total del vástago (50 mm) como apertura total del gripper, se realiza el modelado mediante el software solidworks de una mordaza fija y una mordaza móvil que mediante el desplazamiento del vástago bien sea hacia adentro o hacia afuera permita la apertura del gripper.

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

## MONTAJE

Al tener ensamblado el gripper, este se acoplará al manipulador ABB de la siguiente manera para aplicar las rutinas de movimiento.

![](griper%20montado.jpg)

## Diseño de trayectorias y Rutinas
### Diagramas de flujo
A continuación, se puede encontrar el diseño lógico del funcionamiento del código donde para empezar debe realizar el posicionamiento en home con la función Homming(), calibración de herramienta con la función Soltar() y establecer la posición de trabajo donde se evitan singularidades, Posición_inicial().
Comienza la iteración que desactiva la orden de finalización siempre y cuanto se encuentre en FALSO. Esta permitirá realizar la selección entre los 3 objetos mediante las etiquetas A1, A2 y A3, preguntando si cada una fue la que se seleccionó. Una vez determine que cualquiera de las etiquetas de objeto fue la seleccionada, procede a llamar la rutina correspondiente a coger el objeto (Coger_cilindro(), Coger_prisma() o Coge_cil_acostado()).

![Diagrama 1](https://github.com/jcardenash99/Robotica_2023-2_Proyecto/assets/63479570/3e51be62-97df-499a-9a34-3776c1f15c36)

Una vez se tenga seleccionado el objeto se llamará la función 'agarrar;' y preguntará si hay espacio en la banda para colocar el objeto desde la estantería. Si hay espacio, se procede a realizar la rutina DejarEnBanda() y posteriormente mediante la función 'soltar;', dejar el objeto en la banda. Si no hay espacio, únicamente se soltará el objeto con la función 'soltar;' en el espacio en la estantería donde se encontraba

![Diagrama 2](https://github.com/jcardenash99/Robotica_2023-2_Proyecto/assets/63479570/7e63c629-39ed-4699-a59f-0898c104f6e8)


### Trayectorias
Para el diseño de las trayectorias se partió del modelado de la estantería, y se ubicó en una posición frente al robot, esta estantería se convirtió en work object sobre el cual se crearían los primeros puntos los cuales dependen del work object la posición final del work object frente al mundo es de: [500,-300,540] esta distancia en mm, los puntos iniciales de los objetos respecto al work object fueron 

Cilindro:[130,368,15]mm

Prisma:[173,221,345]mm

Cilindro recostado: [148,170,10]mm

Para que el robot llegase a estos puntos y el gripper diseñado no entrara en colisión con el objeto se creó un punto de acercamiento el cual solo presenta un offset de altura en z respecto al punto del objeto los cuales fueron:

Acercamiento cilindro:[130,368,75]mm

Acercamiento Prisma:[173,221,390]mm

Acercamiento Cilindro Recostado:[148,170,84]mm

Para poder lograr que el robot partiese de una posición en el espacio no ocupado por la caja que envuelve la estantería, y entrase en el espacio de la estantería si colisionar con esta fue necesario crear 3 puntos que se encontrase frente al punto de acercamiento de cada objeto, pero sin entrar en el espacio de la estantería, para que con esto se realizase un movimiento lineal de ingreso a la estantería estos puntos fueron:

Acercamiento al Work object en posición Cilindro:[-70,368,75]

Acercamiento al Work object en posición Prisma:[-66,221,390]

Acercamiento al Work object en posición Cilindro recostado:[-58,186,182]

Como podemos observar para la mayoría de las posiciones tenemos que estas difieren en máximo una distancia con respecto a un eje, una rutina que llego a dar problemas fue la rutina de tomar el cilindro recostado, esta rutina al pasar por sus punto tenía una configuración errónea, por lo que fue necesario definir esta rutina en términos de variables articulares, lo mismo sucedió con la rutina que pasa los objetos a la banda, esta rutina parte de las variables articulares por sus pasos por cero y giros muy grandes.

Para la definición de las trayectorias tenemos que el robot debe pasar por los puntos previamente tomados, en un orden especifico, para el caso de tomar un objeto la rutina pasa por los puntos de acercamiento al work object, luego acercamiento al objeto, después tenemos el objeto en posición, se cierra el gripper y retomamos los puntos de forma inversa, vamos al acercamiento al objeto, luego acercamiento al work object y terminamos con la posición inicial. 
Este es el orden que rige el conjunto de las instrucciones de desplazamiento del objeto, las cuales son:

- Tomar objeto 1 al 3 conocidos como cilindro, prisma y cilindro recostado.
- Poner objeto en banda en las posiciones C1 a la C3 de izquierda a derecha vistas desde el robot.

Estas rutinas componen lo que es en esencia las rutinas de trayectorias.

### Rutinas
En las rutinas las cuales no componen una trayectoria de desplazamiento de objetos tenemos un conjunto variado, desde las rutinas que controlan las salidas digitales que encienden las bobinas que abren y cierran el gripper, también conocidas como:

- Agarrar();
- Soltar();

Estas rutinas constan de encender y apagar las bobinas de forma ordenada, el concepto de estas rutinas es primero apagar las 2 bobinas, luego encender la deseada ya sea la que abre o cierra el gripper y se finaliza con volver a apagar las bobinas, en este caso de organización de objetos pequeños podemos apagar las bobinas para limitar su consumo sin el problema que el peso del objeto abra el gripper.

Las rutinas de Homming y posicionamiento inicial que como su nombre lo indica son rutinas que se encarga específicamente de llevar el robot a la posición de home o a la posición inicial que es una configuración de las juntas con la intención de salir de la singularidad inicial.

La de posición es una rutina en la que se pregunta si las variables C1 a la C3 están llenas, si lo están suelta el objeto, si están vacías lleva a el objeto a la posición en banda que se encuentra vacía.

La rutina comprobar espacio es una rutina auxiliar a la de posición en la cual si los espacios en banda están llenos suelta el objeto en la posición que se tomó, en caso contrario si hay espacio vacío no hace nada y salta a la rutina posición para que lleve el objeto al espacio vacío.

Finalmente en la rutina Main tenemos las configuraciones iniciales las cuales inician un valor a las variables persistentes booleanas, luego se asegura que el gripper se encuentre abierto con la rutina soltar, después enviar al robot a Home por si el robot se encuentra en alguna posición ajena al home y a la posición inicial, después de estar en home se envía a la posición inicial que rompe con la singularidad, posterior entra en el bucle principal while que mientras la variable finalizar asociada a este se encuentre desactivada el while se mantiene y mantiene al robot preguntando que objeto desea tomar.

A continuación, se muestra el Código en RAPID del Modulo1 en donde se encuentra en general todo, desde las rutinas las constantes de posición lineal y de juntas y las variables booleanas.



```MOD
MODULE Module1
        PERS tooldata Gripper1TCP:=[TRUE,[[11.5,-9.419,142.496],[0.707106781,0,-0.707106781,0]],[0.347,[-5.649,5.049,83.176],[1,0,0,0],0,0,0]];
    TASK PERS wobjdata Estanteria:=[FALSE,TRUE,"",[[0,0,0],[1,0,0,0]],[[500,-300,540],[1,0,0,0]]];
    CONST jointtarget Romper_singularidad:=[[3,-35,18,19,30,2],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Acercamiento_wobj_cilindro:=[[-70.611,368.952,75],[1,0,0,0],[0,-2,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Acecamiento_cilindro:=[[129.389,368.952,75],[1,0,0,0],[0,-2,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Agarre_cilindro:=[[129.389,368.952,15],[1,0,0,0],[0,-2,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Acercamiento_wobj_prisma:=[[-66.748,221.465,390.002],[1,0,0,0],[0,0,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Acercamiento_Prisma:=[[173.252,221.465,390.002],[1,0,0,0],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Agarre_prisma:=[[173.252,221.465,345.002],[1,0,0,0],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Acercamiento_wob_cil_ac:=[[-58.349830876,186.942,182.638559728],[1,0,0,0],[-1,0,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    
    CONST jointtarget Acercamiento_cilindro_acostado:=[[-15,-5.21,16.65,-53,-19,51.34],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST jointtarget Agarre_cilindro_acostado:=[[-15,5.9,17.3,-33.9,-26.7,30.6],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    
    !CONST robtarget Acercamiento_cilindro_acostado:=[[68.92938971,186.942,83.643610326],[1,0,0,0],[0,0,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    !CONST robtarget Agarre_cilindro_acostado:=[[104.28472876,186.942,48.288271256],[1,0,0,0],[0,0,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST jointtarget Home:=[[0,0,0,0,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget banda_acercar:=[[-3.9,-677.8,780.1],[0.975774,0.175182,0.116858,0.0593263],[0,0,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget dejar_banda:=[[-3.9,-677.8,576.5],[0.975774,0.175186,0.116858,0.0593273],[0,0,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
        PERS wobjdata world1:=[FALSE,TRUE,"",[[0,0,0],[1,0,0,0]],[[0,0,0],[1,0,0,0]]];
        CONST jointtarget jt_dejar_banda:=[[-89.7,8.4,11.7,5.4,-24.3,-0.1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
        CONST jointtarget jt_acercar_banda:=[[-87.5,4.1,-11.2,71.6,-7.5,-66.8],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
        CONST jointtarget jt_dejar_bandacaja:=[[-75.1,11.1,9.1,35.9,-29.4,-28.5],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
        CONST jointtarget jt_dejar_bandacrema:=[[-108.8,9.5,13.7,-33.2,-31.9,35.3],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    
    !***********************************************************
    !
    ! Módulo:  Module1
    !
    ! Descripción:
    !   <Introduzca la descripción aquí>
    !
    ! Autores: Johan Castellanos - Juan Cardenas - Wilfer Fiquitiva 
    !
    ! Versión: 1.0
    !
    !***********************************************************
    !Declaración de variables VAR y PERS booleanas
    !***********************************************************
    PERS bool A1;
    PERS bool A2;
    PERS bool A3;
    PERS bool C1;
    PERS bool C2;
    PERS bool C3;
    PERS bool ledA1;
    PERS bool ledA2;
    PERS bool ledA3;
    PERS bool finalizar;
    PERS bool pinza_cerrada;
    PERS bool reiniciar_valores;
    !***********************************************************
    !main
!***********************************************************
    PROC main()

        C1:=FALSE;
        C2:=FALSE;
        C3:=FALSE;
        A1:=FALSE;
        A2:=FALSE;
        A3:=FALSE;
        ledA1:=FALSE;
        ledA2:=FALSE;
        ledA3:=FALSE;
        finalizar:=FALSE;
        pinza_cerrada:=TRUE;
        reiniciar_valores:=FALSE;
        Homming;
        soltar;
        Posicion_inicial;
        WHILE finalizar=FALSE DO
            
            IF A1 = TRUE THEN
                ledA1:=TRUE;
                Coger_cilindro;
                Posicion;
                ledA1:=false;
            ENDIF
            IF A2 = TRUE THEN
                ledA2:=TRUE;
                Coger_prisma;
                Posicion;
                ledA2:=false;
            ENDIF
            IF A3 = TRUE THEN
                ledA3:=TRUE;
                Coger_cil_acost;
                Posicion;
                ledA3:=false;
            ENDIF
            IF reiniciar_valores = TRUE THEN
                C1:=FALSE;
                C2:=FALSE;
                C3:=FALSE;
                A1:=FALSE;
                A2:=FALSE;
                A3:=FALSE;
                ledA1:=FALSE;
                ledA2:=FALSE;
                ledA3:=FALSE;
                finalizar:=FALSE;
                pinza_cerrada:=TRUE;
                soltar;
                reiniciar_valores:=FALSE;
            ENDIF
        ENDWHILE
        Homming;
    ENDPROC
    PROC Posicion()
        IF C1 = FALSE THEN
            dejar_en_bandacaja;
            soltar;
            C1:=TRUE;
        ELSEIF C2 = FALSE Then  
            dejar_en_banda;
            soltar;
            C2:=TRUE;
        ELSEIF C3 = FALSE THEN
            dejar_en_bandacrema;
            soltar;
            C3:=TRUE;
        ELSE
            soltar;
        endif
    ENDPROC
    PROC agarrar()
        SetDO DO_02,1;
        WaitTime 0.5;
        SetDO DO_02,0;
        WaitTime 0.5;
        SetDO DO_01,1;
        WaitTime 0.5;
        SetDO DO_01,0;
        WaitTime 0.5;
        pinza_cerrada:=TRUE;
    ENDPROC
    
    PROC soltar()
        SetDO DO_01,0;
        WaitTime 0.5;
        SetDO DO_02,1;
        WaitTime 0.5;
        SetDO DO_02,0;
        WaitTime 0.5;
        pinza_cerrada:=FALSE;
    ENDPROC
    
    PROC Posicion_inicial()
        MoveAbsJ Romper_singularidad,v1000,z100,Gripper1TCP\WObj:=Estanteria;
    ENDPROC
    PROC comprobar_espacio()
        IF C1= TRUE THEN
            IF C2= TRUE THEN
                IF C3= TRUE THEN
                    soltar;
                ENDIF
            ENDIF
        ENDIF
    endproc
    PROC Coger_cilindro()
        MoveJ Acercamiento_wobj_cilindro,v300,z10,Gripper1TCP\WObj:=Estanteria;
        MoveL Acecamiento_cilindro,v300,z10,Gripper1TCP\WObj:=Estanteria;
        MoveL Agarre_cilindro,v300,fine,Gripper1TCP\WObj:=Estanteria;
        
        agarrar;
        comprobar_espacio;
        MoveL Acecamiento_cilindro,v300,z10,Gripper1TCP\WObj:=Estanteria;
        MoveJ Acercamiento_wobj_cilindro,v300,z10,Gripper1TCP\WObj:=Estanteria;
        Posicion_inicial;
        
    ENDPROC
    PROC Coger_prisma()
        MoveJ Acercamiento_wobj_prisma,v300,z10,Gripper1TCP\WObj:=Estanteria;
        MoveL Acercamiento_Prisma,v300,fine,Gripper1TCP\WObj:=Estanteria;
        MoveL Agarre_prisma,v300,z10,Gripper1TCP\WObj:=Estanteria;
        
        agarrar;
        comprobar_espacio;
        
        MoveL Acercamiento_Prisma,v300,z10,Gripper1TCP\WObj:=Estanteria;
        MoveJ Acercamiento_wobj_prisma,v300,z10,Gripper1TCP\WObj:=Estanteria;
        Posicion_inicial;
    ENDPROC
    PROC Coger_cil_acost()
        MoveJ Acercamiento_wob_cil_ac,v300,z10,Gripper1TCP\WObj:=Estanteria;
        MoveabsJ Acercamiento_cilindro_acostado,v300,fine,Gripper1TCP\WObj:=Estanteria;
        MoveabsJ Agarre_cilindro_acostado,v300,fine,Gripper1TCP\WObj:=Estanteria;
        
        agarrar;
        comprobar_espacio;
        
        MoveabsJ Acercamiento_cilindro_acostado,v300,fine,Gripper1TCP\WObj:=Estanteria;
        MoveJ Acercamiento_wob_cil_ac,v300,z10,Gripper1TCP\WObj:=Estanteria;
        Posicion_inicial;
    ENDPROC
    PROC Homming()
        MoveAbsJ Romper_singularidad,v1000,z100,Gripper1TCP\WObj:=Estanteria;
        MoveAbsJ Home,v1000,z100,Gripper1TCP\WObj:=Estanteria;
    ENDPROC
	PROC dejar_en_banda()
		MoveAbsJ Romper_singularidad\NoEOffs, v300, z50, Gripper1TCP\WObj:=Estanteria;
		MoveAbsJ jt_acercar_banda\NoEOffs, v300, fine, Gripper1TCP\WObj:=world1;
        MoveAbsJ jt_dejar_banda\NoEOffs, v300, fine, Gripper1TCP\WObj:=world1;
        
        soltar;
        
        MoveAbsJ jt_acercar_banda\NoEOffs, v300, fine, Gripper1TCP\WObj:=world1;
		MoveAbsJ Romper_singularidad\NoEOffs, v300, z50, Gripper1TCP\WObj:=Estanteria;
	ENDPROC
	PROC dejar_en_bandacaja()
		MoveAbsJ Romper_singularidad\NoEOffs, v300, z50, Gripper1TCP\WObj:=Estanteria;
		MoveAbsJ jt_acercar_banda\NoEOffs, v300, fine, Gripper1TCP\WObj:=world1;
        MoveAbsJ jt_dejar_bandacaja\NoEOffs, v300, fine, Gripper1TCP\WObj:=world1;
        
        soltar;
        
        MoveAbsJ jt_acercar_banda\NoEOffs, v300, fine, Gripper1TCP\WObj:=world1;
		MoveAbsJ Romper_singularidad\NoEOffs, v300, z50, Gripper1TCP\WObj:=Estanteria;
	ENDPROC
	PROC dejar_en_bandacrema()
		MoveAbsJ Romper_singularidad\NoEOffs, v300, z50, Gripper1TCP\WObj:=Estanteria;
		MoveAbsJ jt_acercar_banda\NoEOffs, v300, fine, Gripper1TCP\WObj:=world1;
        MoveAbsJ jt_dejar_bandacrema\NoEOffs, v300, fine, Gripper1TCP\WObj:=world1;
        
        soltar;
        
        MoveAbsJ jt_acercar_banda\NoEOffs, v300, fine, Gripper1TCP\WObj:=world1;
		MoveAbsJ Romper_singularidad\NoEOffs, v300, z50, Gripper1TCP\WObj:=Estanteria;
	ENDPROC
ENDMODULE
```
## Diseño de la interfaz de usuario
En el diseño de la interfaz de usuarion primero vamos a hablar de las variables booleanas persistentes en el Código en rapid ya que estas son las que la interfaz edita para que el robot realice las rutinas deseadas.

```MOD
! Variables de rutina de tomar objeto
	PERS bool A1;
	PERS bool A2;
	PERS bool A3;
! Variables de espacio en banda
	PERS bool C1;
	PERS bool C2;
	PERS bool C3;
! Variables de led indicador de objeto agarrado
	PERS bool ledA1;
	PERS bool ledA2;
	PERS bool ledA3;
! Variables auxiliares para finalizar el ciclo while, para reiniciar las variables por defecto y la del led que indica si la pinza está cerrada
	PERS bool finalizar;
	PERS bool pinza_cerrada;
	PERS bool reiniciar_valores;
```

La interfaz diseñada es una interfaz simple de una solo ventana donde se encuentran los botones y los leds indicadores, esta interfaz se muestra a continuación:

![Interfaz simple](https://github.com/jcardenash99/Robotica_2023-2_Proyecto/assets/61796945/f5d5ee6f-3db3-4652-b1da-9a39383fd288)

Como podeos observar en la interfaz teneos 5 botones los cuales son:

- Tomar posición 1: asociado a la variable A1 la cual ejecuta la rutina de agarrar cilindro
- Tomar posición 2: asociado a la variable A2 la cual ejecuta la rutina de agarrar prisma
- Tomar posición 3: asociado a la variable A3 la cual ejecuta la rutina de agarrar cilindro recostado
- Reset Memory: el cual se asocia a la variable reiniciar valores la cual pone el valor por defecto a las variables booleanas
- Salir: el cual es el botón asociado a la variable finalizar la cual rompe el ciclo while y permite al Código continuar y finalizar con normalidad.

En la ventana también se observan 7 leds los cuales son:

- Leds 1-3 ubicados al lado del botón de Tomar Posición correspondiente, estos leds leen las variables LedA1 a LedA3, estas variables se activan durante la ejecución de la rutina de tomar objeto y se apaga cuando el robot regrese a la posición inicial al final de la rutina.
- Led 1-3 en la sección de espacios llenos, estos leds están ligados a las variables C1- C3, las cuales se activan cuando el robot fue a llevar un objeto a esa posición asociada a la banda, estos leds se mantienen activos hasta que se reinicien sus valores con el botón Reset o hasta que se reinicie el programa del robot.
- Led pinza cerrada este led se encuentra asociado a la variable pinza cerrada y lee le valor de esta variable, esta variable es un poco más compleja dado a que la pinza tiene 4 opciones posibles una por cada combinación de bobina:
  	* DO_01 =1 y DO_02 =0: gripper cerrado.
  	* DO_01 =0 y DO_02 =1: gripper abierto.
  	* DO_01 =1 y DO_02 =1: imposible determinar depende de varios factores asociados a la configuración de la válvula de 2 vías.
  	* DO_01 =0 y DO_02 =0: imposible determinar depende de la última configuración de la válvula de 2 vías ya sea que el gripper estuviese abierto o cerrado
Como normalmente nos vemos en el último caso lo que se hizo con a variable pinza cerrada fue activarla por defecto desactivarla cuando la rutina soltar se ejecute y volverla a activar cuando la rutina agarrar se ejecute, con esto y en base a la disposición del Código se puede garantizar que mientras se ejecuten los movimientos desde la interfaz este led indique si el gripper se encuentre abierto o cerrado conforme a la última instrucción de abierto o cerrado enviada.


## Video Presentación
El video puede encontrarse a continuación ya que hubo restricción de subida directa en el repositorio.

ENLACE VIDEO: https://www.youtube.com/watch?v=4hn36XYPoaw


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

### Repisa
Será un estante metálico de 4 secciones de una altura de 1,16 metros y 0,6 metros de ancho. La altura de las secciones es de 35 centimetros y en estas se colocarán es su respectvo orden los elementos con los que se van a trabajar.

![](Captura%20estante.PNG)

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

## DIAGRAMA DE FLUJO
## MONTAJE

Al tener ensamblado el griper, este se coplará al manipulador ABB de la siguiente manera para aplicar las rutinas de movimiento.

![](griper%20montado.jpg)

## Diseño de trayectorias y Rutinas
### Trayectorias
Para el diseño de las trayectorias se partio del modelado de la estanteria, y se ubico en una posicion frente al robot, esta estanteria se convirtio en work object sobre el cual se crearian los primeros puntos los cuales dependen del work object la posicion final del work object frente al mundo es de: [500,-300,540] esta distancia en mm, los puntos iniciales del los objetos respecto al work object fueron 

Cilindro:[130,368,15]mm

Prisma:[173,221,345]mm

Cilindro recostado: [148,170,10]mm

Para que el robot llegase a estos puntos y el gripper diseñado no entrara en colision con el objeto se creo un punto de acercamiento el cual solo presenta un offset de altura en z respecto al punto del objeto los cuales fueron:

Acercamiento cilindro:[130,368,75]mm

Acercamiento Prisma:[173,221,390]mm

Acercamiento Cilindro Recostado:[148,170,84]mm

Para poder lograr que el robot partiese de una posicion en el espacio no ocupado por la caja que envuelve la estanteria, y entrase en el espacio de la estanteria si colisionar con esta fue necesario crear 3 puntos que se encontrase frente al punto de acercamiento de cada objeto pero sin entrar en el espacio de la estanteria, para que con esto se realizace un movimiento lineal de ingreso a la estanteria estos puntos fueron:

Acercamiento al Work object en posicion Cilindro:[-70,368,75]

Acercamiento al Work object en posicion Prisma:[-66,221,390]

Acercamiento al Work object en posicion Cilindro recostado:[-58,186,182]

Como podemos observar para la mayoria de las posciones tenemos que estas difieren en maximo una distancia con respecto a un eje, una rutina que llego a dar problemas fue la rutina de tomar el cilindro recostado, esta rutina al pasar pos sus punto tenia una configuracion erronea, por lo que fue necesario definir esta rutina en terminos de variables articulares, lo mismo sucedio con la rutina que pasa los objetos a la banda, esta rutina parte de las variables articulares por sus pasos por cero y giros muy grandes.

Para la definicion de las trayectorias tenemos que el robot debe pasar por los puntos previamente tomados, en un orden especifico, para el caso de tomar un objeto la rutina pasa por los puntos de acercamiento al wwork object, luego acercamiento al objeto, despues tenemos el objeto en posicion, se cierra el gripper y retomamos los puntos de forma inversa, vamos al acercamiento al objeto, luego acercamiento al work object y terminamos con la posicion inical. 
Este es el orden que rige el conjunto de las instrucciones de desplzamiento del objetos, las cuales son:

- Tomar objeto 1 al 3 conocidos como cilindro, prisma y cilindro recostado.
- Poner objeto en banda en las posiciones C1 a la C3 de izquierda a derecha vistas desde el robot.

Estas rutinas componen lo que es en escencia las rutinas de trayectorias.

### Rutinas
En las rutinas las cuales no componen una trayectoria de desplazamiento de ojetos tenemos un conjunto variado, desde las rutinas que controlan las salidas digitales que encienden las bobinas que abren y cierran el gripper, tambien conocidas como:

- Agarrar();
- Soltar();

Estas rutinas constan de encender y apagar la bobinas de forma ordenada, el concepto de estas rutinas es primero apagar las 2 bobinas, luego encender la deseada ya sea la que abre o cierra el gripper y se finaliza con volver a apagar las bobinas, en este caso de organizacion de objetos pequeños podemos apagar las bobinas para limitar su consumo sin el problema que el peso del objeto abra el gripper.

Las rutinas de homming y posicionamiento incial que como su nombre lo indica son rutinas que seencargan especificamente de llevar el robot a la posicion de home o a la posicion inicial que es una configuracion de las juntas con la intencion de salir de la sigularidad inicial.




```RAPID
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
    !Declaracion de variables VAR y PERS boleanas
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



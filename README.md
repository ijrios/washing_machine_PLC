# SISTEMAS DE SUPERVISIÓN - PRACTICA HMI Y SECUENCIA

## 1.	FUNDAMENTO TEÓRICO

Muchas de las aplicaciones de automatización requieren una interfaz con el usuario gráfica y necesitan realizar trabajo en una secuencia específica, para ello es muy común utilizar un sistema de control PLC-HMI.


## 2.	OBJETIVO(S)

Diseña e implementa Lógica secuencial con funciones en un PLC para un ejercicio propuesto por el docente.

Realiza interfaz en una HMI para el control del PLC


## 3.	RECURSOS REQUERIDOS

•	1 motor trifásico jaula de ardilla
•	4 Electro-válvulas 
•	Cables para control y potencia punta-banana, banana-banana y punta-punta.
•	Botonera y suiches según diseño
•	Relés según diseño con bobina 24Vdc
•	1 sensor inductivo
•	1 Fuente 24Vdc
•	Contactores según diseño
•	Variador De Velocidad
•	4 pilotos 24Vdc


## 4.	PROCEDIMIENTO O METODOLOGÍA PARA EL DESARROLLO

### 4.1.	Descripción del Requerimiento

1.	En una lavadora industrial (por ejemplo, en una tintorería) es fundamental contar con un control secuencial que garantice un funcionamiento correcto y ajustable a las diferentes telas que se necesiten. 

2.	Se requiere controlar por medio de un PLC la lavadora industrial por lotes que tiene un motor para mover la carga en dos direcciones, cuatro electroválvulas (Suministro de Agua Caliente, Suministro de Agua Fría, suministro de jabón líquido y desagüe), un sensor inductivo para garantizar que la puerta este correctamente cerrada, 4 sensores de nivel (Se simularán con suiches) (vacío, bajo, medio y alto) para determinar el nivel de agua en la lavadora.

3.	Los requerimientos son:

a.	Crear una interfaz gráfica con:
1.	Página de Inicio con los nombres de los integrantes
2.	Página de operación, donde se evidencie que programa esta seleccionado (Lana, Paño o Jean), los estados de las válvulas y del motor con su dirección, el estado de la máquina (En marcha, Pausa, Reposo o Alarma), la posición de la puerta y se pueda iniciar, pausar y detener.
3.	Una Página de Selección donde se pueda iniciar, pausar, detener y seleccionar el tipo de programa, el nivel deseado y de indique visualmente los ciclos que involucran las selecciones (Solo se puede cambiar el nivel o el tipo de programa si la máquina está detenida). Adicional se deben agregar 3 campos para ingresar la velocidad deseada del motor en cada tipo de tela y al igual que las demás selecciones no se podrá modificar si la máquina esta en marcha o pausa. (Se debe restringir el valor a ingresar entre 30% y 100%, siendo 100% 60Hz)
4.	Una página de Alarma donde se evidencie la causa de alarma (Fallo variador, puerta abierta mientras está en marcha la máquina, fallo en el suministro de agua u obstrucción en la válvula de desagüe (Ver lógica del PLC).
5.	Las variables involucradas en la pantalla deben estar en un bloque de datos 

b.	Las señales trabajan de la siguiente manera:
1.	El sensor inductivo entrega voltaje cuando la puerta está bien cerrada.
2.	Los sensores de nivel entregan voltaje cuando detectan líquidos a nivel o superior.
3.	El Variador entrega voltaje mientras que el motor esté en condiciones normales de operación.
4.	Las válvulas se simularán con pilotos y estas son normalmente cerradas (si no se les inyecta voltaje impiden el paso).
5.	El motor tiene 2 direcciones de dos salidas del PLC diferentes (Se debe implementar en variador para controlar la inversión de giro).

c.	El programa del PLC debe cumplir
1.	La máquina debe pausar su marcha si se presiona Pausa en el panel.
2.	Si se presiona Detener la máquina debe quedar en reposo (Esperando un start para empezar de cero un programa seleccionado) 
3.	La máquina debe pasar a Alarma si ocurre algo de lo siguiente y cuando se solucione la falla debe quedar en pausa a espera de start para continuar donde estaba:
•	Se detecta fallo en el Variador de Velocidad
•	Pasan 20 segundos después de activar cualquier válvula de agua y no se alcanza el nivel deseado.
•	Pasan 15 segundos después de activar la válvula de desagüe y no esta está vacío el nivel.
•	Se abre la puerta mientras está en marcha o pausa el sistema
4.	Después de seleccionar un nivel deseado (alto, medio o bajo), de seleccionar el tipo de tela (Lana, Paño o Jean) y de presionar Start no se podrá cambiar los parámetros ni reiniciar el ciclo hasta que la máquina quede en Reposo (termine el ciclo o se presione detener).

d.	Las secuencias de la lavadora son según la tela y el nivel:
1.	Lana: se llena con agua tibia (caliente y fría) hasta el nivel deseado, se activa hacia la derecha el motor durante 5 segundos, luego otros 5 hacia la izquierda, se desocupa el tanque y se activa por 6 segundos la válvula del jabón líquido, se llena nuevamente al nivel deseado, se activa hacia la derecha el motor durante 5 segundos, luego otros 5 hacia la izquierda, se desocupa el tanque, se llena nuevamente para enjuague, se desocupa y por último se activa el motor a la derecha 15 segundos para secar y termina.
2.	Paño: se llena con agua fría hasta el nivel deseado, se activa hacia la izquierda el motor durante 5 segundos, se desocupa el tanque y se activa por 4 segundos la válvula del jabón líquido, se llena nuevamente al nivel deseado, se activa hacia la derecha el motor durante 10 segundos, luego otros 5 hacia la izquierda, se desocupa el tanque se llena nuevamente para enjuague, se desocupa y por último se activa el motor a la izquierda 10 segundos para secar y termina.
3.	Jean: se llena con agua caliente hasta el nivel deseado, se activa hacia la izquierda el motor durante 5 segundos, luego se activa el motor hacia la derecha 5 segundos, se desocupa el tanque y se activa por 8 segundos la válvula del jabón líquido, se llena nuevamente al nivel deseado, se activa hacia la derecha el motor durante 10 segundos, luego otros 10 hacia la izquierda, se desocupa el tanque y se activa por 4 segundos la válvula del jabón líquido, se llena nuevamente al nivel deseado, se activa hacia la derecha el motor durante 5 segundos, luego otros 5 hacia la izquierda, se desocupa el tanque se llena nuevamente para enjuague, se desocupa y por último se activa el motor a la izquierda 20 segundos para secar y termina.

e.	El programa del PLC debe tener como mínimo estos bloques: 
1.	Bloque FC para manejo de Alarmas
2.	Bloque DB para variables HMI
3.	Bloque FC para manejo de Vbles HMI
4.	Bloque FC parametrizable para el control de las válvulas de entrada de agua así:
•	Entrada de agua
a.	Cerrar ambas cuando la entrada este en 0
b.	Abrir la válvula de Fría si la entrada es 1
c.	Abrir la válvula de Caliente si la entrada es 2
d.	Abrir la ambas Válvulas si la entrada es 3
•	Salidas
a.	Válvula agua caliente
b.	Válvula agua fría
5.	Se debe realizar en un bloque FC las señalizaciones de las velocidades deseadas y la salida análoga del PLC.
6.	Las velocidades deben quedar en 50% al encender el PLC..
7.	El variador debe mover el motor automáticamente a la velocidad que se indicó por pantalla y en la dirección correcta.
pleando el software Tia Portal mediante lenguaje de diagrama de bloques.

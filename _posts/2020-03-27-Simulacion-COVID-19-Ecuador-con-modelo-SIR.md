---
layout: post
title:  "Simulacion COVID-19 Ecuador modelo SIR"
date:   25-03-2020
categories: simulaciones
mathjax: true
---
$Preliminares$

Antes que nada, quisiera hacerles notar que no me he tomado la molestia de poner tildes en el texto ni simbolos propios del espanol. Mi problema no es con el lenguage, pero el teclado que tengo no se presta y la computadora tampoco me da la opcion de corregir porque estoy escribiendo esto en un editor bastante rudimentario. Es mas, la laptop que uso es una verdadera basura y su sistema operativo Windows es otra. No hacen caso. Me canse. Entonces, a los amantes de la ortografia, entre otros, me disculparan y ojala puedan soportar las tragicas y dolorosas faltas ortograficas.

Ahora bien, escribi este articulo porque, para serles franco, la situacion en la que estamos y la incertidumbre me tiene loco, y  me ha distraido hacerlo, ademas de satisfacer un par de curiosidades personales. Pero espero que, quien lea esto, tambien se distraiga algo, y quizas saque algo positivo. Por cierto, hay un poco de matematicas y, si no te interesa, puedes simplemente saltarte esas partes e ir directamente a la seccion sobre la simulacion, aunque no lo recomiendo. En tal caso. sin embargo, recuerda que esto simplemente te muestra tendencias bajo todas las asunciones del modelo que son varias y estimaciones de parametros hechas sobre informacion incompleta. Estan advertidos.

$Introduccion$

El coronavirus 2 del síndrome respiratorio agudo severo (SARS-CoV-2) es el causante de la enfermedad "coronavirus 2019 (COVID-19)" que nos tiene ahora a todos encerrados. Es un virus que usa ARN monocatenario de sentido positivo como material genetico, el cual puede servir como ARN mensajero y por tanto traducirse en proteína en la célula huésped directamente...

Y esto es todo lo que dire sobre la biologia del virus, por el simple hecho que no me sirve para mis propositos con este escrito. Lo que ahora me interesa es, usando lo que sabemos hasta ahora,  poder escribir sobre respuestas a preguntas como, si nos infectaremos todos? Cuanta gente infectada tiene que haber para que haya una epidemia? Si hay epidemia, cuales son los parametros que hay que monitorear para poder saber el estado de la propagacion de la enfermedad, por ejemplo, si esta mejorando o no, a base de nuevas medidas que se introduzcan? Y mas importante aun, claro esta, cuando mierda se acaba esto? Felizmente existen modelos matematicos simples cuyo proposito es modelar epidemias, y que cualquiera puede usar para explorar y tratar de responder preguntas de interes practico que uno tenga.

$El$ $modelo$

Ahora bien, resulta que el modelo en epidemologia mas sencillo y que nos puede dar algunas ideas es el modelo $SIR$, letras siendo las inciales de las palabras "Susceptible, Infected, Recovered", del ingles, aunque conincide con sus equivalentes en espanol. Este modelo divide la poblacion en tres compartimentos: el de los suceptibles $S$, los infectados $I$, y los recuperados $R$. Bajo este modelo, existen varias asunciones. Se asume, por ejemplo, que los miembros de $R$ no pueden ser re-infectados por el virus. Tampoco hay introduccion/sacada de miembros a la poblacion, fuere por migracion o por nuevos nacimientos, muertes... y esto es decir que la poblacion bajo estudio es constante; se asume que los miembros de toda la poblacion estan distribuidos en proporciones iguales, o sea, que es homogenea: y, para rematar, no se asume un tiempo de incubacion... Nada mas lejos de la realidad, pero da bastante intuicion sobre la propagacion de enfermedades infeciosas, sin embargo!

Ahora dejenme introducirles el modesto modelo $SIR$:

$$ \frac{dS}{dt} = - \beta SI \ \ \ \ \ (1)$$

$$ \frac{dI}{dt} = \beta SI - \gamma I \ \ \ \ \ (2) $$

$$ \frac{dR}{dt} = \gamma I \ \ \ \ \ (3) $$

con las condiciones iniciales $S(0) = S_0 >0, I(0) = I_0 >0$, $R(0) = 0$. Fijate que $S(t) + I(t) +  R(t) =  S_0 + I_0 = N$, ya que la poblacion, por asuncion, es constante.

Tecnicamente, esto es un sistema de ecuaciones diferenciales ordinarias no lineal. Si te suena complicado, no te lo creas. No lo es. Cada ecuacion te dice de manera cuantificada, el cambio de la cantidad de individous de cada compartimento en un instante de tiempo. $(1)$, por ejemplo, te dice que tan rapido cambia la poblacion de $S$ en un instante de tiempo. Que tan rapido es? La respuesta es $- \beta SI$. Por que? Miralo de esta forma: por cada infectado por instante de tiempo, hay $N\beta$ contactos. De esos  $N\beta$ contactos, $S/N$ son con sucsceptibles (por que? Pista: recuerda asuncion sobre distribucion de la gente en la poblacion). Como hay $I$ infectados, estamos hablando que son $(N\beta) (S/N)(I) = \beta SI$. El signo negativo esta includido porque a medida que la gente se infecta, la cantidad de miembros en $S$ disminuye, obviamente. Es decir, si empiezas con una poblacion inicial de $ S_0$ miembros, vas a tener $S(t) < S_0$ para todo $t$ $(t > 0)$. Similarmente, $(2)$, habla sobre $I$ y puedes ver que su cambio depende del cambio neto entre la gente que se enferma y los que se recuperan.

De importancia en particular son los parametros $\beta$ y $\gamma$. $\beta$ es una fraccion y tiene unidades de contactos por persona por dia. $\gamma$, en cambio, es la fraccion de infectados que se recuperan por unidad de tiempo y no soprendentemente lo llaman, a veces, como "el removal rate". Y fijate que $1/\gamma $ es el tiempo promedio de infeccion. Por ejemplo, si $\gamma = 1/10$, o sea, por dia el $10\%$ de la gente infectada se recupera, entonces $1/\gamma = 10$ dias y esto es decir que, en promedio, son $10$ dias que alguien infectado puede infectar a otros, antes de recuperarse.

Ahora ya habiendo introducido el modelo, podemos empezar con las preguntas.

$Cuando$ $hay$ $epidemia?$

Para ahorrar espacio, usare a veces la notacion de Lagrange para representar derivadas ($\frac{dS}{dt} = S'$). 

Escribe $S'_{*} = -S'$. Ahora $(2)$ es 

$$I' = S'_{*}  - R'$$

y como $I'$ representa el cambio de cantidad de infectados en un instante de tiempo, para que haya epidemia, requerimos que sea positivo.

Como puedes ver, esta ultima relacion es simplemete una resta que dice que, cuando $I' > 0$, hay mas que se enferman que de los que se recuperan, y por tanto cada vez habran mas infectados y tienes epidemia. Inversamente, si hay mas que se recuperen de los que se enferman, $ I' < 0$, y la poblacion de infectados va agotandose poco a poco hasta que se acaba, y no hay epidemia. Lo importante de esta simple resta es que puedes deducir que la epidemia se desvanece porque la poblacion queda libre de infectados y no porque se infectaron todos.

Ahota observa que cuando hay epidemia

$$I' = S'_{*}  - R' = \beta S_0I - \gamma I > 0$$

Pon $\beta S_0I - \gamma I = I(\beta S_0 - \gamma)$. Entonces

$$I(\beta S_0 - \gamma) > 0 \iff  \beta S_0 - \gamma > 0  \iff \beta S_0 > \gamma $$

De $\beta S_0 > \gamma$ puedes deducir does expresiones equivalentes de interes practico

$$S_0 >\gamma/\beta \iff S_0\beta/\gamma > 1 $$

Ambas expresiones te dicen bajo que condiciones la epidemia es garantizada. Cuando tienes $S_0 <\gamma/\beta \$ (o $R_0 > 1$), tienes epidemia. Inversamente, cuando tienes $S_0 >\gamma/\beta \$ (o $R_0 < 1$), no hay epidemia. El minimo valor para que haya epidemia, sin embargo, es cuando $S_0 = \gamma/\beta$ en donde $I' = 0$, como podras verificar. Incidentemente, este punto es cuando

$$ 0 = S'_{*}  - R' \iff  S'_{*}  = R' $$

Denota $S_0\beta/\gamma$ con el simbolo $R_0$. $R_0$ es conocido como el $basic$ $reproduction$ $number$ y es el numero de infecciones causadas por un infectado introducido a una poblacion susceptible, a lo largo de su vida como agente infeccioso. El ratio $\beta/\gamma$ en $R_0$ a veces se lo llama el "contact number".

$Cuando$ $se$ $acaba$ $esto?$

Esa informacion podemos deducir de las soluciones del sistema, las cuales, felizmente, es posible conseguir explicitamente aunque hay que cambiar la perspectiva (como reducir la dimension del sistema), el cual facilita las cosas y terminas con algo asi:

$$ \frac{dI}{dt} =  \frac{dI}{dS}  \frac{dS}{dt}$$

$$ \frac{dI}{dS} = \frac{ \frac{dI}{dS}}{ \frac{dS}{dt}} = -1 + \frac{\gamma}{\beta S} $$

Invocas el teorema fundamental de calculo 

$$ I(S) = \int_c^s \frac{dI}{dS}\Bigr|_{u=s}du + I(c) = -S + \gamma/\beta \ln{S}+ \text{constante} $$

De aqui puedes sacar bastante informacion. Dos pedazos de informacion, por ejemplo, son la cantidad maxima de infectados en un instante de tiempo durante toda la trayectoria $I$

$$I_{max} = I(\gamma/\beta) = S_0\left( 1 - \frac{1 + \ln{R_0}}{R_0}\right) $$

Y al resolver $ \frac{dS}{dR} = -\frac{\beta}{\gamma}S$, la cantidad de susceptibles que sobran cuando $I = 0$ 

$$ S(\infty)= S_0e^{\beta/\gamma(N_0 - S(\infty))} $$

que la encuntras en las raises de esa funcion trascendental, siendo este numero positivo (por que?) lo que implica que, segun el modelo, la epidemia termina antes de que todos se infecten y una parte se sale con la suya, en teoria...

Incidentemente, tambien puedes deducir los tiempos de estos eventos...

$Simulacion$ $Ecuador$

$Preliminares$

Para poner el modelo a correr se necesita los valores de los parametros $\beta$ y $\gamma$ y las condiciones iniciales $S_0,I_0, R_0$. Hay que estimarlos y en este caso es particularmente dificil. Primeramente, porque las estimaciones dependeran de los reportes oficiales y esto es un problema. Pues, como ya saben, no hay suficiente diagnostico y por tanto las cifras reportadas realmente no representa las cifras reales. El otro problema es que no sabemos con seguridad si la cifra que el gobierno reporta al publico, es la cifra que ellos tienen y la 'real' aunque, de todas formas, esta cifra tampoco es la cifra real, por la falta de diagnosticos. Entonces, como puedes imaginar, evidentemente las estimaciones basadas en informacion incompleta seguramente no seran precisas, peor aun cuando la incompleta informacion tambien es muy poca. Y para rematar, el modelo $SIR$ no es el apropiado debido a sus restricciones. A pesar de todo esto, es un experimento y me interesa saber las proyecciones que este simple modelo pueda darnos. 

Entonces, sin mas quejas, voy a suponer que $I_0$ es igual a $6$, no obstante pareciera que todo el relajo empezo con la senora que ha venido de Espana, aunque es bastante la gente que ha venido de Europa y no creo que sea realista asignar $I_0 = 1$. Pero el verdadero motivo de ponerlo igual a seis es porque los datos con los que estoy trabajando (de HDX), tiene reportado como $6$ casos desde el primero de marzo... 

Para estimar $ S_0$,  ten en cuenta que el modelo asume que la poblacion de los tres compartimentos esta distribuida homogeneamente y y eso obviamente no ocurre en el mundo real. Para darle la vuelta al problema, voy a asumir que $S_0$ es, en cambio, la poblacion susceptible expuesta, o sea, una parte de toda la poblacion, y entonces pondre $S_0 = 50000$. Esto me conviene tambien, porque la basura de mi laptop tendra serios problemas computando cifras con uno o dos ordenes de magnitud superior a mi cifra.

Sobre $\beta$ y $\gamma$, hay que estimarlos usando informacion de casos reportados oficialmente. Para el caso Ecuador, lamentablemente, la estimacion de $\gamma$ es particularmente dificil. Hasta ahora, 27 de marzo, solamente en una ocasion se ha reportado oficialemente la recuperacion de 3 pacientes. De hecho, reporte de casos de muertes es diez veces mas a los de recuperacion! Ahora esto, claro esta, no quiere decir que los prospectos de los infectados en Ecuador a recuperase sean bastante malos. Recuperacion puede demorar y es cuestion de tiempo, verdad?

Para la estimaciones, he usado para el 'loss function' en el proceso de optimización el error cuadrático medio (RMSE).

$Resultados$ $de$ $la$ $simulacion$ $Ecuador$ $con$ $modelo$ $SIR$

Los siguiente datos son actualizados cada tres dias, a medida que nuevos casos van reportandose, mejorando las estimaciones de los parametros. Mas importante aun, el diario monitoreo de los parametros podra darnos una idea si las tendencias estan mejorando o no.

Aqui tienes la simulacion a largo plazo. (Te tocara abrir la imagen.) Recuerden que hacer proyecciones a largo plazo y con informacion incompleta, especialmente, es un crimen. Como fuere, aqui lo tienes: 

![Proyecciones](/EcuadorS.png)

aunque alrededor de nuestra fecha actual los numeros no estan tan disparados 

![Proyecciones](/EcuadorZ.png)

Valor de los parametros:

![Proyecciones](/vtab.png)

Aqui ya te das cuenta que los numeros estan mal y eso era de esperarse. Hay varias posibles razones de la causa y posibles arreglos que puedan enmendar un poco los numeros. Pero creanme, ese es otro trabajo, y ahora me da pereza escribir sobre eso. De hecho, este articulo ha sido mas largo de lo que esperaba. Ademas, si le doy tiempo al tiempo, a medida que haya mas datos, las estimaciones seran mejores, espero... 

Como fuere, quizas me anime a escribir otro articulo usando extensiones del presente modelo que son mas apropiados para nuestra circunstancia.

Bis bald!
Juan

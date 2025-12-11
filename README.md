
# Analisis de representacion del conocimiento aplicado a FANN para Qbits. 




## Abstract



## Introduccion


## Background


### Qubit transmission problem
El problema de tranmisión de Qubits [1] viene de la física cuántica. Un qubit o bit cuántico es la unidad básica de información en computación cuántica, análoga al bit clásico pero con propiedades mucho más ricas debido a los principios de la mecánica cuántica. Una cadena de transmisión de qubits es un sistema o arquitectura diseñada para transportar estados cuánticos es decir, qubits de un lugar a otro sin perder su información cuántica (superposición, amplitudes y posibles correlaciones como el entrelazamiento). En otras palabras, es el equivalente cuántico a una línea de transmisión clásica, pero adaptada para mover información extremadamente delicada que puede perderse con facilidad.
Podemos pensarla como una cadena de partículas cuánticas interactuando en el tiempo según las leyes de la física cuántica. El estado inicial es con todas las partículas en reposo menos la primera que está en un estado de exitación. Quisiéramos ver la misma exitación sobre la última particula de la cadea pero claramente no es el caso dado que la interacción entre ellas debilita la exitación inicial. Una partícula exitada va estimulando a sus vecinas por lo que se puede apreciar una onda de estimulacion viajando en la cadena (por consiguiente se puede observar la posición y amplitud de su cresta). Para tratar de mitigar la pérdida de exitacion los físicos propusieron entre otras cosas actuar externamente sobre una partícula en cada momento de tiempo con la esperanza de que llegue al final tanta exitación como sea posible con respecto al estado inicial. La cuestión entonces es en qué partícula actuar en cada momento del tiempo y esa es la respuesta que se busca en [2] usando FANN's y RL. En este entorno una solución entonces es un vector con las posiciones de las partículas estimuladas en cada instante de tiempo. La forma de medir cuanta exitación esta llegando al final es con una magnitud física llamada fidelidad, la cual se evalúa luego de aplicar cada estímulo en el orden de tiempo y comparando lo que llega con respecto a lo que había en la primera partícula. Por último el estado de la cadena es un verctor de números complejos (tantos como eslabones tiene la cadena) los cuales son el estado cuántico de cada partícula. Así entonces diremos que el estado de una cadena de longitud $n$ es un vector $\vec{e}=(c_1,...,c_n)$ donde $c_i$ son números complejos.
<img src="QbitChain.png" width="500" />

### FANN & RL
Las redes FANN [2] son una clase de Red Neuronal Artificial biológicamente inspirada. Esta inspiración viene dada tanto por su arquitectura como por su funcionamiento y composición ya que tienen asociada una dinámica neuronal la cual define que pasa en una neurona que esta siendo estimulada a través de sus conexiones dendríticas, las cuales a su vez pueden ser inhibitorias, exitatorias o eléctricas. Como definidas en [2] tenemos principalmente 4 clases de arquitectura FANN (TWC, FC y sos modificaciones SA). El análisis del presente trabajo esta basado en el uso de las arquitecturas FC ya que se centra en el estudio del impacto de cambiar (en un mismo modelo) la representación del estado de la cadena de transmisión de Qubits y las arquitecturas TWC siempre tienen la misma cantidad de neuronas de entrada mientras que en las FC se puede definir dicha cantidad a necesidad. Por otro lado también quedan excluidas de este análisis las arquitecturas SA ya que cada entrenamiento de éstas en realidad genera una nueva red neuronal diferente a la de otro entrenamiento invalidando la posibilidad aislar la comparación solamente a la representación del estado de la cadena.
En las imágenes de abajo se ven ejemplos de todas estas arquitecturas, sólo a modo informativo son brevemente descriptas aquí, el lector interesado en tener mas detalles puede consultar [2]. 

<ul>

  <li>
    TWC: Este circuito es es un modelo exacto de un circuito encontrado en un ser vivo, consta de 11 neuronas y 26 conexiones. 4 neuronas son de entrada (triangulares no verdes), dos son de salida (triangulares verdes) y 5 internas, en esas imágenes están rotuladas las conexiones con sus pesos en el momento en que fueron tomadas las imágenes. Las neuronaes tasán rotuladas con los valores de sus estados internos.
  </li>  
  <li>  
FC: Estas redes son al estilo de las Fully Connected encontradas en el Deep Learning clásico, cada neurona de cada capa está conectada con todas las de la capa siguiente, se respeta una proporción aproximada entre las diferentes clases de conexiones con respecto a lo observado en TWC y sólo tiene una neurona de salida.
      </li>  
  <li>  
SA: Shuffle Architecture es la arquitectura obtenida a partir de una arquitectura básica (TWC o FC) pero justo antes del entrenamiento las conexiones son aleatoriamente mezcladas cambiando así el trazado original. Esta arquitectura es básicamente diferente en cada entrenamiento. 
      </li>  

<ul>
  
Ejemplos de TWC y TWC - SA

<img src="TWC.png" width="700" />

Ejemplos de FC y FC - SA

<img src="FC.png" width="700" />

En [2] estas redes son usadas conjuntamente con RL para encontrar una solución al problema. Ahí se implementa un environment desde el punto de vista de Reinforcement Learning. La parte que nos interesa conocer en este momento es que este environment modela el Qbit transmission control problem exponiendo como descriptor de estado el vector de estado de las partículas cuánticas $\vec{e}$, desde donde se sacan las 4 versiones de características que serán pasadas a la FANN   

### KR
La representación del conocimiento constituye un pilar fundamental en el desarrollo de sistemas de inteligencia artificial ya que determina cómo la información del mundo real o del problema modelado es estructurada y procesada por los componentes algoritmicos. En el contexto del machine learning, esta representación adquiere especial relevancia al influir directamente en la capacidad de un modelo para aprender patrones útiles a partir de los datos. En el ambito de las redes neuronales artificiales el desempeño depende en gran medida de la calidad y la forma en que se presentan sus entradas.
La cantidad de entradas o número de características de una red neuronal es un aspecto crítico de su funcionamiento puesto que define el espacio de información disponible para el aprendizaje. Una representación innecesariamente amplia puede introducir ruido, redundancias y costos computacionales elevados, mientras que una representación insuficiente puede limitar la capacidad del modelo para capturar relaciones relevantes. Este trabajo intenta discernir el impacto de 4 diferentes formas de representar el conocimiento para el caso del problema de cadenas de transmision de Qubits. Como bien se dijo antes, el estado de la cadena es un vector de numeros complejos, cada uno representando el estado cuantico de una particula en la cadena por lo que el tamaño del vector es igual a la longitud de la cadena.
Si $\vec{e}=((r_1,i_1),...,(r_n,i_n))$ es el estado de una cadena de longitud $n$, en este trabajo se prueba alimentar a las redes que buscan una solucion con 4 aproximaciones diferentes:
<ul>
  <li>Sólo la parte imaginaria del estado: en este caso la red neuronal tiene $n$ neuronas de entrada y se la alimenta con $(i_1),...,(i_n)$.</li>
  <li>Sólo la parte real del estado: en este caso la red neuronal tiene $n$ neuronas de entrada y se la alimenta con $(r_1),...,(r_n)$. </li>
  <li>El estado completo: en este caso la red neuronal tiene $2*n$ neuronas de entrada y se la alimenta con $\vec{e}$.</li>
  <li>Sólo 2 valores representando la posición y magnitud de la cresta de la onda: en este caso la red tiene sólo $2$ neuronas de entrada y se la alimenta con los valores $a,p$ donde  $(\forall i: 1 \leq i \leq n:  \vert{c_i}\vert{} \leq a)$ y $p=i$ con $a=\vert{c_i}\vert{}$.</li>
</ul>



## Desarrollo experimental
## Analisis estadisticos
## Resultados
## Conclusiones

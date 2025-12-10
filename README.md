# Analisis de representacion del conocimiento aplicado a FANN para Qbits. 
## Abstract
## Introduccion
## Background
### Qubit transmission problem
El problema de tranmision de Qubits [1] viene de la fisica cuantica. Un qubit o bit cuántico es la unidad básica de información en computación cuántica, análoga al bit clásico pero con propiedades mucho más ricas debido a los principios de la mecánica cuántica. Una cadena de transmisión de qubits es un sistema o arquitectura diseñada para transportar estados cuánticos es decir, qubits de un lugar a otro sin perder su información cuántica (superposición, amplitudes y posibles correlaciones como el entrelazamiento). En otras palabras, es el equivalente cuántico a una línea de transmisión clásica, pero adaptada para mover información extremadamente delicada que puede perderse con facilidad.
Podemos pensarla como una cadena de particulas cuanticas interactuando en el tiempo segun las leyes de la fisica cuantica. El estado inicial es con todas las particulas en reposo menos la primera que esta en un estado de excitacion. Quisieramos ver la misma excitacion sobre la ultima particula de la cadea pero claramente no es el caso dado que la interaccion en tre ellas debilita la excitacion inicial. Una particula exitada va estimulando a sus vecinas por lo que se puede apreciar una onda de estimulacio viajando en la cadena. Para tratar de mitigar la perdida de exitacion los fisicos propusieron entre otras cosas actuar externamente sobre una particula en cada momento de tiempo con la esperanza de que llegue al final tanta exitacion como la presente en la primer particula en el estado inicial. La cuestion entonces es en que particula actuar en cada momento del tiempo y esa es la respuesta que se busca en [2] usando FANN's y RL. En este entorno una solucion entonces es un vector con las posiciones de las particulas estimuladas en cada instante de tiempo. La forma de medir cuanta exitacion esta llegando al final es con una magnitur fisica llamada fidelidad, la cual se evalua luego de aplicar cada estimulo en el orden de tiempo y comparando lo que llega con respecto a lo que habia en la primer particula. Por ultimo el estado de la cadena es un verctor de numeros complejos (tantos como eslabones tiene la cadena) los cuales son el estado cuantido de cada particula.
<img src="QbitChain.png" width="500" />
### FANN & RL
Las redes FANN [2] son una clase de Red Neuronal Artificial biologicamente inspirada. Esta inspiracion viene dada tanto por su arquitectura como por su funcionamiento ya que tienen asociada una dinamica neuronal la cual define que pasa en una neurona que esta siendo estimulada a traves de sus conexiones dendriticas. Como definidas en [2] tenemos principalmente 3 clases de arquitectura FANN (TWC, FC y SA). El analisis del presente trabajo esta basado en el uso de las arquitecturas FC ya que se centra en el estudio del impacto de cambiar (en un mismo modelo) la representacion del estado de la cadena de transmision de Qubits y las arquitecturas TWC siempre tienen la misma cantidad de neuronas de entrada mientras que en las FC se puede definir dicha cantidad a necesidad. Por otro lado tambien quedan excluidas de este analisis las arquitecturas SA ya que cada entrenamiento de estas en realidad genera una nueva red neuronal diferente a la de otro entrenamiento invalidando la posibilidad aislar la comparacion solamente a la representacion del estado de la cadena.
En [2] estas redes son usadas conjuntamente con RL para encontrar una solucion al problema.  

Ejemplos de TWC y TWC - SA

<img src="TWC.png" width="700" />

Ejemplos de FC y FC - SA

<img src="FC.png" width="700" />

### KR
La representación del conocimiento constituye un pilar fundamental en el desarrollo de sistemas de inteligencia artificial ya que determina cómo la información del mundo real o del problema modelado es estructurada y procesada por los componentes algoritmicos. En el contexto del machine learning, esta representación adquiere especial relevancia al influir directamente en la capacidad de un modelo para aprender patrones útiles a partir de los datos. En el ambito de las redes neuronales artificiales el desempeño depende en gran medida de la calidad y la forma en que se presentan sus entradas.
La cantidad de entradas o número de características de una red neuronal es un aspecto crítico de su funcionamiento puesto que define el espacio de información disponible para el aprendizaje. Una representación innecesariamente amplia puede introducir ruido, redundancias y costos computacionales elevados, mientras que una representación insuficiente puede limitar la capacidad del modelo para capturar relaciones relevantes. Este trabajo intenta discernir el impacto de 4 diferentes formas de representar el conocimiento para el caso del problema de cadenas de transmision de Qubits. Como bien se dijo antes, el estado de la cadena es un vector de numeros complejos, cada uno representando el estado cuantico de una particula en la cadena por lo que el tamaño del vector es igual a la longitud de la cadena.
En este trabajo se prueba alimentar a la red con:
<ul>
  <li>solo la parte compleja del estado</li>
  <li>solo la parte real del estado</li>
  <li>el estado completo</li>
  <li>solo 2 magnitudes representando la posicion y magnitud de la cresta de la onda</li>
</ul>



## Desarrollo experimental
## Resultados
## Conclusiones

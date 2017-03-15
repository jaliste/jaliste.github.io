```

```

# Algebra lineal: consideraciones numéricas

Lo primero que veremos en este curso es como resolver sistemas de ecuaciones lineales de la forma

$$
\begin{array}c
a_{11}x_1+a_{12}x_2+\cdots + a_{1n}x_n = b_1 \\
a_{21}x_1+a_{22}x_2+\cdots + a_{2n}x_n = b_2\\
\vdots\\
a_{n1}x_1+a_{12}x_2+\cdots + a_{nn}x_n = b_n
\end{array}
$$


En notación matricial, escribimos,

$$
Ax = b
$$


donde $$A$$ es una matriz real cuadrada de $$n\times n$$. Recordemos que este sistema tiene solución si y solo sí $$det(A)\neq 0$$, en cuyo caso, además la solución es única.

En principio, si $$det(A)\neq 0$$, podemos usar la regla de Cramer para calcular la solución $$x  = A^{-1}b$$. Luego, teoricamente tenemos un algoritmo para resolver sistemas cuadrados:

a.- Calcule el determinante de $$A$$.  
b.- Si el determinante es distinto de $$0$$, aplicar la regla de Cramer para calcular $$x_i$$:


$$
x_i = \frac{det(A_i)}{det(A)}
$$


donde $$A_i$$ se obtiene de reemplazar la $$i$$-ésima columna de $$A$$ por $$b$$. **La pregunta natural que aparece al considerar esta fórmula es: ¿Qué tan rápido es este algoritmo?**

El problema de la regla de Cramer es que calcular el determinante de manera directa, usando por ejemplo la definición,


$$
det(A) = \sum_{i=1}^{n} a_{ij}(-1)^{i+1}det(A_{ij})
$$


donde $$A_{ij}$$ es la matriz que se obtiene al eliminar la columna $$j$$ y la fila $$i$$, es un procedimiento que necesita de muchas operaciones.

**Ejercicio 1.**  
Cuantas operaciones algebraícas se utilizan para calcular un determinante de $$2\times 2$$, $$3\times 3$$, $$4\times 4$$, $$5\times 5$$. Y en el caso $$n\times n$$?

Como resultado del Ejercicio 1, el problema de calcular el determinante de una matriz de $$100\times 100$$ se vuelve intratable si usamos la definición del determinante de más arriba. Sin embargo, veremos formas mucho mas eficientes de calcular el determinante. Existen numerosos problemas en aplicaciones de ingeniería y física donde debemos resolver sistemas lineales de muchas variables \(incluso miles o millones de variables\)... **Luego necesitamos aprender a medir cuan rápido es un algoritmo.**

**Observación.**  Teoricamente, para cada problema existe un algoritmo que requiere el menor número de operaciones posibles. A este número se le conoce como complejidad del algoritmo y se escribe, usualmente, en términos del tamaño del problema a resolver. Más adelante daremos una definición más precisa  
de complejidad.

### Como resolver sistemas triangulares

Estudiemos ahora un caso en principio bastante particular de sistema


$$
\begin{array}c
&r_{11}x_1+&r_{12}x_2+&\cdots + &r_{1n}x_n = z_1 \\\\
&                   &r_{22}x_2+&\cdots + &r_{2n}x_n = z_2\\\\
&                   &                    &\ddots    &     \vdots\\\\
&                   &                    &              & r_{nn}x_n = z_n
\end{array}
$$


En notación matricial, $$Rx = b$$ donde la matríz $$R$$ es triangular superior, es decir $$r_{ij} = 0$$ para todo $$i>j$$. Este sistema se resuelve de manera directa resolviendo la $$n$$-esima ecuación y reemplazando el valor de $$x_n$$ en la $$n-1$$-ésima ecuación y así sucesivamente. En fórmulas,


$$
\begin{array}l
x_n &:= z_n / r_{nn},&\quad \text{si } r_{n,n}\neq 0,\\\\
x_{n-1}&:= (z_{n-1}-r_{n-1,n}x_n)/r_{n-1,n-1},&\quad\text{si } r_{n-1,n-1}\neq 0,\\\\
\vdots\\\\
x_{1} &:= (z_1 - r_{12}x_2-\cdots-r_{1n}x_n)/r_{11},&\quad \text{si }r_{11}\neq0.
\end{array}
$$


es claro que $$det(R)=r_{1,1}r_{2,2}\cdots r_{n,n}$$.

**Proposición.** $$det(R)\neq 0$$ si y solo sí $$r_{i,i}\neq 0$$ para todo $$i$$.

Esto nos dice que las formulas anteriores nos proveen un algoritmo para resolver el sistema exactamente cuando el sistema tiene solución única. El costo de calcular esta solución será:

a. Para la i-ésima fila, $$n - i$$ sumas , $$n-i$$ multiplicaciones, una división.  
b. En total tenemos $$n^2 + o(n^2)$$ operaciones.

Llamaremos **substitución inversa** a esta forma de resolver un sistema triangular superior,  
y por analogía, se pueden resolver sistemas triangulares inferiores usando **substitución directa**.

**Ejercicio.** Escribir las fórmulas de substitución hacia adelante para resolver un sistema $$Lz=b$$ donde $$L = (l_{ij})$$ es una matriz triangular inferior de $$n\times n$$.

**Observación.** El determinante de una matriz triangular superior de $$n\times n$$ puede ser calculado usando $$n$$ operaciones algebraícas.

### Implementando la solución en Python3.

Pensemos ahora que queremos resolver el sistema $$Rx=z$$ en el computador. La primera pregunta que nos hacemos es como guardar la matriz $$R$$ y el vector $$z$$. Este curso está basado en Python3 usando como módulos principales numpy, scipy y matplotlib, luego veremos como generar y acceder matrices en python usando el módulo.

**Definición.** Un módulo es un agrupamiento  de  funciones y objetos de Python con un objetivo cómun.

Por ejemplo, el módulo numpy contiene todos los objetos y funciones necesarias para poder trabajar en cálculo numérico de manera eficiente. Hay básicamente dos formas de usar los objetos y funciones de un módulo.

```python
import numpy as np
```

o alternativamente

```python
from numpy import array
```

La primera instrucción le pide a Python leer las definiciones del módulo numpy y asignarles el nombre de espacio _np_. Esto quiere decir que para llamar a una función de numpy se debe anteponer _np_ seguido de un punto al nombre de la función.

**Ejemplo:**

```python
import numpy as np
A = np.array([1,2])
B = np.zeros(10)
```

El nombre _np_ es solo una convención para escribir _np.array_ en vez de _numpy.array_. La segunda forma importa la definición de array y la agrega al espacio **global**. Esto quiere decir que no es necesario anteponer el _np_ para llamar a array. **Esta forma de importar funciones de numpy no es recomendada para principiantes pues puede generar confusiones.**

#### Arrays

En numpy la mayoría de los datos se guardan usando el tipo **np.array**. Este tipo de objetos representa una tabla $$n$$-dimensional de datos, donde $$n$$ puede ser arbitrario. Un array tiene las siguiente propiedades o atributos.

* **array.ndim** número de dimensiones del **array**.  Usualmente los vectores se representan usando un array con ndim=1 y las matrices usando un array con ndim=2.

* **ndarray.shape** El tamaño o forma del arraglo. Esta es una $$n$$-tupla  indicando el tamaño del array en cada dimensión.  Por ejemplo, para una matriz de $$n$$ filas y $$m$$ columnas, **shape** será $$(n,m)$$. El largo del **shape** siempre será el número de dimensiones, **ndim**.

* **ndarray.size** El número total de elementos del array.

* **ndarray.dtype** Descripción del tipo de datos de cada elemento del array. Todos los datos del array son del mismo tipo, por ejemplo, enteros, boolean, floats, etc.

**Ejemplo:**

```python
import numpy as np

A = np.array([0,1])
B = np.array([0,1],dtype=float)
```

En el array $$A$$ cada elemento del array será de tipo entero, mientras que en $$B$$ cada elemento será del tipo float. Concentremos nos ahora en arrays 1 y 2 dimensionales y como crearlos.  Existen varias funciones para crear arrays, de las cuales solo veremos, por ahora , las básicas.  Para más detalles sobre cada función, ver la ayuda de cada una.

* **np.zeros** Crea un array de ceros.
  ```python
  np.zeros(10)   # Crea un array 1-dimensional de 10 ceros
  np.zeros((10,20)) # Crea un array 2-dimensional de 10 filas y 20 columnas de ceros.
  ```
* **np.eye**  Crea un array con unos en la diagonal y ceros fuera.
  ```python
  np.eye(3)          # Crea un array con la matriz identidad de 3x3
  np.eye(4,k=1)
  np.eye(4,k=-1)
  ```
* **np.array** \# Crea un array con los datos proporcionados.
  ```python
  np.array([1,2,3])
  np.array([[1,2,3],[2,3,4]])
  ```
* **np.ones**  \# Crea un array de unos.

  ```python
  np.ones(10)
  np.ones((10,10))
  ```

* **np.diag**    \# Crea un array 2-dimensional con los datos proporcionados en la diagonal.

  ```
  np.diag([1,2,3,4])
  np.diag([1,2,3,4],k=1)
  np.diag([1,2,3,4],k=-1)
  ```

con esta información ya podemos crear nuestro problema en el computador. Por ejemplo,  
supongamos que


$$
 R = \begin{pmatrix} 1 & 2 & 3 \\\\0 & -3 & 1\\\\ 0& 0& 2\end{pmatrix},\qquad z = \begin{pmatrix} 0\\\\1\\\\-2\end{pmatrix}
$$


Este sistema lo ingresaremos en python de la siguiente forma:

```python
import numpy as np    # Dejaremos de escribir esta linea, pero siempre debe ser la primera linea de nuestro programa
R = np.array([[1,2,3],[0,-3,1],[0,0,2]])
z = np.array([0,1,-2])
```

La pregunta ahora es como transformar las ecuaciones que describen la solución en código. Aquí aparece la primera confusión: **¿Cuanto vale **$$r_{1,2}$$**?**. La respuesta es clara, $$1$$ se refiere a la primera fila y $$2$$ se refiere a la segunda columna, luego $$r_{1,2}=2$$. Sin embargo,  
en python

```python
print(R[1,2])
```

imprimirá $$1$$.

**IMPORTANTE.** En Python3, los índices siempre parten desde $$0$$! Esto significa que la primera fila corresponde a la fila $$0$$, luego para acceder al elemento en la primera fila y segunda columna deberemos ejecutar

```python
print(R[0,1])
```

Podemos ahora implementar nuestra solución cuando $$n = 3$$. Antes debemos sí re-escribir las ecuaciones de la solución


$$
\begin{array}l
x_3 &=z_3/r_{33}\\
x_2 &=(z_2-r_{23}x_3)/r_{22}\\
x_1 &= (z_1 - r_{12}x_2-r_{13}x_3)/r_{11}
\end{array}
$$


Ahora debemos transformar estas ecuaciones en un programa. Una nueva pregunta que aparece es como generar el vector $$x$$. La convención usual es crear el vector $$x$$ como un array del tamaño correcto con puros ceros usando la función _np.array_ y luego calcular cada valor de $$x_i$$.

```python
x = np.zeros(3)
x[2] = z[2]/R[2,2]
x[1] = (z[1]-R[1,2]*x[2])/R[1,1]
x[0] = (z[0]-R[0,1]*x[1]-R[0,2]*x[2])/R[0,0]
```

**Es importante observar como cambian los índices entre la matemática y Python. Para evitar esta diferencia, podemos \(casi siempre\) escribir la matemática usando indices que comiencen en **$$0$$

El código de más arriba es correcto pero solo funcionará cuando el tamaño $$n$$ sea igual a $$3$$. Si $$n$$ fuera igual a $$100$$ y quisieramos escribir un código analogo, tendríamos que escribir un código de 100 lineas!!! Más aún, que pasa si no conocemos $$n$$ al momento de diseñar nuestro programa. Recordemos que de la fórmula de substitución inversa
tenemos

$$
x_{i} = (z_i - r_{i,i+1}x_{i+1}-\cdots-r_{i,n}x_n)/r_{i,i}
$$
que puede ser re-escrita como sumatoria:
$$
x_i = (z_i - \sum_{j=i+1}^n r_{i,j}x_j) / r_{i,i}
$$

Esta sumatoria puede ser calculada en Python usando la directiva **for** y la función **range**.

---
**range\(a,b,s\).** genera una progresión aritmética $$a,a+s,a+2s,\cdots,a+ks$$, donde $$k$$ es el número más grande tal que $$a + ks < b$$ cuando $$s$$ es positivo ó $$a + ks > b$$ cuando $$s$$ es negativo.
***
**for.** nos permite iterar el valor de un _nombre_ \(variable\) desde una secuencia.
***
Podemos ahora calcular la sumatoria:

```py
x[i] = z[i]
for j in range(i+1,n):   # los indices parten de cero.
  x[i] -= R[i,j]*x[j]
x[i] /= R[i,i]
```
Ahora que tenemos un algoritmo para calcular $$x_i$$ necesitamos
calcular $$x_i$$ para todo $$i$$, pero desde abajo! Además necesitamos calcular $n$, que corresponde al número de filas de $$R$$. Este se puede obtener usando la propiedad **array.shape**. El programa completo queda:

```py
import numpy as np
R = np.array([[1,2,3],[0,-3,1],[0,0,2]])
z = np.array([0,1,-2])
n,m = R.shape
x = np.zeros(n)

for i in range(n-2,-1,-1):
  x[i] = z[i]
  for j in range(i+1,n):
    x[i] -= R[i,j]*x[j]
  x[i]/=R[i,i]
```
### Eliminación Gaussiana

Ahora recordemos el proceso de eliminación Gaussiana, que viene a ser el método más eficiente, entre los métodos clásicos, para resolver sistemas de ecuaciones lineales.  Carl Friedrich Gauss \(1777-1855\) describe este método en su trabajo _Theoria Motus Corporum Coelestium_ \(1809\)  
    " Los valores pueden ser obtenidos usando el método de eliminación usual "  
Sin embargo, se sabe que el método de eliminación ya era conocido por J.L. Lagrange en 1759 e incluso se conocía en China al menos en el siglo primero d.C.

Volvamos a considerar el sistema general (a partir de ahora consideramos los índices, cuando sea posible, partiendo de $$0$$)


$$
\begin{array}c
a_{00}x_0&+a_{01}x_1&+\cdots &+ a_{0,n-1}x_{n-1} = b_0\\
a_{10}x_0&+a_{11}x_2&+\cdots &+ a_{1,n-1}x_{n-1} = b_2\\
&&\vdots\\
a_{n-1,1}x_1&+a_{n-1,2}x_2&+\cdots &+ a_{n-1,n-1}x_{n-1} = b_n
\end{array}
$$
y tratemos de transformarlo en un sistema \(equivalente\) triangular superior.  La idea es manipular el sistema de manera que los coeficientes $$a_{21}$$ hasta $$a_{n1}$$ se anulen. \(De ahi el nombre de **eliminación** del método\)  
Consecuentemente obtenemos
$$
\begin{array}c
a_{00}x_0+&a_{01}x_1+\cdots + a_{0,n-1}x_{n-1} = b_0' \\
&a_{11}'x_1+\cdots + a_{1,n-1}'x_{n-1} = b_1'\\
&\vdots\\
&a_{n-1,1}'x_1+\cdots + a_{n-1,n-1}'x_{n-1} = b_{n-1}'
\end{array}
$$

La nueva matriz $$A'$$ es el resultado de **pivotear la columna 0** usando la fila 0 como **pivote**. Veamos como implementar este método: Para eliminar el término $$a_{i0}x_0$$  de la fila $$i$$ le restaremos un múltiplo de la fila $$0$$, es decir,
$$
\text{nueva fila }i := \text{fila }i - l_{i1}\cdot  \text{fila }0.
$$ Explicitamente,  
$$
a_{ij}' = a_{ij} - l_{i0}a_{0j}\qquad\forall j\in\{0,\ldots,n-1\}
$$y$$
b_i' = b_i  - l_{i0}b_1
$$
Para calcular $$l_{i0}$$ imponemos la condición  $$a_{i0}-l_{i0}a_{00}=0$$, de donde se obtiene que  
$$
l_{i0}=a_{i0}/a_{00}
$$
Luego podemos realizar este proceso si y solo sí suponemos que el **pivote** $$a_{00}$$ es distinto de $$0$$. Las ecuaciones para $$a_{i,j}'$$ se traducen en python de manera bastante directa:

```py
for i in range(1,n):
  l[i,0] = A[i,0] / A[0,0]
  for j in range(0,n):
    A[i,j] = A[i,j] - l[i,0]*A[0,j]
```
**Importante.** Aún no hemos definido nuestra matriz $$l$$, luego la linea donde usamos $$l_{i0}$$ generará un error en python. Otro problema es que estamos guardando el resultado del cálculo en el mismo array $$A$$.


Después de este paso del método de eliminación, obtenemos una submatriz de $$(n-1)\times (n-1)$$ en las filas 2 a las $$n$$. Es decir, tenemos la situación inicial pero con un tamaño menor. Así, podemos aplicar recursivamente el método para encontrar una secuencia de matrices


$$
 A = A^{(1)}\rightarrow  A^{(1)} \rightarrow \cdots \rightarrow A^{(n)} =: R
$$


ssdsd  
sdsdsds d

```python
for i in range(1:n):
    blabla
sdsd sd
```
Luego, podemos aplicar este método recursivamente a las últimas $$n-1$$ filas para obtener un sistema triangular superior. Esto quiere decir que solo necesitamos entender como  
como transformar el sistema \(_\) en el sistema \(\*_\).
s

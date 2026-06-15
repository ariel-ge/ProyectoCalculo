# Proyecto - Cálculo II

## Universidad Nacional Autónoma de México

*Matemáticas Aplicadas y Computación*

**Equipo:**
- Govea Escalona Ingemar Ariel
- López Sanchez Uriel Alejandro
- Miranda Mateo Hector Adolfo
- Sánchez Ortega Tsugui

**Profesor:** Ricardo Guzmán Fuentes

---

## Descripción

Este proyecto implementa **cinco aplicaciones prácticas** basadas en conceptos de Cálculo II (integral definida, sumas de Riemann, integración numérica):

| Módulo | Descripción | Concepto matemático |
|--------|-------------|---------------------|
| 1 | Integración Romberg | Extrapolación de Richardson |
| 2 | Limpiador de señal (filtro pasabajo) | Convolución + Sumas de Riemann |
| 3 | Simulación de montaña rusa | Longitud de arco \( L = \int_a^b \sqrt{1 + [f'(x)]^2} \, dx \) |
| 4 | Análisis de potencia RMS en guitarra eléctrica | Valor eficaz \( V_{RMS} = \sqrt{\frac{1}{T} \int_0^T [v(t)]^2 \, dt} \) |
| 5 | Excedente del consumidor CFE | Área bajo la curva |

---

### Módulo 1: Crecimiento poblacional (Integración Romberg)

#### Introducción

El estudio de las poblaciones biológicas es un pilar fundamental en la ecología y la biología matemática. Comprender cómo y por qué cambia el número de individuos en una población a lo largo del tiempo permite predecir tendencias, gestionar recursos naturales y planificar estrategias de conservación o control.

Desde una perspectiva matemática, estos cambios siguen patrones que pueden describirse mediante **ecuaciones diferenciales**. Este proyecto demuestra cómo el cálculo integral, específicamente a través del método de **Extrapolación de Romberg**, permite calcular con alta precisión el crecimiento poblacional total en un intervalo de tiempo $[a, b]$, partiendo de la tasa de cambio instantánea de la población.

#### Fundamentos biológicos

**Crecimiento poblacional:** Aumento en el número de organismos que integran una población en un tiempo determinado, debido a tres factores: natalidad, mortalidad y migraciones.

**Natalidad:** Proceso por el cual la población aumenta debido a individuos que nacen.

**Mortalidad:** Número de organismos que mueren, disminuyendo la población.

**Migraciones:** Movimiento de organismos hacia otras regiones (emigración = salida, inmigración = entrada).

En los modelos simplificados (exponencial y logístico), asumimos migraciones nulas (población cerrada), por lo que el cambio neto depende del balance entre natalidad y mortalidad, agrupado en una **tasa de crecimiento intrínseca ($r$)**.

---

#### Método Numérico: Extrapolación de Romberg

Cuando no es posible encontrar una antiderivada analítica, recurrimos a la integración numérica. La **Extrapolación de Romberg** mejora sistemáticamente la aproximación de una integral definida.

**Regla del Trapecio:** Aproxima el área bajo la curva $f(x)$ en $[a, b]$ dividiéndolo en $n$ subintervalos de ancho $h = \frac{b-a}{n}$:

$$
T(n) = \frac{h}{2} \left[ f(a) + f(b) + 2 \sum_{i=1}^{n-1} f(a + i \cdot h) \right]
$$

El error es proporcional a $h^2$ (es decir, $\mathcal{O}(h^2)$).

**Extrapolación de Richardson:** Si calculamos la aproximación con paso $h$ y con paso $h/2$, podemos combinar ambas para eliminar el error $\mathcal{O}(h^2)$, obteniendo una nueva aproximación con error $\mathcal{O}(h^4)$. La fórmula general es:

$$
R_{k,j} = R_{k,j-1} + \frac{R_{k,j-1} - R_{k-1,j-1}}{4^j - 1}
$$

Donde:
- $R_{k,0}$ es la Regla del Trapecio con $n = n_0 \cdot 2^k$ subintervalos
- $R_{k,j}$ es la aproximación refinada en la fila $k$ y columna $j$
- $4^j - 1$ es el factor de corrección

**Algoritmo de Romberg:** Construye una tabla triangular. Se comienza calculando la primera columna ($j=0$) con la regla del trapecio duplicando subintervalos en cada fila ($k$). Luego se rellenan las columnas siguientes con Richardson. El proceso se detiene cuando:

$$
|R_{k,k} - R_{k-1,k-1}| < \epsilon
$$

---

#### Modelado mediante Ecuaciones Diferenciales

Sea $P(t)$ el tamaño de la población en el tiempo $t$.

**1. Crecimiento Natural (Exponencial):** Recursos ilimitados. La tasa de cambio es proporcional al tamaño actual:

$$
\frac{dP}{dt} = r \cdot P(t)
$$

Solución analítica: $P(t) = P_0 e^{rt}$

**2. Crecimiento Logístico:** Recursos finitos. Introducimos la **capacidad de carga** $K$ (máximo de individuos que el entorno puede sostener):

$$
\frac{dP}{dt} = r \cdot P(t) \left(1 - \frac{P(t)}{K}\right)
$$

Solución analítica: $P(t) = \dfrac{K}{1 + \left(\frac{K - P_0}{P_0}\right)e^{-rt}}$

---

#### Aplicación del Cálculo Integral

El Teorema Fundamental del Cálculo establece que la integral definida de la tasa de cambio en un intervalo $[a, b]$ nos da el cambio neto de la población:

$$
\Delta P = \int_{a}^{b} \frac{dP}{dt} \, dt = P(b) - P(a)
$$

En el programa:
- Modelo exponencial: integramos $f_1(t) = r \cdot P_0 \cdot e^{rt}$
- Modelo logístico: integramos $f_2(t) = r \cdot P(t) \cdot \left(1 - \frac{P(t)}{K}\right)$

El método de Romberg aproxima el valor numérico de estas integrales definidas con la precisión solicitada.

---

#### Parámetros del programa

| Parámetro | Valor por defecto | Descripción |
|-----------|-------------------|-------------|
| $P_0$ | 100.0 | Población inicial (individuos en $t=0$) |
| $r$ | 0.05 | Tasa de crecimiento intrínseca (5% por unidad de tiempo) |
| $K$ | 1000.0 | Capacidad de carga (máximo de individuos) |
| Intervalo | $[0, 10]$ | Tiempo de simulación |
| Precisión | 6 dígitos | Tolerancia del método de Romberg |

---

#### Ejemplo de salida

Para el modelo logístico en $t \in [0, 10]$, con 4 subintervalos iniciales y 6 dígitos de precisión:

Tabla de Romberg (j=0: Trapecio, extrapolaciones):   
 k      h      n |    estimaciones ->   
 0      2.5     4 |   11.379842  11.379842  
 1      1.25    8 |   11.408531  11.418094  11.418094  
 2     0.625   16 |   11.415701  11.418091  11.418089  11.418089  




## Fundamentos matemáticos de los módulos destacados
### Módulo 2: Limpiador de señal de telecomunicaciones (filtro pasabajo)

Este software simula un canal de telecomunicaciones analógico contaminado por perturbaciones exógenas de alta frecuencia (ruido determinista). El objetivo es procesar dicha señal continua para recuperar la información útil (voltaje limpio) mediante el modelado digital de un filtro pasabajo RC (Resistor-Capacitor).

#### Fundamentación matemática

**Definición de partición y norma**

Decimos que $x_0, x_1, x_2, \dots, x_n \subseteq [a, b]$ es una partición $P$ de $[a, b]$, si $a = x_0 < x_1 < x_2 < \dots < x_{n-1} < x_n = b$. Al número $\max\{x_i - x_{i-1} : i \in \{1, \dots, n\}\}$ se le llamará la **norma** de $P$ y se denota por $\|P\|$.

**Aplicación algorítmica:** El software inicializa $a = 0.0$ y $b = T_{\text{ventana}}$. Al implementar una partición regular (uniforme), la norma se computa como:

$$
\Delta \tau = \frac{T_{\text{ventana}} - 0}{n_{\text{rectángulos}}}
$$

**Sumas de Riemann por la izquierda**

Si hacemos $x^*_i = x_{i-1}$, tenemos la suma de Riemann:

$$
L_n = \sum_{i=1}^n f(x_{i-1})\Delta x_i
$$

**Aplicación algorítmica:** El motor numérico implementa un bucle iterativo donde el punto muestral es desplazado al extremo izquierdo de cada subintervalo a través de la expresión $\tau_i = a + i \cdot \Delta \tau$.

**Teorema de existencia de la integral**

Si $f : [a, b] \rightarrow \mathbb{R}$ es una función continua en $[a, b]$, entonces $f$ es integrable en $[a, b]$.

**Aplicación algorítmica:** Nuestra función integrando está dada por el producto físico $f(\tau) = s(\tau) \cdot h(\tau)$, donde:

- Señal de entrada (contaminada): $s(\tau) = \sin(\tau) + 0.4\cos(20\tau)$
- Filtro pasabajo: $h(\tau) = e^{-\tau}$

Al ser funciones trigonométricas y exponenciales, ambas son continuas en $\mathbb{R}$. Por el teorema de existencia, se garantiza que el algoritmo numérico converge a un único valor real estable cuando la precisión tiende a infinito ($n \rightarrow \infty$).

**Integral impropia en intervalos no acotados**

$$
\int_{a}^{\infty} f \, dx = \lim_{t \rightarrow \infty} \int_{a}^{t} f \, dx
$$

**Aplicación algorítmica:** Un canal físico analógico transmite indefinidamente ($b \rightarrow \infty$). Sin embargo, las limitaciones de memoria exigen truncar la integración. Dado que el filtro decrece asintóticamente rápido hacia cero ($e^{-10} \approx 0.000045$), la masa residual de la integral impropia desde $T$ hasta el infinito es despreciable, validando el acotamiento de la ventana en un intervalo compacto $[0, T]$.

#### Parámetros de entrada del programa

| Parámetro | Rango | Descripción |
|-----------|-------|-------------|
| Instante de tiempo $t$ | $[0, 10]$ | Momento del muestreo físico en segundos |
| Ancho de la ventana $T$ | $[0.1, 20]$ | Intervalo cerrado de análisis para la sumatoria de Riemann |
| Precisión $n$ | $[10, 2000]$ | Número de rectángulos que subdividirán la señal continua |

#### Salida del programa

1. Despliegue en terminal de las definiciones y teoremas de integrabilidad
2. Cálculo analítico con el voltaje neto limpio en formato decimal
3. Ventana gráfica con las curvas continuas y las barras discretizadas de Riemann

---

### Módulo 3: Simulación de montaña rusa

El software calcula la longitud total de la vía de una montaña rusa ficticia mediante el concepto de **longitud de arco**. Partiendo del teorema de Pitágoras para un segmento infinitesimal:

$$
L = \sqrt{(\Delta x)^2 + (\Delta y)^2} = \sqrt{1 + \left(\frac{\Delta y}{\Delta x}\right)^2} \, \Delta x
$$

Al tomar el límite cuando $\Delta x \to 0$, la sumatoria se convierte en la integral definida:

$$
L = \int_a^b \sqrt{1 + [f'(x)]^2} \, dx
$$

El programa implementa numéricamente esta integral mediante un bucle que:
1. Divide el intervalo $[a, b]$ en $n$ segmentos de ancho $\Delta x = (b-a)/n$
2. Calcula la altura en los puntos $x_i$ y $x_{i+1}$
3. Acumula la longitud de cada segmento usando Pitágoras

El usuario puede elegir entre distintos perfiles de curva (caída libre exponencial o sección ondulada senoidal), definir el intervalo de la pista y el número de subdivisiones para la aproximación.

---

### Módulo 4: Análisis de potencia RMS

Este módulo demuestra matemáticamente por qué una señal con distorsión entrega más potencia que una señal limpia, incluso cuando ambas tienen el mismo volumen máximo amplitud $A$.

**Señal limpia (onda senoidal):**

$$
v_1(t) = A \sin(t)
$$

$$
V_{RMS, limpia} = \sqrt{\frac{1}{2\pi} \int_0^{2\pi} [A \sin(t)]^2 \, dt} = \frac{A}{\sqrt{2}} \approx 0.707A
$$

**Señal distorsionada (onda cuadrada):**

$$
v_2(t) = \begin{cases} 
A & \text{si } 0 \leq t < \pi \\
-A & \text{si } \pi \leq t < 2\pi 
\end{cases}
$$

$$
V_{RMS, dist} = \sqrt{\frac{1}{2\pi} \left( \int_0^{\pi} A^2 \, dt + \int_{\pi}^{2\pi} (-A)^2 \, dt \right)} = A
$$

**Conclusión:** La señal distorsionada entrega aproximadamente un **41% más de potencia** que la señal limpia, lo que explica físicamente por qué el sonido con alta ganancia es más denso y agresivo.

### Módulo 5: Excedente del consumidor (tarifas CFE)

#### Fundamentación matemática

**Definición de partición**

Una colección $P = \{t_0, t_1, \dots, t_n\} \subset [a, b] \subset \mathbb{R}$ con $a < b$, $n \in \mathbb{N}$, $t_0 = a$, $t_n = b$ y $\forall i \in \{1, \dots, n\}: t_{i-1} < t_i$ se llama **partición** de $[a, b]$.

**Norma de una partición**

$$||P|| = \max \{ |t_i - t_{i-1}| : i \in \{1, \dots, n\} \}$$

**Suma de Riemann**

Sea $f: [a, b] \to \mathbb{R}$ acotada, $P = \{t_0, \dots, t_n\}$ una partición y $A = \{S_1, \dots, S_n\}$ una selección de $P$ (con $t_{i-1} \leq S_i \leq t_i$). La suma de Riemann es:

$$S(f, P, A) = \sum_{i=1}^{n} f(S_i)(t_i - t_{i-1})$$

**Integral de Riemann**

Si $f: [a, b] \to \mathbb{R}$ es acotada y existe el límite:

$$\lim_{||P|| \to 0} S(f, P, A) = L$$

entonces $f$ es Riemann integrable y:

$$\int_a^b f(x) \, dx := \lim_{||P|| \to 0} S(f, P, A)$$

**Área entre curvas**

Si $f, g: [a, b] \to \mathbb{R}$ son Riemann integrables, el área comprendida entre sus gráficas es:

$$A(D) = \int_a^b |f(x) - g(x)| \, dx$$

**Teorema Fundamental del Cálculo**

Sea $f: [a, b] \to \mathbb{R}$ integrable con primitiva $F$, entonces:

$$\int_a^b f(t) \, dt = F(b) - F(a)$$

---

#### Planteamiento del problema

El excedente del consumidor mide el beneficio que obtienen los consumidores al pagar un precio menor al que estaban dispuestos a pagar. Geométricamente, corresponde al área entre la curva de demanda $D(q)$ y la recta del precio de mercado $p_0$:

$$EC = \int_0^{Q_0} [D(q) - p_0] \, dq$$

donde $Q_0$ es el consumo de equilibrio ($D(Q_0) = p_0$).

**Caso CFE:** La Comisión Federal de Electricidad aplica tarifas reguladas por kWh. Los consumidores valoran más los primeros kilowatts-hora (refrigeración, iluminación básica) que los últimos. Esta diferencia entre el valor percibido y el precio pagado representa un beneficio económico real.

**Pregunta de investigación:** ¿Cómo cuantificar el beneficio económico que recibe un hogar mexicano con tarifa PDBT a partir de los datos de su recibo de luz, utilizando sumas de Riemann e integral definida?

---

#### Modelo matemático propuesto

Como no existe una función de demanda pública de CFE, se propone un modelo que cumple con las propiedades típicas: decreciente, convexa, y que en el punto de equilibrio iguala al precio de mercado:

$$D(q) = p_m \cdot \left(1 + 2\left(1 - \frac{q}{Q_{max}}\right)^2\right)$$

donde:
- $p_m$ = cargo variable real del recibo ($/kWh)
- $Q_{max} = 800$ kWh (consumo máximo considerado)

**Cálculo del punto de equilibrio:** Se resuelve numéricamente $D(Q_0) = p_m$ mediante búsqueda binaria en $[0, Q_{max}]$.

**Suma de Riemann para el excedente:** Dividiendo $[0, Q_0]$ en $n$ subintervalos de ancho $\Delta q = Q_0/n$ y tomando el punto izquierdo:

$$S_n = \sum_{i=1}^{n} [D(q_i) - p_m] \cdot \Delta q$$

Por el teorema de existencia (toda función continua es Riemann integrable), cuando $n \to \infty$ (norma de partición $\to 0$), $S_n \to EC$.

---

#### Implementación computacional

El programa permite:

1. **Modelar** matemáticamente la función de demanda para consumo eléctrico residencial
2. **Calcular** numéricamente el excedente usando sumas de Riemann con precisión variable ($n = 100, 500, 2000, 10000$)
3. **Ingresar** datos del recibo (consumo, precio por kWh, total a pagar)
4. **Visualizar** gráficamente el área del excedente (región entre la curva de demanda y la recta del precio)
5. **Interpretar** los resultados como beneficio económico mensual

#### Gráfica generada

| Elemento | Descripción |
|----------|-------------|
| Curva azul | Demanda $D(q)$ |
| Línea roja horizontal | Precio de mercado $p_m$ |
| Línea naranja punteada vertical | Consumo mensual real del usuario |
| Área sombreada en verde | Excedente del consumidor |
| Rectángulos verdes translúcidos | Representación visual de la suma de Riemann |









---

# Requisitos
- Python 3.8 o superior
- Pip (gestor de paquetes)

# Instalación paso a paso
## 1. Verificar/Instalar Python
**WINDOWS**

Verificar versión de Python
```cmd
python --version
```
Si no está instalado, descargar desde: https://www.python.org/downloads/

IMPORTANTE: Marcar "Add Python to PATH" durante la instalación

**LINUX**

Verificar versión de Python
```bash
python3 --version
```

Si no está instalado:
```bash
sudo apt update
sudo apt install python3 python3-pip python3-venv
```
## 2. Verificar/Instalar librerias
**WINDOWS**
Verifica que las librarias esten
```cmd
python -c "import numpy, matplotlib, sympy; print('Estan todas las librerías')"
```
De lo contrario se instala asi
```cmd
pip install numpy matplotlib sympy
```

**LINUX**
Verifica que las librarias esten
```bash
python3 -c "import numpy, matplotlib, sympy; print('Estan todas las librerías ')"
```
De lo contrario se instala asi
```bash
pip3 install numpy matplotlib sympy
```
## 📥 Descargar el proyecto

[ Descargar ProyectoFinal](https://github.com/ariel-ge/ProyectoCalculo/blob/ariel-ge-patch-1/ProyectoFinal.py)

---
## 3 Ejecutar el proyecto

**WINDOWS**
```cmd
python ProyectoFinal.py
```
**LINUX**
```bash
python3 ProyectoFinal.py
```


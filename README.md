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


## Fundamentos matemáticos de los módulos destacados
### Módulo 2: Limpiador de señal de telecomunicaciones (filtro pasabajo)

Este software simula un canal de telecomunicaciones analógico contaminado por perturbaciones exógenas de alta frecuencia (ruido determinista). El objetivo es procesar dicha señal continua para recuperar la información útil (voltaje limpio) mediante el modelado digital de un filtro pasabajo RC (Resistor-Capacitor).

#### Fundamentación matemática

**Definición de partición y norma**

Decimos que $x_0, x_1, x_2, \dots, x_n \subseteq [a, b]$ es una partición $P$ de $[a, b]$, si $a = x_0 < x_1 < x_2 < \dots < x_{n-1} < x_n = b$. Al número $\max\{x_i - x_{i-1} : i \in \{1, \dots, n\}\}$ se le llamará la **norma** de $P y se denota por $\|P\|$.

**Aplicación algorítmica:** El software inicializa $a = 0.0$ y $b = T_{\text{ventana}}$. Al implementar una partición regular (uniforme), la norma se computa como:

$$
\Delta \tau = \frac{T_{\text{ventana}} - 0}{n_{\text{rectángulos}}}
$$

**Sumas de Riemann por la izquierda**

Si hacemos \(x^*_i = x_{i-1}\), tenemos la suma de Riemann:

\[
L_n = \sum_{i=1}^n f(x_{i-1})\Delta x_i
\]

**Aplicación algorítmica:** El motor numérico implementa un bucle iterativo donde el punto muestral es desplazado al extremo izquierdo de cada subintervalo a través de la expresión \(\tau_i = a + i \cdot \Delta \tau\).

**Teorema de existencia de la integral**

Si \(f : [a, b] \rightarrow \mathbb{R}\) es una función continua en \([a, b]\), entonces \(f\) es integrable en \([a, b]\).

**Aplicación algorítmica:** Nuestra función integrando está dada por el producto físico \(f(\tau) = s(\tau) \cdot h(\tau)\), donde:

- Señal de entrada (contaminada): \(s(\tau) = \sin(\tau) + 0.4\cos(20\tau)\)
- Filtro pasabajo: \(h(\tau) = e^{-\tau}\)

Al ser funciones trigonométricas y exponenciales, ambas son continuas en \(\mathbb{R}\). Por el teorema de existencia, se garantiza que el algoritmo numérico converge a un único valor real estable cuando la precisión tiende a infinito (\(n \rightarrow \infty\)).

**Integral impropia en intervalos no acotados**

\[
\int_{a}^{\infty} f \, dx = \lim_{t \rightarrow \infty} \int_{a}^{t} f \, dx
\]

**Aplicación algorítmica:** Un canal físico analógico transmite indefinidamente (\(b \rightarrow \infty\)). Sin embargo, las limitaciones de memoria exigen truncar la integración. Dado que el filtro decrece asintóticamente rápido hacia cero (\(e^{-10} \approx 0.000045\)), la masa residual de la integral impropia desde \(T\) hasta el infinito es despreciable, validando el acotamiento de la ventana en un intervalo compacto \([0, T]\).

**Parámetros de entrada del programa**

| Parámetro | Rango | Descripción |
|-----------|-------|-------------|
| Instante de tiempo \(t\) | \([0, 10]\) | Momento del muestreo físico en segundos |
| Ancho de la ventana \(T\) | \([0.1, 20]\) | Intervalo cerrado de análisis para la sumatoria de Riemann |
| Precisión \(n\) | \([10, 2000]\) | Número de rectángulos que subdividirán la señal continua |

**Salida del programa**

1. Despliegue en terminal de las definiciones y teoremas de integrabilidad
2. Cálculo analítico con el voltaje neto limpio en formato decimal
3. Ventana gráfica con las curvas continuas y las barras discretizadas de Riemann




### Módulo 3: Simulación de montaña rusa

El software calcula la longitud total de la vía de una montaña rusa ficticia mediante el concepto de **longitud de arco**. Partiendo del teorema de Pitágoras para un segmento infinitesimal:


El programa implementa numéricamente esta integral mediante un bucle que:
1. Divide el intervalo [a, b] en n segmentos de ancho dx = (b-a)/n
2. Calcula la altura en los puntos x_i y x_{i+1}
3. Acumula la longitud de cada segmento usando Pitágoras

El usuario puede elegir entre distintos perfiles de curva (caída libre exponencial o sección ondulada senoidal), definir el intervalo de la pista y el número de subdivisiones para la aproximación.

### Módulo 4: Análisis de potencia RMS 

Este módulo demuestra matemáticamente por qué una señal  con distorsión entrega más potencia que una señal limpia, incluso cuando ambas tienen el mismo volumen máximo amplitud \A.

**Señal limpia (onda senoidal):**
\[
v_1(t) = A \sen(t)
\]
\[
V_{RMS, limpia} = \sqrt{\frac{1}{2\pi} \int_0^{2\pi} [A \sen(t)]^2 \, dt} = \frac{A}{\sqrt{2}} \approx 0.707A
\]

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

[ Descargar ProyectoFinal](https://tu-link.com/ProyectoFinal.zip)

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


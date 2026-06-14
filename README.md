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
| 2 | Limpiador de señal | Convolución + Sumas de Riemann |
| 3 | Simulación montaña rusa | Longitud de arco |
| 4 | Análisis RMS guitarra | Valor eficaz (integral) |
| 5 | Excedente del consumidor CFE | Área bajo la curva |

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

## Ejecutar el proyecto

**WINDOWS**
```cmd
python ProyectoFinal.py
```
**LINUX**
```bash
python3 ProyectoFinal.py
```


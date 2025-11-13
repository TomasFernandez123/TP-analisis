# 🧮 TP IAD — Comparación Aglomerados 13 (Gran Córdoba) vs 32 (CABA) — 2016–2025

## 📘 Descripción general
Este proyecto forma parte del **Segundo Parcial** de la materia **Introducción al Análisis de Datos** (UTN FRA).

El objetivo es analizar la evolución de las **tasas de actividad, empleo, desocupación e ingresos reales** de la población para los aglomerados **13 (Gran Córdoba)** y **32 (CABA)** durante el período **2016–2025**, utilizando los **microdatos de la EPH (INDEC)**.

---

## 🧭 Estructura del proyecto

TP_IAD/
├── data/
│ ├── raw/ # Microdatos EPH Personas (2016–2025)
│ ├── interim/ # Muestras filtradas o intermedias
│ └── processed/ # Resultados listos para el informe
├── src/
│ ├── 00_setup.ipynb # Prueba inicial (lectura, filtros, muestra 2024T3)
│ ├── tasas.ipynb # Cálculo tasas actividad, empleo, desocupación
│ ├── ingresos.ipynb # (Próximo) Deflactación P21 y análisis ingresos reales
│ ├── multivariado.ipynb # (Próximo) Cortes por sexo, edad, educación
│ └── imputacion.ipynb # (Próximo) Modelo de imputación y diagnóstico
├── output/
│ ├── tabla_comparativa_anual_2016_2025_aglo13_32.csv
│ ├── graf_actividad_anual_13_32.png
│ ├── graf_empleo_anual_13_32.png
│ ├── graf_desocupacion_anual_13_32.png
│ └── informe_TP_IAD.ipynb # (opcional) plantilla exportable a PDF
└── README.md

yaml
Copiar código

---

## ⚙️ Requisitos

- **Python 3.10+**
- Librerías:
  ```bash
  pip install pandas numpy matplotlib
  # Opcional para Parquet:
  pip install pyarrow  # o fastparquet
Editor: VS Code o Jupyter Lab

📥 Descarga de datos
Acceder a EPH - Microdatos (INDEC)

Descargar los microdatos de Personas (base individual) de los 4 trimestres por año (2016–2025).

Guardarlos tal cual vienen en data/raw/.

⚠️ No es necesario descargar la base Hogares (solo se usa más adelante si se analiza IPCF).

▶️ Flujo de trabajo
1️⃣ Lectura inicial (00_setup.ipynb)
Carga un archivo de prueba (ejemplo: 3T-2024)

Normaliza columnas a mayúsculas

Filtra los aglomerados 13 y 32

Guarda un .csv.gz intermedio en data/interim/

2️⃣ Cálculo de tasas (tasas.ipynb)
Lee todos los archivos de data/raw/

Base poblacional: personas de 10 años o más

Ponderador: PONDERA

Calcula:

Tasa de actividad = (ocupados + desocupados) / población 10+

Tasa de empleo = ocupados / población 10+

Tasa de desocupación = desocupados / fuerza laboral

Agregado anual ponderado

Resultados:

bash
Copiar código
data/processed/tasas_trimestrales_2016_2025_aglo13_32.csv
data/processed/tasas_anuales_2016_2025_aglo13_32.csv
Figuras generadas:

bash
Copiar código
output/graf_actividad_anual_13_32.png
output/graf_empleo_anual_13_32.png
output/graf_desocupacion_anual_13_32.png
3️⃣ Análisis de ingresos reales (ingresos.ipynb)
Une P21 con serie IPC (base diciembre por año)

Calcula P21_real

Genera tabla anual por aglomerado:

Media, mediana, P25–P75 (IQR), D1, D5, D9

Gráfico: evolución de mediana P21 real (13 vs 32)

4️⃣ Análisis multivariado (multivariado.ipynb)
Cortes sugeridos:

Sexo (CH04)

Edad (CH06, agrupada)

Nivel educativo (NIVEL_ED)

(Opcional) Rama (PP04B_COD) y Ocupación (PP04D_COD)

Gráficos de barras y boxplots comparativos

5️⃣ Modelo de imputación (imputacion.ipynb)
Modelo para no respuesta de P21

Usar regresión lineal o árbol de decisión

Variables predictoras: edad, sexo, educación, condición laboral, aglomerado

Reportar:

R² / MAE

Diagnóstico de residuos

Variables con mayor influencia

6️⃣ Informe final
Documento PDF (6–10 páginas) con:

Introducción y objetivos

Metodología

Resultados de tasas (tabla + gráficos)

Resultados de ingresos reales (tabla + gráfico)

Análisis multivariado

Modelo de imputación (si aplica)

Conclusiones

Anexo técnico (supuestos, fuentes, ponderadores, código)

🧩 Validaciones clave
Variable	Descripción	Notas
ESTADO	Condición de actividad (1=Ocupado, 2=Desocupado, 3=Inactivo, 4=<10 años)	Excluir 4
PONDERA	Ponderador individual	Para tasas
P21	Ingreso de ocupación principal	Para ingresos reales
AGLOMERADO	13 = Gran Córdoba, 32 = CABA	Claves del análisis

🧠 Base: población de 10 años y más
🧮 Agregado anual: promedio ponderado por población 10+ del trimestre

📊 Entregables esperados
 Tabla comparativa anual de tasas

 3 gráficos (actividad, empleo, desocupación)

 Tabla de ingresos reales P21

 Gráfico de mediana P21 real

 Gráficos multivariados

 Modelo de imputación

 Mapa georreferenciado

 Informe PDF final

👥 Equipo
Integrante	Rol
Tomás Fernández	Pipeline de tasas, estructura del proyecto, coordinación
Agustín González	Ingresos P21 real, análisis multivariado
Mariano Pastor	Modelo de imputación, mapa y redacción final PDF

🗺️ Roadmap
 Limpieza y filtrado inicial (3T-2024)

 Cálculo de tasas y comparación anual

 Ingresos P21 real (deflactación + tabla + figura)

 Análisis multivariado

 Imputación de no respuesta

 Mapas y pulido del informe

💡 Consejos
Usar siempre nombres de columnas en mayúsculas.

No incluir microdatos originales en repos públicos (peso + licencia).

Documentar supuestos: base 10+, IPC base diciembre, ponderador.

Pies de figura y tabla:

Fuente: EPH-INDEC. Elaboración propia.

📚 Referencias
INDEC – EPH Microdatos

Diseño de Registro EPH (2024)

Última actualización: noviembre 2025
Cátedra: Luis Fernández – UTN FRA, División 141

TP IAD — Comparación Aglos 13 (Gran Córdoba) vs 32 (CABA) — 2016–2025
📌 Objetivo

Informe (6–10 páginas) con:

Evolución de tasa de actividad, empleo y desocupación.

Ingresos con inflación considerada (P21 real como mínimo).

Gráficos y tablas comparando aglomerados 13 y 32.

(Plus para nota alta) Multivariado, modelo de imputación de no respuesta en ingresos y mapa.

Fuente: EPH – INDEC (Personas), trimestres 2016–2025.

🧭 Estructura de carpetas
TP_IAD/
  data/
    raw/          # Microdatos EPH Personas tal como se descargan (2016–2025)
    interim/      # Muestras/archivos intermedios (p.ej., 2024T3 aglo 13/32)
    processed/    # Resultados tabulares listos para el informe (tasas, P21 real, etc.)
  src/
    00_setup.ipynb          # Lectura archivo de prueba (3T-2024), filtro aglo 13/32
    <notebooks de trabajo>  # Cálculos de tasas, P21 real, multivariado, etc.
  output/
    tabla_comparativa_anual_2016_2025_aglo13_32.csv
    graf_actividad_anual_13_32.png
    graf_empleo_anual_13_32.png
    graf_desocupacion_anual_13_32.png
    informe_TP_IAD.ipynb    # (opcional) plantilla para exportar a PDF


Importante: Evitar rutas duplicadas tipo data/raw/data/raw. Si pasa, corregir moviendo los archivos a data/raw/ real.

🛠️ Requisitos (mínimos)

Python 3.10+

Paquetes:

pandas, numpy, matplotlib

(Opcional) pyarrow o fastparquet si guardamos Parquet

Editor: Jupyter/VS Code

Instalación rápida:

pip install pandas numpy matplotlib
# Opcional parquet
pip install pyarrow    # o: pip install fastparquet

🔽 Descarga de datos (equipo)

Ir a INDEC → EPH → Microdatos Personas (2016–2025).

Descargar los 4 trimestres por año (base Personas) y guardarlos sin renombrar en data/raw/.

No bajar base Hogares por ahora (se usa más adelante para IPCF si aplica).

Los nombres pueden variar (usu_individual_Txyz.txt, etc.). El pipeline detecta patrones como individu, person, usu_individual, personas.

▶️ Flujo de trabajo (resumen)

Prueba de lectura (una muestra):

Abrir src/00_setup.ipynb.

Cargar un archivo de 3T-2024 desde data/raw/.

Normalizar columnas a mayúsculas y filtrar aglo 13 y 32.

Guardar intermedio en data/interim/ (*.csv.gz si no hay engine Parquet).

Tasas (actividad, empleo, desocupación):

Notebook de tasas: lee todos los data/raw/, filtra 13/32.

Base: población 10+ años (excluye ESTADO=4).

Ponderador: PONDERA.

Calcula trimestral y agrega anual ponderado (promedio ponderado por población 10+ trimestral).

Guarda:

data/processed/tasas_trimestrales_2016_2025_aglo13_32.csv

data/processed/tasas_anuales_2016_2025_aglo13_32.csv

Figuras y tabla comparativa (para el informe):

Generar en output/:

tabla_comparativa_anual_2016_2025_aglo13_32.csv

graf_actividad_anual_13_32.png

graf_empleo_anual_13_32.png

graf_desocupacion_anual_13_32.png

Ingresos P21 real (próximo paso del equipo):

Unir con IPC (base diciembre por año o base única si lo pide el profe).

Calcular P21_real.

Tabla anual por aglo (media, mediana, P25–P75, deciles).

Gráfico: mediana P21 real 13 vs 32.

Multivariado (para nota alta):

Cortes por sexo (CH04), edad (grupos a definir), nivel educativo (NIVEL_ED).

Opcional: rama (PP04B_COD) y ocupación (PP04D_COD).

Modelo de imputación (nota 9–10):

Regresión sobre log(P21) para no respuesta (o árbol).

Reportar R²/MAE, diagnóstico breve y variables más influyentes.

Mapa (opcional + puntos):

Mapa simple (por 2–3 años) con tasa de desocupación o mediana P21 real por aglo (en nuestro caso 13 vs 32, sirve igual como evidencia geográfica).

🧪 Validaciones clave

ESTADO:

1 Ocupado, 2 Desocupado, 3 Inactivo, 4 <10 años, 0/NA No respuesta.

Base de tasas = 10+ años (excluye 4).

Tasas:

Actividad = (1 + 2) / 10+

Empleo = 1 / 10+

Desocupación = 2 / (1 + 2)

Agregado anual: promedio ponderado por población 10+ del trimestre.

Ponderador: PONDERA (para tasas); para ingresos se usan los ponderadores específicos si la cátedra lo exige (más adelante).

🙋 Roles sugeridos (reparto ágil)

Tomas: pipeline de tasas + gráficos, armado de tabla comparativa.

Agustín: ingresos P21 real (deflactación + tabla/figura) y multivariado (sexo/edad/educación).

Mariano: modelo de imputación + mapa y editor del PDF (estructura, pies de figura y conclusiones).

(Roles intercambiables según disponibilidad.)

📝 Entregable (PDF)

Secciones mínimas:

Introducción (qué, por qué, fuente EPH).

Metodología (población 10+, PONDERA, fórmulas de tasas, agregado anual, criterio de deflactación).

Resultados — Tasas (tabla comparativa + 3 figuras + 4–6 bullets).

Resultados — Ingresos P21 real (tabla + 1 figura + lectura).

Multivariado (cortes y hallazgos).

(Opcional) Modelo de imputación (métrica + interpretación).

(Opcional) Mapa.

Conclusiones (5 bullets).

Anexo técnico (detalle de variables usadas, supuestos, links a fuentes).

🧩 Tips y problemas comunes

Ruta duplicada data/raw/data/raw: mover archivos a data/raw/ real y reintentar.

Parquet falla: usar CSV comprimido (to_csv(..., compression="gzip")) o instalar pyarrow/fastparquet.

Encoding: EPH suele venir latin-1 y separador ;.

Columnas cambiantes (177 vs 235): no pasa nada; usamos columnas mínimas comunes.

🔒 Buenas prácticas

No commitear microdatos al repo público (peso/licencias).

Sí commitear código/notebooks y archivos generados pequeños (output/, processed/ si son livianos).

Documentar supuestos (base 10+, IPC base diciembre, etc.).

Pies de figura/tabla: “Fuente: EPH-INDEC. Elaboración propia.”

🗺️ Roadmap (lo que falta)

 P21 real (deflactación + tabla y mediana anual por aglo).

 Multivariado (sexo/edad/educación; opcional rama/ocupación).

 Modelo de imputación y diagnóstico.

 Mapa (si hay tiempo).

 Redacción final y exportar PDF.

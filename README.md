# 🥔 ¿Pudo preverse la crisis del sector papero colombiano de 2025?

> Pipeline de análisis exploratorio y descriptivo sobre la cadena productiva de la papa en Colombia y su contexto mundial, orientado a la detección temprana de señales de crisis sectorial mediante datos abiertos.

[![Python](https://img.shields.io/badge/Python-3.12-blue.svg)](https://www.python.org/)
[![pandas](https://img.shields.io/badge/pandas-2.x-150458.svg)](https://pandas.pydata.org/)
[![scipy](https://img.shields.io/badge/scipy-1.x-8CAAE6.svg)](https://scipy.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Completed-brightgreen.svg)]()

---

## 📌 Contexto

Durante 2025, Colombia atravesó una crisis profunda en su sector papero: los precios pagados al productor cayeron muy por debajo de los costos de producción, se desencadenaron paros agrarios en Boyacá y Nariño, y el Gobierno nacional intervino con compras directas que generaron nuevas distorsiones en la cadena de comercialización.

Este proyecto formula y responde una pregunta concreta a partir de datos públicos:

> **¿Estaban presentes en los datos señales de alerta temprana que hubieran permitido anticipar la crisis?**

**Spoiler:** sí. Y el bootstrap del margen anual lo evidencia desde 2020.

---

## 🎯 Objetivos del análisis

1. Integrar fuentes heterogéneas (mundial, nacional, departamental, mensual) y gestionar las imperfecciones típicas de los datos agropecuarios reales.
2. Aplicar el flujo completo de EDA: desde la limpieza hasta la inferencia con simulación.
3. Convertir descripción estadística en razonamiento contrafáctico: pasar de *"qué pasó"* a *"qué pudo haber pasado bajo decisiones diferentes"*.

---

## 🔬 Pipeline de análisis

El script principal `scripts/analisis_papa.py` implementa 8 fases secuenciales:

| Fase | Técnica | Salida clave |
|------|---------|--------------|
| 1 | Importación y diagnóstico de calidad | Detección de NaN, duplicados, errores de tipeo |
| 2 | Limpieza y preparación | Estandarización, deduplicación, imputación, interpolación lineal |
| 3 | Resúmenes numéricos | Tendencia central, dispersión, asimetría, curtosis, CV |
| 4 | Gráficas univariadas | Histograma + KDE, boxplot por año, barras horizontales |
| 5 | Gráficas bivariadas | Dispersión con codificación temporal, doble eje precio/importaciones |
| 6 | Exploración multivariada | Correlograma, dendograma jerárquico (Ward), gráfico de burbujas |
| 7 | Bootstrapping | IC 95% del margen anual — 5.000 remuestreos por año |
| 8 | Monte Carlo + tests | 10.000 simulaciones por escenario, t-Welch, Spearman, Kendall, Pearson |

---

## 📊 Datos

El análisis integra **cinco fuentes públicas** consolidadas en CSV, generadas por `scripts/construir_dataset.py`:

| Archivo | Granularidad | Período | Fuente original |
|---------|--------------|---------|-----------------|
| `produccion_mundial_papa.csv` | País × año — 30 países | 2018–2024 | FAOSTAT / Our World in Data |
| `produccion_departamental_colombia.csv` | Departamento × año — 10 deptos | 2018–2025 | EVA – MinAgricultura / Fedepapa |
| `precios_mensuales_papa.csv` | Mes × año | 2018–2025 | SIPSA-DANE / Observatorio FNFP |
| `comercio_exterior_papa.csv` | Año | 2018–2025 | Legiscomex / Fedepapa |
| `indicadores_nacionales.csv` | Panel anual consolidado | 2018–2025 | Síntesis de las anteriores |

> ⚠️ **Nota sobre los datos:** los valores están calibrados con cifras publicadas por FAOSTAT, Fedepapa, MinAgricultura, DANE-SIPSA y prensa especializada (2018–2025). El dataset incluye deliberadamente NaN, duplicados y errores de tipeo para reproducir el desafío real de trabajar con fuentes agropecuarias heterogéneas — el script de construcción documenta cada decisión de diseño.

---

## 🏗️ Estructura del repositorio

```
.
├── README.md
├── requirements.txt
├── .gitignore
├── Informe_Analisis_Crisis_Papera_Colombia.pdf  ← Informe ejecutivo (5 páginas)
├── scripts/
│   ├── construir_dataset.py    ← Genera los 5 CSVs con semilla fija
│   ├── analisis_papa.py        ← Pipeline EDA completo
│   └── generar_informe.py      ← Compila el PDF con reportlab
├── datos/
│   ├── produccion_mundial_papa.csv
│   ├── produccion_departamental_colombia.csv
│   ├── precios_mensuales_papa.csv
│   ├── comercio_exterior_papa.csv
│   └── indicadores_nacionales.csv
└── figuras/
    ├── 01_univariada_precios.png
    ├── 02_univariada_barras_deptos.png
    ├── 03_bivariada_precio_vs_costo.png
    ├── 04_bivariada_precio_vs_importaciones.png
    ├── 05_correlograma.png
    ├── 06_dendograma_paises.png
    ├── 07_burbujas_deptos.png
    ├── 08_bootstrap_margen.png
    ├── 09_simulacion_montecarlo.png
    ├── 10_panel_alerta_temprana.png
    └── tabla_resumen_numerico.csv
```

---

## 🚀 Reproducibilidad

### Instalación
```bash
git clone https://github.com/<usuario>/analisis-crisis-papera-2025.git
cd analisis-crisis-papera-2025
pip install -r requirements.txt
```

### Ejecución
```bash
# 1. Construir los datasets (idempotente — semilla np.random.seed(42))
python scripts/construir_dataset.py

# 2. Correr el pipeline completo y generar las figuras
python scripts/analisis_papa.py

# 3. (Opcional) Regenerar el informe PDF
python scripts/generar_informe.py
```

Todas las operaciones aleatorias usan semilla fija: los resultados son **100% reproducibles**.

---

## 🔍 Hallazgos principales

### 1. Volatilidad estructural del margen
El **coeficiente de variación del margen mensual es 1.062%**, una cifra incompatible con cualquier planeación de ciclos productivos de 4–6 meses. La distribución de precios presenta asimetría positiva (+1,13): existieron episodios extremos al alza (2022–2023) y a la baja (2025) que una lectura basada solo en la media habría invisibilizado.

### 2. Concentración geográfica como amplificador de riesgo
Cundinamarca + Boyacá + Nariño concentran ~85% de la producción nacional. Cualquier choque local en estos tres departamentos tiene impacto sistémico en la oferta del país.

### 3. Sustitución estructural detectada en el correlograma
- Producción nacional vs. importaciones: **r = −0,69**
- Producción nacional vs. exportaciones: **r = −0,80**

La correlación negativa entre importaciones de papa precocida y producción doméstica es una señal estructural identificable años antes del colapso de precios.

### 4. El dendograma ubica a Colombia en el cluster correcto
El agrupamiento jerárquico con linkage de Ward sobre patrones temporales estandarizados (z-score por país) ubica a Colombia junto a México y Ucrania — los tres con trayectorias decrecientes — mientras China, India, Bangladesh y Pakistán forman un cluster ascendente que sostiene la presión bajista en los precios internacionales.

### 5. Bootstrap: alertas estadísticas desde 2020
- En **2020 y 2021**, el IC 95% del margen anual estuvo *completamente* por debajo de cero.
- En **2024**, volvió a cruzar el eje, anticipando el colapso de 2025.
- La ventana de intervención oportuna era 2021, no agosto de 2025.

### 6. Monte Carlo: cuantificación del contrafáctico

| Escenario | Medida simulada | P(pérdida del productor) |
|-----------|-----------------|--------------------------|
| A | Status quo 2025 | **100%** |
| B | Salvaguardia + antidumping | 99% |
| C | B + planificación de siembras | **36%** |

Solo la combinación de medidas comerciales con regulación de la oferta habría reducido significativamente la probabilidad de pérdida.

### 7. Tests estadísticos confirmatorios
- **T-Welch** — margen 2018–2022 vs 2023–2025: cambio de signo de +$7.201 a −$4.815 por bulto de 50 kg (p < 0,001).
- **Spearman** — margen mensual vs tiempo: ρ = −0,30 (p < 0,01), tendencia decreciente significativa.

---

## 🛠️ Stack técnico

| Librería | Uso en el proyecto |
|----------|--------------------|
| `pandas` | Carga, limpieza, imputación (`groupby().transform()`, `interpolate`), reshaping |
| `numpy` | Operaciones vectoriales, bootstrapping, simulación Monte Carlo |
| `matplotlib` | Histograma, barras, dispersión, doble eje, series temporales |
| `seaborn` | Correlograma (`heatmap`), boxplot estilizado |
| `scipy.stats` | T-Welch, Pearson, Spearman, Kendall |
| `scipy.cluster.hierarchy` | Linkage Ward + dendograma |
| `reportlab` | Maquetación y generación del informe en PDF |

---

## 📚 Fuentes

- **FAOSTAT** — [Our World in Data: potato production](https://ourworldindata.org/grapher/potato-production)
- **Fedepapa** — Federación Colombiana de Productores de Papa
- **Observatorio FNFP** — [observatoriofnfp.com](https://observatoriofnfp.com/home/datos/tableros-de-informacion/)
- **MinAgricultura** — Evaluaciones Agropecuarias Municipales (EVA) / Agronet
- **DANE** — Sistema de Información de Precios y Abastecimiento (SIPSA)
- **Legiscomex** — Estadísticas de comercio exterior

---

## 📄 Informe ejecutivo

El [informe PDF](Informe_Analisis_Crisis_Papera_Colombia.pdf) sintetiza en 5 páginas el problema, la metodología, los resúmenes numéricos, las visualizaciones clave y la discusión de resultados.

---

## 🤝 Contribuciones

Sugerencias para ampliar el período histórico, conectar nuevas fuentes o extender el análisis a otros cultivos transitorios son bienvenidas. Abre un *issue* o envía un *pull request*.

---

## 📜 Licencia

MIT. Los datos derivados de fuentes públicas siguen las condiciones de uso de cada organismo emisor (FAO, DANE, MinAgricultura).

---

> *"En agricultura, los números cuentan la historia mucho antes de que estallen los paros. El problema rara vez es la falta de datos — es la falta de quien los mire a tiempo."*



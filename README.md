# PrevisionCrisisSectorPapero2025
Análisis exploratorio y descriptivo de datos sobre la cadena productiva de la papa en Colombia y su contexto mundial, con énfasis en la detección temprana de señales de crisis sectorial.

# 🥔 ¿Pudo preverse la crisis del sector papero colombiano de 2025?

> Análisis exploratorio y descriptivo de datos sobre la cadena productiva de la papa en Colombia y su contexto mundial, con énfasis en la detección temprana de señales de crisis sectorial.

[![Python](https://img.shields.io/badge/Python-3.12-blue.svg)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Completed-brightgreen.svg)]()

---

## 📌 Contexto

Durante 2025, Colombia atravesó una crisis profunda en su sector papero: los precios pagados al productor cayeron muy por debajo de los costos de producción, se desencadenaron paros agrarios en Boyacá y Nariño, y el Gobierno nacional intervino con compras directas que generaron nuevas distorsiones en la cadena de comercialización.

Este proyecto formula y responde una pregunta concreta:

> **¿Estaban presentes en los datos públicos señales de alerta temprana que hubieran permitido anticipar la crisis?**

**Spoiler:** sí. Y el bootstrap del margen anual lo muestra desde 2020.

---

## 🎯 Objetivos

1. Aplicar el flujo completo de análisis exploratorio y descriptivo de datos sobre un problema económico-agrícola real.
2. Integrar fuentes heterogéneas (mundiales, nacionales, departamentales, mensuales) y manejar las imperfecciones típicas del trabajo con datos agropecuarios.
3. Demostrar las técnicas requeridas: visualización univariada/bivariada, exploración de asociaciones, resúmenes numéricos, bootstrapping y simulación Monte Carlo.
4. Convertir descripción en inferencia: pasar de "qué pasó" a "qué pudo haber pasado bajo decisiones diferentes".

---

## 🔬 Metodología

El análisis sigue 8 fases ejecutadas en `scripts/analisis_papa.py`:

| Fase | Técnica | Salida clave |
|------|---------|--------------|
| 1 | Importación y diagnóstico de calidad | Detección de NaN, duplicados, errores de tipeo |
| 2 | Limpieza y preparación | Estandarización, deduplicación, imputación, interpolación |
| 3 | Resúmenes numéricos | Tendencia central, dispersión, asimetría, curtosis |
| 4 | Gráficas univariadas | Histograma, boxplot, barras horizontales |
| 5 | Gráficas bivariadas | Dispersión con codificación temporal, doble eje |
| 6 | Exploración multivariada | Correlograma, dendograma jerárquico, burbujas |
| 7 | Bootstrapping | IC 95% del margen anual con 5.000 remuestreos |
| 8 | Monte Carlo + tests | 10.000 simulaciones por escenario, t-Welch, Spearman |

---

## 📊 Datasets

El análisis integra **cinco fuentes públicas** consolidadas en CSV:

| Archivo | Granularidad | Período | Fuente original |
|---------|--------------|---------|-----------------|
| `produccion_mundial_papa.csv` | País × año (30 países) | 2018–2024 | FAOSTAT (vía Our World in Data) |
| `produccion_departamental_colombia.csv` | Departamento × año (10 deptos) | 2018–2025 | EVA – MinAgricultura / Fedepapa |
| `precios_mensuales_papa.csv` | Mes × año | 2018–2025 | SIPSA-DANE / Observatorio FNFP |
| `comercio_exterior_papa.csv` | Año | 2018–2025 | Legiscomex / Fedepapa |
| `indicadores_nacionales.csv` | Panel anual consolidado | 2018–2025 | Síntesis de las anteriores |

> ⚠️ **Nota sobre los datos:** los valores fueron calibrados con cifras publicadas oficialmente entre 2018 y 2025 por FAOSTAT, Fedepapa, MinAgricultura, Agronet, DANE y reportes de prensa colombiana (Semana, El Espectador, Infobae). El dataset incluye deliberadamente NaN, duplicados y un error de tipeo para reproducir el desafío real de trabajar con fuentes agropecuarias heterogéneas, como documentan los script de generación.

---

## 🏗️ Estructura del repositorio

```
.
├── README.md                                    ← Este archivo
├── Informe_Analisis_Crisis_Papera_Colombia.pdf  ← Informe ejecutivo (5 páginas)
├── scripts/
│   ├── construir_dataset.py                     ← Genera los 5 CSVs base
│   ├── analisis_papa.py                         ← Pipeline completo de análisis
│   └── generar_informe.py                       ← Compila el PDF con reportlab
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

### Requisitos
```bash
python >= 3.10
pandas
numpy
matplotlib
seaborn
scipy
reportlab     # solo si se quiere regenerar el PDF
pypdf         # solo para verificación del PDF
```

### Instalación
```bash
git clone https://github.com/<usuario>/analisis-crisis-papera-2025.git
cd analisis-crisis-papera-2025
pip install -r requirements.txt
```

### Ejecución
```bash
# 1. Generar los CSVs base (idempotente; semilla fija np.random.seed(42))
python scripts/construir_dataset.py

# 2. Ejecutar el pipeline completo de análisis
python scripts/analisis_papa.py

# 3. (Opcional) Regenerar el informe PDF
python scripts/generar_informe.py
```

Todas las semillas aleatorias están fijadas, por lo que los resultados son **100% reproducibles**.

---

## 🔍 Hallazgos principales

### 1. El margen del productor era estructuralmente inestable
- **Coeficiente de variación del margen mensual: 1.062%** — incompatible con cualquier planeación agrícola realista.
- Asimetría positiva en precios (+1,13): la distribución es heavy-tailed, con episodios extremos al alza (2022–2023) y a la baja (2025).

### 2. La concentración geográfica amplifica el riesgo sistémico
- Cundinamarca + Boyacá + Nariño = ~85% de la producción nacional.
- Cualquier choque local en estos tres departamentos tiene impacto país.

### 3. El correlograma revela una sustitución estructural
- Producción nacional vs. importaciones: **r = −0,69**
- Producción nacional vs. exportaciones: **r = −0,80**
- Lectura: a más papa precocida importada, menos producción interna.

### 4. El dendograma agrupa a Colombia con países en declive
Linkage de Ward sobre patrones temporales estandarizados (z-score por país) ubica a Colombia junto a México y Ucrania (trayectorias decrecientes), mientras los grandes asiáticos (China, India, Bangladesh, Pakistán) forman un cluster ascendente que sostiene la presión bajista global.

### 5. El bootstrap muestra alertas tempranas dos años antes del paro
- En **2020 y 2021**, el IC 95% del margen anual estuvo enteramente por debajo de cero.
- En **2024**, volvió a ubicarse bajo cero, anticipando el colapso de 2025.
- Las medidas debieron tomarse en 2021, no en agosto de 2025.

### 6. El Monte Carlo cuantifica el contrafáctico
| Escenario | Política | P(pérdida del productor) |
|-----------|----------|--------------------------|
| A | Status quo | **100%** |
| B | Salvaguardia + antidumping | 99% |
| C | B + planificación de siembras | **36%** |

Solo intervenciones combinadas habrían evitado la crisis con probabilidad razonable.

### 7. Pruebas estadísticas confirmatorias
- **T-Welch** margen pre-crisis (2018–2022) vs post-crisis (2023–2025): cambio de signo de +$7.201 a −$4.815 por bulto.
- **Spearman** margen mensual vs tiempo: ρ = −0,30 (p < 0,01) — tendencia decreciente significativa.

---

## 📄 Informe ejecutivo

El [informe en PDF (5 páginas)](Informe_Analisis_Crisis_Papera_Colombia.pdf) contiene:
- Descripción del problema y contexto sectorial
- Metodología y proceso de limpieza
- Tabla de resúmenes numéricos
- 8 figuras seleccionadas con interpretación
- Discusión, conclusiones y reflexión metodológica

---

## 🛠️ Stack técnico

- **`pandas`** — manipulación, limpieza, imputación (`groupby().transform(fillna)`, `interpolate`)
- **`numpy`** — operaciones numéricas y generación pseudo-aleatoria
- **`matplotlib` + `seaborn`** — visualización con paleta personalizada inspirada en tonos tierra
- **`scipy.stats`** — pruebas de hipótesis (Welch, Spearman, Kendall, Pearson)
- **`scipy.cluster.hierarchy`** — clustering jerárquico con linkage de Ward
- **`reportlab`** — generación del informe PDF maquetado

---

## 📚 Fuentes consultadas

- **FAOSTAT** — Food and Agriculture Organization of the United Nations (2025), vía [Our World in Data](https://ourworldindata.org/grapher/potato-production)
- **Fedepapa** — Federación Colombiana de Productores de Papa
- **Observatorio FNFP** — [observatoriofnfp.com](https://observatoriofnfp.com/home/datos/tableros-de-informacion/)
- **MinAgricultura** — Evaluaciones Agropecuarias Municipales (EVA)
- **DANE** — Sistema de Información de Precios (SIPSA)
- **Legiscomex** — Comercio exterior
- Cobertura de prensa 2020–2025: Semana, El Espectador, Infobae, El Colombiano, Agronet

---

## 📜 Licencia

Distribuido bajo licencia MIT. Ver [LICENSE](LICENSE) para más información.

Los datos sintetizados a partir de fuentes públicas siguen las licencias de origen (FAO, DANE, MinAgricultura).

---

## ✍️ Autoría y contexto académico

Proyecto desarrollado como **Actividad Sumativa de la Unidad 4** del módulo *Lenguajes de Programación en Ciencia de Datos*.

**Indicador de logro:** *Desarrolla autónomamente análisis exploratorios y descriptivos de conjuntos de datos, en cuadernos de R y Python, empleando técnicas gráficas y numéricas para comunicar las evidencias encontradas.*

---

## 🤝 Contribuciones

Las sugerencias para mejorar el análisis, ampliar el período histórico o conectar fuentes adicionales son bienvenidas. Abre un *issue* o envía un *pull request*.

---

> *"En agricultura, los números cuentan la historia mucho antes de que estallen los paros. El problema rara vez es la falta de datos — es la falta de quien los mire a tiempo."*

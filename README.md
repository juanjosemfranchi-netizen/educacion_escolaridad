# 🏫 Segmentación de Establecimientos Educativos e Indicadores de Trayectoria Escolar

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458.svg)](https://pandas.pydata.org/)
[![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-Machine%20Learning-F7931E.svg)](https://scikit-learn.org/)
[![Google Colab](https://img.shields.io/badge/Google%20Colab-Open%20Notebook-F9AB00.svg)](https://colab.research.google.com/)

Este proyecto analiza las heterogeneidades en la infraestructura, equipamiento tecnológico y conectividad de las escuelas en Argentina mediante técnicas de **aprendizaje no supervisado (Clustering)**. El objetivo es identificar perfiles institucionales y evaluar su relación con los indicadores de eficiencia interna y trayectoria escolar (**abandono, promoción efectiva y repitencia**).

---

## 📌 Tabla de Contenidos
- [Contexto y Problema](#-contexto-y-problema)
- [Objetivos](#-objetivos)
- [Estructura del Repositorio](#-estructura-del-repositorio)
- [Conjuntos de Datos](#-conjuntos-de-datos)
- [Uso en Google Colab](#-uso-en-google-colab)
- [Metodología](#-metodología)
- [Autores](#-autores)

---

## 🔍 Contexto y Problema

En el sistema educativo argentino existen disparidades significativas en términos de acceso a electricidad, calidad de internet, recursos informáticos e instalaciones pedagógicas. 

Actualmente resulta complejo identificar objetivamente agrupamientos de escuelas que compartan condiciones materiales o tecnológicas similares. Este proyecto busca responder:

> **¿Existen agrupamientos naturales de escuelas según su infraestructura y equipamiento que se asocien a comportamientos diferenciales en sus tasas de abandono, repitencia y promoción?**

---

## 🎯 Objetivos

1. **Preprocesamiento e Integración:** Unificar la Base de Datos de Establecimientos Educativos con la de Indicadores Educativos a través de la clave única institucional (`cueanexo`).
2. **Segmentación (Clustering):** Aplicar algoritmos de aprendizaje no supervisado (ej. *K-Means*, *DBSCAN*) sobre variables de infraestructura, conectividad y equipamiento tecnológico.
3. **Análisis de Trayectorias (EDA):** Analizar mediante exploración estadística post-clustering si los perfiles identificados presentan diferencias significativas en las tasas de abandono, repitencia y promoción efectiva.

---

## 📂 Estructura del Repositorio

```text
├── data/
│   ├── base_escuelas.csv           # Dataset principal de infraestructura y equipamiento
│   └── indicadores_educativos.csv  # Dataset secundario de trayectoria escolar
├── notebooks/
│   └── analisis_clustering_educacion.ipynb  # Cuaderno principal de Google Colab / Jupyter
├── README.md                       # Documentación general del proyecto
└── requirements.txt                # Dependencias de Python (opcional)
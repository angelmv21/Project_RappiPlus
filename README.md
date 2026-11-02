readme_content = """# 📊 Proyecto RappiPlus: De datos a decisiones de negocio

Este repositorio documenta el proyecto de **Data Analytics** enfocado en evaluar la efectividad y rentabilidad de "RappiPlus", un servicio de suscripción premium dentro del ecosistema de Rappi. El análisis busca responder interrogantes clave del equipo de negocio sobre el incremento real en la frecuencia de compra, la rentabilidad del modelo y los cuellos de botella en el proceso de conversión de los usuarios.

---

## 🎯 Objetivo del Proyecto

El proyecto tiene como finalidad traducir datos transaccionales, de catálogo y de marketing en *insights* comerciales accionables. A través de este análisis, se busca responder:
- **¿Podemos confiar en los datos?** Auditoría, limpieza y validación de las bases de datos.
- **¿Estamos ganando dinero?** Cálculo de *Revenue*, costos operativos, inversión publicitaria y *Profit* Neto.
- **¿Dónde se pierden los usuarios?** Análisis de embudo (*Funnel Analysis*) para identificar fricciones en el flujo de compra.
- **¿Los usuarios regresan?** Medición de la retención por cohortes.
- **¿Los cambios generan impacto?** Validación de experimentos A/B mediante pruebas de significancia estadística.

---

## 📁 Datasets Utilizados

El análisis se alimenta de tres bases de datos extraídas del sistema transaccional y plataformas de adquisición:

| Archivo | Descripción | Variables Clave |
| :--- | :--- | :--- |
| **`rappiplus_orders_raw.csv`** | Registros de compras realizadas por los usuarios. | `id_pedido`, `fecha_hora_pedido`, `cantidad`, `precio_unitario`, `monto_descuento`, `monto_total`. |
| **`rappiplus_catalog.csv`** | Maestro de productos ofrecidos en la plataforma. | `nombre_producto`, `categoria_producto`, `costo_unitario`, `proveedor`. |
| **`rappiplus_marketing_spend.csv`** | Histórico de inversión en campañas de adquisición. | `fecha`, `pais`, `canal`, `gasto`. |

---

## 🧩 Etapas del Análisis

El desarrollo técnico se estructuró en 6 etapas progresivas:

### **Paso 1: Preparación y calidad de datos con Python**
- Carga de datos directamente desde repositorios en la nube (AWS S3) usando Pandas.
- Conversión segura de formatos temporales y eliminación de duplicados.
- Auditoría matemática (*Recálculo de monto_total*) para solucionar más de 1,100 inconsistencias transaccionales.
- Tratamiento de sentinels (compras irracionales/negativas) e imputación responsable de valores nulos (asignación a categoría "Desconocido") para salvaguardar la integridad de las métricas macro de ingresos.

### **Paso 2: Análisis de Rentabilidad (KPIs Core)**
- Integración de catálogos y órdenes mediante cruces transaccionales (`.merge`).
- Cálculo preciso de métricas operativas: Ingreso Total (*Revenue*), Costo Total, Gasto en Marketing y *Profit* Neto.
- Definición de KPIs comerciales: Ticket Promedio ($385.88) y Producto Estrella (Vacuum-Pro-Black por volumen de unidades).

### **Paso 3: Embudo de Conversión (SQL Funnel Analysis)**
- Conexión a base de datos PostgreSQL mediante SQLAlchemy.
- Diseño de consultas analíticas avanzadas utilizando funciones de ventana (`LAG`, `FIRST_VALUE`) para calcular conversiones *paso a paso* y conversiones *totales*, mapeando la pérdida exacta de prospectos desde su primera visita hasta el *checkout*.

### **Paso 4: Retención por Cohortes (SQL Cohort Analysis)**
- Modelado temporal para agrupar clientes por mes de registro (Cohorte).
- Cálculo matricial de retención utilizando agrupación relativa por semanas transcurridas (`FLOOR((fecha_actividad - fecha_registro) / 7)`).

### **Paso 5: Pruebas Estadísticas (A/B Testing)**
- Formulación de hipótesis estadísticas (H₀ vs H₁) para evaluar un rediseño de interfaz en el *checkout*.
- Aplicación de un **Z-test de Proporciones** (usando `statsmodels`) para validar con rigor matemático (nivel de significancia del 5%) si la nueva versión generó un levantamiento real en la conversión de compra o si el resultado fue producto del azar.

### **Paso 6: Comunicación de Resultados (Dashboard BI)**
- Diseño de un Panel Ejecutivo consolidando *Revenue*, *Gross Profit* (desacoplado estratégicamente del gasto general de marketing para evaluación por categorías) y *Ticket Promedio*.
- Solución arquitectónica de errores de origen (incompatibilidad de decimales) e implementación de interacciones filtradas para proteger las métricas macro de cortes dimensionales inconsistentes.

---

## 💻 Cómo Ejecutar el Notebook

Puedes explorar o reproducir este análisis de diversas maneras:

### 1. **Visualización Directa en GitHub**
Navega en este repositorio y haz clic en el archivo `.ipynb`. GitHub renderizará automáticamente el código, los resultados en consola y los hallazgos sin necesidad de software adicional.

### 2. **Ejecución Interactiva en Google Colab**
1. Abre [Google Colab](https://colab.research.google.com/).
2. Importa el notebook directamente desde este repositorio de GitHub.
3. Al conectarse dinámicamente a las URLs de AWS, no necesitas subir archivos manualmente. Simplemente ejecuta las celdas en orden (`Runtime > Run all`).

---

## 🛠️ Guía de Reproducción Local

Para correr el proyecto en tu propio entorno (VS Code / Jupyter):

### **Prerrequisitos**
Tener instalado Python 3.8+ y `pip`.

### **Instalación**

1. **Clonar el repositorio:**
   ```bash
   git clone [https://github.com/angelmv21/Project_RappiPlus.git](https://github.com/angelmv21/Project_RappiPlus.git)
   cd Project_RappiPlus

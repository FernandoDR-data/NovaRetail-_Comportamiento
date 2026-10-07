# Explorando factores de comportamiento en NovaRetail+

Análisis exploratorio y **correlacional** sobre clientes de NovaRetail+, una plataforma de comercio electrónico en Latinoamérica. El objetivo es responder:

> **¿Qué factores del comportamiento del cliente están más fuertemente asociados con el ingreso anual generado?**

> ⚠️ Correlación ≠ causalidad. Este proyecto identifica asociaciones, no efectos causales.

## Contenido del repositorio

| Archivo | Descripción |
|---|---|
| `NovaRetail+_COMPORTAMIENTO.ipynb` | Notebook con la exploración, limpieza, visualización, correlaciones e interpretación de negocio |

## Datos

Dataset `novaretail_comportamiento_clientes_2024.csv` (leído desde `/datasets/`): **15,000 clientes** y **12 columnas**, sin valores nulos.

| Columna | Descripción |
|---|---|
| `id_cliente` | Identificador único del cliente |
| `edad` | Edad del cliente |
| `nivel_ingreso` | Ingreso anual estimado del cliente |
| `visitas_mes` | Visitas mensuales a la app o sitio |
| `compras_mes` | Compras realizadas en el mes |
| `gasto_publicidad_dirigida` | Gasto en anuncios asignado al usuario |
| `satisfaccion` | Calificación de satisfacción (1 a 5) |
| `miembro_premium` | Suscripción premium (1) o no (0) |
| `abandono` | Abandonó la plataforma (1) o no (0) |
| `tipo_dispositivo` | Móvil, escritorio o tablet |
| `region` | Norte, sur, este u oeste |
| `ingreso_anual` | Ingreso anual que el cliente genera para la empresa (**variable objetivo**) |

## Metodología

1. **Carga y exploración**: estructura, tipos de dato y estadísticas descriptivas.
2. **Limpieza**: `edad` convertida de decimal a entero. El resto de variables ya estaban correctamente tipificadas.
3. **Visualización**: mapa de calor de correlaciones y *scatterplots* con línea de regresión para `visitas_mes` y `compras_mes` contra `ingreso_anual`.
4. **Coeficientes según el tipo de variable**:
   - **Pearson** y **Spearman**: numérica–numérica (relaciones lineales y monótonas).
   - **Punto biserial**: numérica–binaria.
   - **V de Cramér**: categórica–categórica.

## Resultados principales

| Relación | Método | Resultado |
|---|---|---|
| `compras_mes` ↔ `ingreso_anual` | Pearson / Spearman | **0.967 / 0.967** (muy fuerte, positiva) |
| `visitas_mes` ↔ `ingreso_anual` | Pearson / Spearman | 0.337 / 0.321 (moderada, positiva) |
| `miembro_premium` ↔ `ingreso_anual` | Punto biserial | 0.093 (muy débil, positiva; p ≈ 3e-30) |
| `abandono` ↔ `ingreso_anual` | Punto biserial | −0.003 (p = 0.73, sin asociación significativa) |
| `abandono` ↔ `miembro_premium` | V de Cramér | 0.120 (débil) |
| `tipo_dispositivo` ↔ `region` | V de Cramér | 0.012 (prácticamente nula) |

**Lectura de negocio**

- El número de **compras mensuales** es, por mucho, el factor más asociado al ingreso anual. La correlación es tan alta que probablemente refleja **colinealidad** (el ingreso se construye a partir de las compras), por lo que conviene no tratarla como un hallazgo "accionable" por sí sola.
- Las **visitas** muestran una asociación moderada: más visitas tienden a ir con más ingreso, pero no se puede afirmar que aumentar visitas aumente las compras.
- Ser **miembro premium** se asocia con un ingreso ligeramente mayor, pero la relación es muy débil.
- Cerca del 25 % de los clientes no genera ingreso, y el 50 % hace solo una compra al mes, a pesar de visitar el sitio con frecuencia (mediana de 10 visitas).
- El dispositivo móvil predomina en las cuatro regiones, y la región norte concentra más clientes.

**Recomendación**: explorar estrategias que conviertan visitas en compras (engagement previo a la compra) y validarlas con experimentos controlados; segmentar a los miembros premium para detectar clientes de alto valor y dirigirles campañas personalizadas.

## Limitaciones

- Correlación no implica causalidad.
- Posibles efectos de segmentación no explorados y variables no observadas.
- Alta colinealidad entre `compras_mes` e `ingreso_anual`.
- El análisis usa todo el conjunto de datos sin controlar por otras variables.

## Próximos pasos

- Segmentar por dispositivo y región.
- Diseñar experimentos A/B sobre estrategias de conversión.
- Análisis de cohortes.
- Modelos multivariados (p. ej. regresión) para controlar por varias variables a la vez.

## Requisitos

- Python 3.9+
- `pandas`, `numpy`, `scipy`, `matplotlib`, `seaborn`, `jupyter`

```bash
pip install pandas numpy scipy matplotlib seaborn jupyter
jupyter notebook "NovaRetail+_COMPORTAMIENTO.ipynb"
```

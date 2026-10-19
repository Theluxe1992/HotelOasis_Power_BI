# 🏨 Hotel Oasis Digital — Analítica de Datos y Proceso ETL en Power BI & Power Query

Este proyecto documenta e implementa el ciclo completo de **Extracción, Transformación y Carga (ETL)**, gobernanza de datos y **Business Intelligence** para la cadena hotelera **Hotel Oasis Digital**, utilizando **Microsoft Power BI** y **Power Query**.

---

## 🎯 Problema de Negocio y Calidad de Datos

Previo a la intervención, la base de datos del hotel presentaba severas inconsistencias que distorsionaban las métricas gerenciales y operativas:
- **Duplicidad de registros:** Falta de identificación única e incoherencias en nombres de huéspedes (ej. "Juan Pérez" vs "Juan A. Pérez").
- **Valores atípicos extremos (Outliers):** Registros con tarifas por noche anómalas de hasta $10,000,000,000 que sesgaban el promedio real.
- **Inconsistencia en fechas:** Registros con fechas de Check-Out anteriores a la fecha de Check-In.
- **Datos faltantes (Valores Nulos):** Lagunas en canales de reserva, preferencias de huéspedes y cargos por servicio.

---

## 🛠️ Proceso ETL en Power Query (Paso a Paso)

El flujo de limpieza y transformación se ejecutó en **Power Query M** mediante los siguientes pasos aplicados:

1. **Extracción:** Conexión e ingesta de datos desde el archivo Excel fuente (`Informe_de_Huespedes.xlsx`).
2. **Depuración de Duplicados & Columnas Redundantes:**
   - Eliminación de registros idénticos duplicados (`Table.Distinct`).
   - Depuración de la columna *Identificación Huésped* para proteger la privacidad de los datos.
3. **Estandarización de Texto & Formatos:**
   - Normalización de la columna *Nombre del Huésped* a mayúsculas sostenidas (`Text.Upper`).
   - Limpieza de espacios en blanco adicionales (`Text.Trim`).
4. **Tratamiento de Outliers & Valores Nulos:**
   - Normalización del valor atípico extremo en *Tarifa por Noche* mediante el método del **percentil 95** y reemplazo por la media/moda.
   - Imputación de valores faltantes en variables clave utilizando la media aritmética y la moda estadística.
5. **Validación y Lógica de Fechas:**
   - Creación de columnas condicionales/personalizadas para corregir inversiones cronológicas entre *Check-In* y *Check-Out*.

---

## 📊 Dashboards y Reportes de Analítica en Power BI

Una vez cargados los datos limpios en el modelo, se estructuraron **3 reportes ejecutivos e interactivos**:

### 📈 1. Reporte de Ocupación y Reservas
- **KPIs clave:** Total de reservas procesadas (1,026) y duración promedio de estancia (21.43 noches).
- **Análisis de Canales:** Evaluación del rendimiento de reservas por canal (Directa, Agencia, Web, Corporativo).
- **Tendencias:** Gráfico temporal de demanda para identificar meses pico de ocupación.

### 🛏️ 2. Reporte de Preferencias de los Huéspedes
- **Análisis de Consumo & Confort:** Preferencia de tipos de almohada (Firme, Plumas, Sintética) y restricciones dietéticas (Vegetariana, Vegana, Sin Gluten).
- **Distribución por Habitación:** Segmentación de la demanda entre *Suite Junior*, *Suite Ejecutiva*, *Premium King*, *Estándar Doble* e *Individual*.

### 💰 3. Reporte de Tarifas y Revenue Management
- **Métricas Financieras:** Tarifa promedio general ($10,000) y desglose de costo de alojamiento e ingresos por estancia.
- **Rentabilidad por Canal:** Comparativa de tarifas promedio por canal de ventas para optimizar la estrategia de precios.

---

# ***********************************************************************************************************************************

# Exploración de datos

# Conexión a la fuente de datos: Cargamos el archivo Informe de Huéspedes en Data Orange Mining a través del widget “File”. Conectamos este widget con “Data Info”, el cual nos brinda un resumen del # conjunto de datos, donde incluye numero total de instancias (filas) y atributos (columnas), como también de sus características de # tipos de datos,	Ej:	(numéricos, categóricos, entre otros).
# Con “Data Info” se determina que el archivo cuenta con 1030 registros y 15 atributos (columnas).	 

# Valores atípicos: Para identificar los valores atípicos se usa el widget “Column Statistics” del bloque “Data”.
# Hallamos un valor atipico de gran magnitud en la variable “Tarifa por Noche” en cual como valor máximo es de 10,000,000,000, el cual es un valor extremadamente alto en comparación de los demás datos que distorsiona la escala real del análisis.	

# Media y moda:
# Usamos el mismo widget “Column Statistics” para verificar la media y la moda, con las variables “Tarifa por Noche”, “Días de estancia” y “Total cargos por servicio”, los valores fueron los siguientes:
# -	“Tarifa por Noche”:
# Media = 10,000. Moda = 10,000
# -	“Días de Estancia”: Media = 3. Moda = 4.
# -	“Total	cargos	por servicio”:
# -	Media = 4,350. Moda = 2,500	

# Columnas de la fuente de datos: Visualizamos la lista de todas las columnas que están contenidas en la base de datos. Usamos el widget “Data Table” en el bloque de “Data” el cual nos muestra el contenido de los encabezados y los primeros registros.
# Dividimos el formato en 3 imágenes de forma secuencial que muestra la forma de la base total ya que al ponerla en una sola imagen gracias al formato en PDF puede no ser legible	 

# Calidad de los datos:
# Evaluando la calidad de los datos que nos permite reconocer e identificar los valores atípicos, datos faltantes o duplicados, usamos el widget “Scatter Plot” del	bloque	“Visualize”. Encontramos los siguientes resultados:
# 1.	Tarifa por Noche (Eje “x”) y Días de estancia (Eje “y) muestra el valor atípico ya identificado en la sección 2 de esta tabla de un color amarillo (10000000000), lo cual debemos eliminar o editar este dato ya que distorsiona	la visualización de los datos y comprime los números según su rango.
# 2.	En  la  misma  imagen podemos  encontrar  una leyenda que dice “  Points with missing 'Tarifa por Noche' or 'Días de Estancia'	are	not displayed”, lo que nos permite identificar datos nulos o faltantes en estas dos columnas “Tarifa por Noche” y “Días de Estancia” lo que afecta la integridad de los datos.
# 3. Debido al valor atípico, la escala del eje “x” (Tarifa por Noche) se ve afectada la cual aumenta el rango de forma exagerada y no permite   visualizar la distribución real de los datos.	

# ***********************************************************************************************************************************

# El proceso ETL se desarrolló en Microsoft Power BI utilizando el Editor de Power Query. Primero, se extrajo el archivo Excel 'Informe de Huéspedes.xlsx'. Luego, se aplicaron transformaciones para mejorar la calidad de los datos: se eliminaron filas duplicadas y la columna 'Identificación Huésped'; se convirtió 'Nombre del Huésped' a mayúsculas; se rellenaron valores nulos con el promedio y la moda; se normalizaron valores atípicos mediante el método del percentil 95; se eliminaron espacios en blanco al inicio y al final de los textos; y se corrigieron fechas inconsistentes (Check-Out anterior a Check-In). Finalmente, los datos limpios se cargaron en el modelo de Power BI para construir los reportes. La finalidad de este proceso fue garantizar que los datos fueran confiables y esten listos para el análisis y la toma de decisiones.

# ***********************************************************************************************************************************

# Reporte 1: Análisis de Ocupación y Reservas Visualizaciones:
•	Tarjetas: Total de Reservas (1,026) y Duración Promedio (21.43 noches).
•	Gráfico de líneas: Tendencia de reservas por mes.
•	Gráfico de barras: Reservas por Canal de Reserva.
•	Segmentación: Filtro por Tipo de Habitación.
Interpretación:
# "El análisis de ocupación muestra que el mes de [Mes con pico] registró el mayor volumen de reservas, con un total de 1,026 reservas en el período analizado. El canal de reserva más utilizado fue [Canal], representando la mayor parte de las reservas, lo que sugiere que las campañas en ese canal están siendo efectivas. La duración promedio de estancia es de 21.43 noches, lo que indica una preferencia por estancias largas. Se recomienda enfocar las promociones en [Canal] durante los meses de alta demanda para maximizar la ocupación y los ingresos." 

# Reporte 2: Preferencias de los Huéspedes Visualizaciones:
•	Gráfico de barras: Preferencia de Almohada.
•	Gráfico circular: Distribución por Tipo de Habitación.
•	Segmentación: Filtro por Tipo de Habitación. 
Interpretación:
# "El análisis de preferencias muestra que la almohada más solicitada es [Tipo de almohada], lo que indica que debería ser la opción predeterminada en todas las habitaciones. El tipo de habitación más reservado es [Tipo], lo que sugiere que este segmento es el mercado principal. Se recomienda garantizar la disponibilidad de [Tipo de almohada] en todas las habitaciones y considerar promociones para los tipos de habitación menos reservados."
 
# Reporte 3: Análisis de Tarifas Visualizaciones:
•	Tarjeta: Tarifa Promedio General (10,000).
•	Gráfico de barras: Tarifa Promedio por Tipo de Habitación.
•	Gráfico de barras: Tarifa Promedio por Canal de Reserva.
Interpretación:
# "El análisis de tarifas muestra que la tarifa promedio general es de 10,000. La habitación [Tipo con tarifa más alta] tiene la tarifa promedio más elevada ([valor]), mientras que [Tipo con tarifa más baja] es la más económica. Por canal de reserva, [Canal] presenta la tarifa más alta, lo que sugiere que los huéspedes que reservan a través de este canal están dispuestos a pagar más. Se recomienda revisar la estrategia de precios para [Tipo de habitación] y considerar promociones en los canales con tarifas más bajas para aumentar la ocupación."

# ***********************************************************************************************************************************

## ⚙️ Requisitos e Instalación

1. Tener instalado **Microsoft Power BI Desktop** (versión 2023 o superior).
2. Abrir el archivo `Informe_Huespedes_ETL.pbix` en Power BI Desktop.
3. *(Opcional)* Entrar al **Editor de Power Query** para revisar el historial completo de las transformaciones en el panel *Pasos Aplicados*.

---

## 👤 Autor

**Juan Pablo Alarcón Naranjo**  
*Estudiante de Tecnología en Desarrollo de Software — UNAD*  
- 📧 Email: junecropa@outlook.com  
- 🐙 GitHub: @Theluxe1992

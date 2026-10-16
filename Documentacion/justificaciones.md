# Justificaciones Técnicas - Evaluación Práctica 2
**Caso de Estudio:** GreenEnergy Analytics
**Asignatura:** Gestión de Datos para Inteligencia Artificial y Big Data (IDS221)

---

## P1: Power BI (Dashboard Interactivo)
Antes de filtrar, el déficit total es de 286,904.20 kWh y la cobertura global es del 90.88%. El déficit energético no se puede obtener restando la demanda total menos la generación total global (lo que arrojaría 233,243.12 kWh y una cobertura falsa de 92.59%), debido a que dicha operación compensa de forma errónea los días de déficit con los días de superávit en toda la red. Por esta razón, el cálculo debe realizarse fila por fila evaluando únicamente los registros donde la demanda supera a la generación (`MAX(demanda - generación, 0)`). Al seleccionar una región mediante el segmentador, Power BI recalcula dinámicamente el contexto de filtrado sobre las filas correspondientes a dicha zona territorial.

## P2: ArcGIS (Análisis Geoespacial)
https://arcg.is/jTLiS2
Se validaron geográficamente 8 sedes y se excluyó 1 sede (`S09` - Solar La Unión) debido a un error crítico de longitud fuera del rango territorial válido (`-67.84` en lugar de estar entre `-91` y `-87`). La sede que presenta el mayor déficit válido es la `S04` (Solar San Salvador) con un total acumulado de 77,336.41 kWh. Mostrar coordenadas erróneas genera distorsiones espaciales graves, ubicando los activos fuera de la región operativa real y comprometiendo severamente la toma de decisiones gerenciales.

## P3: Reportería Gerencial Automatizada
Al volver a ejecutar el script de reportería, se actualizan automáticamente las marcas de tiempo, los gráficos analíticos y las tablas ejecutivas sin que se tenga que intervenir o editar manualmente el informe. La concordancia y consistencia de los datos se comprueban cotejando los totales gerenciales con el contrato `reporte_kpis.csv` y validando la integridad original mediante la firma criptográfica `sha256_fuente`. La principal limitación operativa radica en que el modelo se fundamenta en una simulación de 180 días de operación sintética, por lo que el sistema no está diseñado para anticipar perturbaciones climáticas extremas o desastres naturales repentinos no modelados.

## P4: Fundamentos de Predicción con IA
El conjunto de prueba abarca cronológicamente desde el 2026-05-26 hasta el 2026-06-29 (35 registros). El modelo de Regresión Lineal aporta valor predictivo real al reducir el Error Absoluto Medio (MAE) a 298.77 en comparación con los 1,194.57 del modelo baseline (`DummyRegressor`), logrando una mejora relativa del 74.99%. Se evita la fuga de información (*data leakage*) mediante una partición estrictamente cronológica (80/20) y calculando las variables de rezago y media móvil con un desplazamiento previo (`shift(1)`), garantizando que el modelo jamás se entrene con información del futuro. Su limitación operativa principal es la escasez de historial a corto plazo en el set de prueba y la dependencia estricta de los rezagos previos.

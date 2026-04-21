# Optimización de Consumo Energético en Proceso Industrial
## Identificación de Oportunidades de Mejora en Eficiencia Energética

Autora: Wendy J. Hernández
Perfil: Ingeniería Química · Especialización Ambiental · Data Analytics
Stack: Python · Power BI · Excel
Dataset: Steel Industry Energy Consumption — UCI Machine Learning Repository
https://archive.ics.uci.edu/dataset/851/steel+industry+energy+consumption

---

## Contexto

La gestión eficiente de la energía en procesos industriales es uno de los pilares
de la sostenibilidad operacional y la competitividad empresarial. En la industria
de manufactura continua, el consumo energético representa entre el 20% y el 40%
de los costos operacionales totales (IEA, 2023), lo que hace que identificar
oportunidades de reducción tenga impacto directo en la rentabilidad del negocio
y en el cumplimiento de compromisos de sostenibilidad corporativa.

Este proyecto analiza datos reales de consumo energético de una planta siderúrgica
en Corea del Sur para identificar patrones de ineficiencia, cuantificar
oportunidades de mejora y proponer estrategias de optimización basadas en
evidencia de datos, bajo el marco de gestión energética ISO 50001.

---

## Dataset

Fuente: V E, S., Shin, C., & Cho, Y. (2021). Steel Industry Energy Consumption.
UCI Machine Learning Repository. https://doi.org/10.24432/C52G8C

Planta: DAEWOO Steel Co. Ltd — Gwangyang, Corea del Sur
Período: 1 año completo con mediciones cada 15 minutos
Registros: 35,040
Licencia: Creative Commons Attribution 4.0 (CC BY 4.0)

| Variable | Descripción | Unidad |
|---|---|---|
| Usage_kWh | Consumo de energía eléctrica | kWh |
| Lagging_Reactive_Power_kVarh | Potencia reactiva rezagada — cargas inductivas | kVarh |
| Leading_Reactive_Power_kVarh | Potencia reactiva adelantada — cargas capacitivas | kVarh |
| CO2_tCO2 | Índice de emisiones proporcional al consumo eléctrico | ver nota metodológica |
| Lagging_Power_Factor | Factor de Potencia rezagado — indicador de eficiencia | % |
| Leading_Power_Factor | Factor de Potencia adelantado | % |
| NSM_seconds | Segundos desde medianoche — proxy de hora del día | s |
| WeekStatus | Weekday (día laboral) o Weekend (fin de semana) | categórica |
| Day_of_Week | Día de la semana | categórica |
| Load_Type | Light Load, Medium Load, Maximum Load | categórica |

### Nota metodológica sobre la variable CO2

La documentación original del dataset registra esta variable con unidades de ppm,
lo cual es técnicamente incorrecto para una variable de emisiones industriales.
Las emisiones de CO2 de un proceso industrial se expresan en unidades de masa
(tCO2 o kgCO2), no en concentración. Esta variable representa una estimación
proporcional al consumo eléctrico y debe interpretarse como un índice relativo
de emisiones, no como una medición certificable bajo GHG Protocol.

Para un inventario corporativo real bajo metodología GHG Protocol Scope 2 se
debe aplicar el factor de emisión oficial de la red eléctrica coreana publicado
por KEPCO (Korea Electric Power Corporation — Corporación de Energía Eléctrica
de Corea): 0.4567 kgCO2/kWh para el año 2021, que es el año de referencia del
dataset. Este factor es sustancialmente diferente al factor colombiano de
0.177 tCO2eq/MWh (UPME, 2023), lo que ilustra la importancia de usar el factor
del país y la red eléctrica específica del proceso analizado.

---

## Metodología

Carga del dataset mediante la API ucimlrepo con descarga automática desde UCI.
Análisis exploratorio de patrones de consumo horario y semanal. Ingeniería de
variables temporales: hora del día, turno, potencia aparente e intensidad
energética. Análisis del Factor de Potencia por tipo de carga operacional.
Matriz de correlación entre variables de proceso. Modelado predictivo comparando
Regresión Lineal, Random Forest y Gradient Boosting. Análisis de importancia
de variables con el mejor modelo. Cuantificación de oportunidades de mejora
con ahorro estimado en kWh y reducción de emisiones. Exportación de KPIs
alineados con ISO 50001 para dashboard en Power BI.

---

## Hallazgos principales

El proceso presenta una relación pico/valle de 13.86 veces entre la hora de
mayor y menor consumo. El pico ocurre a las 09:00 horas con 58.55 kWh promedio
y el valle a las 06:00 horas con 4.22 kWh. Los días laborales consumen en
promedio 2.3 veces más que los fines de semana.

La condición de carga ligera opera con un Factor de Potencia de 69.7%, que está
muy por debajo del umbral óptimo del 90% establecido por la norma IEC 61000. En
esos períodos el proceso consume 0.812 kVarh de energía reactiva por cada kWh
útil, que es 68% más de lo aceptable. Las condiciones de carga media (FP 93.1%)
y carga máxima (FP 91.0%) ya operan dentro del rango óptimo.

El modelo Random Forest logró un R² de 0.9996, con la Potencia Reactiva Rezagada
explicando el 87.1% de la importancia predictiva del modelo, seguida del Factor
de Potencia Rezagado con 9.6%.

---

## Oportunidades de mejora

La primera oportunidad es corregir el Factor de Potencia en los períodos de baja
actividad, donde el proceso desperdicia el doble de energía reactiva de lo
aceptable (FP de 69.7% contra el mínimo de 90%). La solución es instalar un
banco de condensadores automático (APFC — Automatic Power Factor Correction)
que corrija este desperdicio sin intervención manual. El retorno de inversión
típico es entre 12 y 24 meses (IEA, 2023).

La segunda oportunidad es reducir el costo fijo de la factura eléctrica. El
operador de red mide el consumo cada 15 minutos durante todo el mes y al final
cobra un cargo fijo basado en el intervalo de 15 minutos donde más energía se
consumió, sin importar cuánto se consumió el resto del tiempo. Este cargo puede
representar entre el 30% y el 50% de la factura mensual total (DemandQ, 2025).
Como el proceso tiene picos de hasta 70 kWh entre las 09:00 y las 17:00 horas,
ese único momento define el cargo fijo del mes entero. Mover operaciones
secundarias como bombas, ventilación y precalentamiento de equipos a la
madrugada o al fin de semana reduce ese pico y con él el cargo fijo de la
factura, sin afectar la producción. Esta medida no requiere inversión en equipos,
solo reorganización de turnos.

---

## Conclusiones

Este proyecto demuestra que el análisis de datos de sensores industriales permite
identificar oportunidades de optimización energética concretas y cuantificables
sin necesidad de instrumentación adicional. Los datos que ya existen en el sistema
de gestión energética de cualquier planta industrial contienen la información
suficiente para tomar decisiones de mejora fundamentadas.

El hallazgo más importante es que el problema de ineficiencia energética de esta
planta está concentrado exclusivamente en los períodos de carga ligera, que
corresponden a las madrugadas y los fines de semana. Corregir el Factor de
Potencia únicamente en esos períodos mediante un sistema APFC eliminaría
prácticamente toda la ineficiencia reactiva del proceso, porque los modos de
carga media y máxima ya operan dentro del rango óptimo.

La alta predictibilidad del consumo energético con R² de 0.9996 confirma que
este proceso tiene un comportamiento altamente determinista, lo que lo hace
ideal para implementar un sistema de gestión energética basado en datos alineado
con la norma ISO 50001. Un modelo predictivo en línea podría estimar el consumo
esperado en tiempo real y alertar al operador cuando el proceso se desvíe de las
condiciones de eficiencia óptima.

Desde la perspectiva de sostenibilidad corporativa, las dos oportunidades de
mejora identificadas tienen impacto simultáneo sobre tres indicadores: reducción
del consumo de energía activa y reactiva, disminución de las emisiones Scope 2
bajo metodología GHG Protocol, y reducción del costo operacional de la factura
eléctrica industrial.


---

## Cómo ejecutar

Instalar dependencias:
```bash
pip install pandas numpy matplotlib seaborn scikit-learn ucimlrepo
```

Ejecutar el notebook:
```bash
jupyter notebook notebook/energy_optimization_industrial.ipynb
```

El dataset se descarga automáticamente desde UCI ML Repository mediante
ucimlrepo. Solo requiere conexión a internet.

---

## Referencias

Aurora Solar (2024). Making sense of demand charges: What are they and how
do they work? Aurora Solar.
https://aurorasolar.com/blog/making-sense-of-demand-charges-what-are-they-and-how-do-they-work/

DemandQ (2025). Understanding electric demand. DemandQ.
https://www.demandq.ai/demandlab/understanding-electric-demand/

IEA — International Energy Agency (2023). Energy efficiency 2023.
IEA Publications.

IEC (2002). IEC 61000: Electromagnetic compatibility — Power quality
standards. International Electrotechnical Commission.

ISO (2018). ISO 50001:2018: Energy management systems — Requirements
with guidance for use. International Organization for Standardization.

KEPCO — Korea Electric Power Corporation (2021). Emission factor for
the Korean national grid. Korea Electric Power Corporation.

V E, S., Shin, C., & Cho, Y. (2021). Steel industry energy consumption
[Dataset]. UCI Machine Learning Repository.
https://doi.org/10.24432/C52G8C

WRI y WBCSD (2015). The greenhouse gas protocol: A corporate accounting
and reporting standard (Revised ed.). World Resources Institute y World
Business Council for Sustainable Development.

# Documentación de la Simulación de Sistemas de Colas

Esta documentación explica la estructura y el funcionamiento del archivo `Simulacion_Sistema_Colas_Enteros.xlsx`, el cual simula un sistema de colas de un solo canal (una sola caja/servidor) bajo diferentes escenarios de distribución de probabilidad.

## Estructura del Archivo Excel

El archivo consta de cuatro hojas principales, cada una diseñada para modelar un aspecto específico del sistema o resumir los resultados.

### 1. Caso Base (U-U)
Modela el sistema con distribuciones uniformes básicas.
- **Distribución de Arribos**: Uniforme entre 1 y 10 minutos.
- **Distribución de Servicio**: Uniforme entre 1 y 6 minutos.

### 2. Escenario 1 (Exp-U)
Analiza el impacto de llegadas aleatorias más variables.
- **Distribución de Arribos**: Exponencial con una tasa media ($\lambda$) de 6 clientes por hora (0.1 clientes/minuto). Esto implica un tiempo promedio entre llegadas de 10 minutos.
- **Distribución de Servicio**: Uniforme entre 1 y 6 minutos.

### 3. Escenario 2 (U-Norm)
Analiza el impacto de tiempos de atención con tendencia central (campana de Gauss).
- **Distribución de Arribos**: Uniforme entre 1 y 10 minutos.
- **Distribución de Servicio**: Normal con media ($\mu$) de 3 minutos y desviación estándar ($\sigma$) de 3 minutos. (Se limita a un mínimo de 0.1 minutos para evitar tiempos negativos).

### 4. Resumen y Comparación
Contiene una tabla comparativa que utiliza fórmulas para extraer promedios y totales de las tres hojas anteriores:
- **Tiempo Promedio en Sistema**: El tiempo total que un cliente pasa desde que llega hasta que sale (después de ser atendido). Tiempo en el Sistema = Tiempo de Espera + Tiempo de Servicio.
- **Tiempo Desocupado Total Cajero**: La suma de todos los momentos en que el cajero no tuvo clientes que atender.
- **Tiempo Promedio de Espera**: El tiempo que el cliente pasa en la cola (antes de ser atendido).

---

## Definición de Columnas

En cada una de las hojas de simulación, se encuentran las siguientes columnas:

| Columna | Descripción | Fórmula Lógica |
| :--- | :--- | :--- |
| **Cliente** | Número identificador del cliente (1 a 50). | Secuencial |
| **Aleatorio Arribo** | Número aleatorio entre 0 y 1 para calcular el tiempo entre llegadas. | `RAND() - ALEATORIO()` |
| **Tiempo entre llegadas** | Minutos que transcurren entre la llegada de un cliente y el anterior. | Según distribución |
| **Tiempo de llegada** | Momento exacto en el tiempo (reloj) en que el cliente entra al sistema. | Acumulado de llegadas |
| **Aleatorio Servicio** | Número aleatorio entre 0 y 1 para calcular el tiempo de atención. | `RAND() - ALEATORIO()` |
| **Tiempo de servicio** | Duración del trámite o atención del cliente en la caja. | Según distribución |
| **Inicio de servicio** | Momento en que el cajero comienza a atender al cliente. | `MAX(Llegada, Fin Anterior)` |
| **Fin de servicio** | Momento en que el cliente termina su atención y sale del sistema. | `Inicio + Servicio` |
| **Tiempo en sistema** | Tiempo total transcurrido desde la llegada hasta la salida. | `Fin - Llegada` |
| **Tiempo de espera** | Tiempo que el cliente pasó haciendo fila. | `Inicio - Llegada` |
| **Tiempo desocupado** | Tiempo que el cajero estuvo sin trabajar esperando al cliente. | `MAX(0, Inicio - Fin Anterior)` |

---

## Observaciones y Notas Técnicas

1. **Simulación Dinámica**: El archivo utiliza fórmulas de Excel en lugar de datos estáticos. Esto significa que cada vez que presionas **F9** o realizas un cambio, se generan nuevos números aleatorios y toda la simulación se actualiza instantáneamente.
2. **Uso de Funciones de Distribución**:
   - Para la distribución **Exponencial**, se utilizó la fórmula de transformación inversa: `-LN(1-R)/lambda`.
   - Para la distribución **Normal**, se utilizó `NORMINV` para asegurar compatibilidad con diferentes versiones de Excel.
3. **Interpretación de Resultados**: Al comparar los escenarios, notarás cómo la variabilidad de la distribución exponencial o la dispersión de la normal afectan directamente el tiempo de espera y el tiempo de ocio del servidor, incluso si las medias son similares.

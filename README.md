# Proyecto 1: Fundamentos

**Repositorio:** https://github.com/Trio-Maravilla/Primer-Proyecto-.git

## Integrantes

| Nombre | Código |
|---|---|
| Sara Sanchez | 202616172 |
| Nelson Donado | 202617494 |
| Leonardo Cifuentes | 202611307 |

---

## 1. Entendimiento del problema

La Alcaldía busca una primera aproximación al bienestar y las condiciones de vida de la población adulta residente en Bogotá D.C., con el objetivo de que esta información pueda contribuir a orientar futuras acciones de política pública. Como parte del análisis solicitado, se requiere hallar patrones, diferencias y características que ayuden a identificar elementos que permitan comprender mejor la situación de la población estudiada.

Teniendo esto en cuenta, se optó por abordar el tema de **bienestar y diversidad de la población**, para poder enfocarse en explorar posibles diferencias de distintos aspectos del bienestar entre los grupos con autorreconocimiento étnico. Esto resulta relevante porque permite identificar posibles relaciones entre determinados grupos étnicos y un mayor bienestar, información útil para orientar las futuras políticas públicas respecto a los grupos étnicos con menor bienestar.

A partir de esto, se planteó la siguiente **pregunta analítica**:

> **¿Qué características tiene el grupo étnico con mayor percepción de bienestar?**

Responder a esta pregunta permitirá que la Alcaldía pueda identificar la tendencia hacia ciertas características que podrían estar relacionadas con el bienestar de los grupos étnicos, y así orientar las acciones de política pública hacia las comunidades étnicas con menor bienestar, buscando aumentar en ellas las características encontradas en el grupo étnico con mayor percepción de bienestar.

Para poder responder esta pregunta, es necesario contar con:

- El grupo étnico con el cual se identifica cada persona.
- Las condiciones de vida de dicha persona (economía, estado civil, vivienda, trabajo).
- El bienestar en diferentes aspectos (laboral, emocional, salud, ocio).

---

## 2. Preparación de los datos (ETL)

### Filtros de edad y residencia en los últimos 12 meses

Para el filtro de edad se utilizó únicamente la variable `P6040`, que guarda el valor de la edad de cada registro y cuenta con 0 registros fallidos (o *Sysmis*).

Por otro lado, fue complicado realizar el filtro de residencia en los últimos 12 meses, debido a que la única variable que servía como identificador de ubicación reducía el dataset a tan solo 200 datos útiles, de los cuales solo 7 cumplían con los filtros de etnia. Por esta razón se decidió **no aplicar el filtro de residencia**; de haberlo sostenido, hubiera sido imposible realizar un análisis siquiera decente para el proyecto.

Al aplicar el filtro de edad, el dataset pasa de ~235 mil filas útiles a ~172 mil, una reducción de aproximadamente el 27 % del total.

### Acotación de variables

Para realizar el análisis de manera más efectiva se descartaron las variables que no aportan información útil o de las que no se tenía suficiente contexto, pasando el dataset de **78 columnas a 14**.

Se eliminaron las variables:

```
"DIRECTORIO", "SECUENCIA_ENCUESTA", "SECUENCIA_P", "ORDEN", "FEX_C", "P6051", "P3510S1",
"P3510S1A1", "P3510S1A2", "P3510S1A3", "P3510S1A4", "P3510S1A5", "P3510S1A6", "P3510S1A7",
"P3510S1A8", "P3510S1A9", "P3510S1A10", "P3510S1A11", "P3510S2", "P3510S2A1", "P3510S2A2",
"P3510S2A4", "P3510S2A5", "P3510S2A6", "P3510S2A7", "P3510S2A8", "P3510S2A9", "P3510S2A10",
"P3510S2A11", "P6020", "P6034", "P6071", "P6071S1", "P756", "P756S1", "P756S2", "P756S3", "P3510",
"P3510S1", "P3510S2", "P3510S1A1", "P3510S1A2", "P3510S1A3", "P3510S1A4", "P3510S1A5", "P3510S1A6",
"P3510S1A7", "P3510S1A8", "P3510S1A9", "P3510S1A10", "P3510S1A11", "P3510S2A1", "P3510S2A2",
"P3510S2A3", "P3510S2A4", "P3510S2A5", "P3510S2A6", "P3510S2A7", "P3510S2A8", "P3510S2A9",
"P3510S2A10", "P3510S2A11", "P6074", "P755", "P755S1", "P755S2", "P755S3", "P754", "P753", "P753S1",
"P753S2", "P752", "P1662", "P6081", "P6081S1", "P6087", "P6083", "P6083S1", "P6088", "P2057", "P2059",
"P1901", "P2061", "P1903", "P1904", "P3038", "P753S3"
```

Para más información sobre el porqué de cada una, ver la tabla en la carpeta `data_cleaning`.

### Estandarización y traducción de datos

Para facilitar la legibilidad del código y el análisis, se reemplazaron los nombres de las columnas (que eran códigos) por una pequeña descripción:

`Es_Jefe_de_hogar`, `Tipo_Documento_identidad`, `Edad`, `Estado_civil`, `Autorreconocimiento_Étnico`, `Satisfaccion_vida`, `Satisfaccion_ingreso`, `Satisfaccion_salud`, `Satisfaccion_seguridad`, `Satisfaccion_trabajo`, `Satisfaccion_tiempo_libre`, `Sentido_proposito`, `Escalon_vida`, `Autorreconocimiento_sexual`

De la misma manera, se reemplazaron algunos números representantes de categorías por las categorías que representan:

**`Autorreconocimiento_Étnico`**

| Código | Categoría |
|---|---|
| 1 | Indígena |
| 2 | Gitano Rom |
| 3 | Raizal |
| 4 | Palenquero |
| 5 | Afrocolombiano |
| 6 | Ningún grupo |

**`Autorreconocimiento_sexual`**

| Código | Categoría |
|---|---|
| 1 | Hombre |
| 2 | Mujer |
| 3 | Hombre trans |
| 4 | Mujer trans |
| 5 | Otro |
| 9 | No responde |

**`Estado_civil`**

| Código | Categoría |
|---|---|
| 1 | Unión libre hace menos de 2 años |
| 2 | Unión libre hace 2 años o más |
| 3 | Viudo/a |
| 4 | Separado/Divorciado |
| 5 | Soltero/a |
| 6 | Casado/a |

Por último, para operar de manera más efectiva con la variable `Es_Jefe_de_hogar`, se modificaron sus valores: originalmente eran un rango muy amplio que no se correspondía adecuadamente con la información de la variable. Para simplificar el análisis, todos los valores distintos de 1 se convirtieron a 2, de modo que se pueda distinguir si se es o no jefe del hogar.

### Diccionario de datos

A continuación se presenta la información de las columnas que quedaron después de la limpieza y estandarización de los valores.

| Columna | Tipo | Significado | Ejemplo | Valores nulos | Proporción nulos | Tratamiento especial |
|---|---|---|---|---|---|---|
| `Es_jefe_del_hogar` | Nominal | Identifica el número de orden de la persona que proporciona información del hogar. | 1 (equivale a que es jefe del hogar) | 0 | 0 | Sí. Se renombraron los valores para que fueran más fáciles de manejar. |
| `Edad` | Numérica discreta | Indaga cuántos años cumplidos tiene la persona. | 20 | 0 | 0 | No |
| `Estado_civil` | Nominal | Indaga la situación conyugal actual de la persona. | Unión libre hace 2 años o más | 0 | 0 | Sí. Se renombraron los valores. |
| `Autorreconocimiento_etnico` | Nominal | Indaga con cuál grupo étnico se autorreconoce la persona según su cultura, pueblo o rasgos físicos. | Afrocolombiano | 0 | 0 | Sí. Se renombraron los valores. |
| `Satisfaccion_vida` | Ordinal | Qué tan satisfecha se siente la persona con su vida actualmente. | 9 | 0 | 0 | No |
| `Satisfaccion_ingreso` | Ordinal | Qué tan satisfecha se siente la persona con su ingreso actualmente. | 5 | 0 | 0 | Sí. Los valores 99 se modificaron a nulos. |
| `Satisfaccion_salud` | Ordinal | Qué tan satisfecha se siente la persona con su salud actualmente. | 3 | 0 | 0 | No |
| `Satisfaccion_seguridad` | Ordinal | Qué tan satisfecha se siente la persona con su nivel de seguridad actualmente. | 7 | 0 | 0 | No |
| `Satisfaccion_trabajo` | Ordinal | Qué tan satisfecha se siente la persona con su trabajo o actividad actualmente. | 1 | 0 | 0 | No |
| `Satisfaccion_tiempo_libre` | Ordinal | Qué tan satisfecha se siente la persona con su tiempo libre. | 4 | 0 | 0 | No |
| `Sentido_proposito` | Ordinal | Qué tanto considera la persona que las cosas que hace en su vida valen la pena. | 10 | 0 | 0 | No |
| `Escalon_de_la_vida` | Ordinal | En qué escalón de una escalera de 0 a 10 diría la persona que se encuentra actualmente. | 6 | 0 | 0 | No |
| `Autorreconocimiento_sexual` | Nominal | Cómo se reconoce la persona en términos de orientación sexual. | Mujer trans | 0 | 0 | Sí. Se renombraron los valores. |
| `Bienestar_emocional` | Numérica continua | Suma de `Satisfaccion_vida`, `Satisfaccion_tiempo_libre`, `Sentido_proposito` y `Escalon_de_la_vida`. | 7.75 | 0 | 0 | No |
| `Bienestar_material` | Numérica continua | Suma de `Satisfaccion_ingreso`, `Satisfaccion_salud`, `Satisfaccion_seguridad` y `Satisfaccion_trabajo`. | 1.75 | 0 | 0 | No |
| `Bienestar_general` | Numérica continua | Suma de `Bienestar_emocional` y `Bienestar_material`. | 6.12 | 0 | 0 | No |

Como se puede observar en la tabla, no se hizo ningún tratamiento especial en los datos, puesto que después de eliminar las columnas innecesarias no quedaron valores nulos.

---

## 3. Análisis descriptivo

Para tener una aproximación más enfocada e integral del bienestar, se decidió por convención crear 3 variables nuevas:

- **`Bienestar_material`**: agrupa la percepción del bienestar en los aspectos de trabajo, salud, seguridad e ingreso.
- **`Bienestar_emocional`**: agrupa satisfacción de vida, sentido de propósito, tiempo libre y escalón de la vida.
- **`Bienestar_general`**: compuesta en partes iguales por las dos anteriores.

En las dos primeras variables, cada factor que las compone tiene un peso del 25 %. Esto se decidió por convención, porque se considera que afectan de manera igual al bienestar. Sin embargo, por cómo está planteado el código, es posible cambiar los porcentajes con asesoría profesional futura para una mayor precisión.

### Distribución de variables

Para la mayor parte de los valores se escogió el promedio, pues se consideró una medida suficientemente correcta para representar su distribución. Para ver gráficas y otras medidas, ver **XXX**.

**`Es_Jefe_de_hogar`**

- 1 (jefe/a de hogar): 51.01 %
- 2 (no es jefe/a de hogar): 48.98 %

**`Edad`**

- Media: 45.84

**`Estado_civil`**

| Categoría | Porcentaje |
|---|---|
| Unión libre hace 2 años o más | 33 % |
| Soltero/a | 24.86 % |
| Casado/a | 20.79 % |
| Separado/Divorciado | 12.98 % |
| Viudo/a | 6.18 % |
| Unión libre hace menos de 2 años | 2.12 % |

**`Autorreconocimiento_Étnico`**

| Grupo | Registros |
|---|---|
| Ningún grupo | 139.159 |
| Indígena | 18.594 |
| Afrocolombiano | 13.499 |
| Raizal | 690 |
| Palenquero | 58 |
| Gitano/a (Rom) | 25 |

**Variables de satisfacción**

| Variable | Medida |
|---|---|
| `Satisfaccion_vida` | 8.14 (media) |
| `Satisfaccion_trabajo` | 7.35 (media) |
| `Satisfaccion_ingreso` | 8.0 (moda) |
| `Satisfaccion_salud` | 7.83 (media) |
| `Satisfaccion_tiempo_libre` | XXX |
| `Sentido_proposito` | XXX |
| `Escalon_vida` | XXX |
| `Autorreconocimiento_sexual` | XXX |

### Valores atípicos

**`Satisfaccion_ingreso`:** aunque su rango es de 0 a 10, se encontraron 21.681 registros con el valor `99`, de un total de 172.025 (aproximadamente 12.6 %). Estos valores se interpretaron como nulos. No se les asignó un valor de 9 (una posibilidad), por falta de contexto. Al calcular el bienestar material, los registros con este valor se calculan de manera distinta; no se descartan del todo porque sus otros valores son válidos.

**`Edad`:** a primera vista podría pensarse que hay valores atípicos, pues según el boxplot realizado hay **XXX** personas con una edad superior a **XXX**. Sin embargo, se decidió no eliminarlos: aunque es una edad alta, es posible e incluso normal. Según registros del DANE, hay aproximadamente 37 personas mayores de 100 años por cada 100.000 habitantes. Además, sus valores en las otras variables son normales.

---

## 4. Hallazgos

- Se encontró una correlación clara entre el bienestar material y el bienestar emocional, siendo de **XXX**.
- Hay diferencias modestas entre la distribución de la felicidad para las distintas etnias, siendo que en promedio **XXX**.

---

## 5. Reflexión

### ¿Qué fue lo más difícil al momento de transformar una necesidad de información en una pregunta analítica?

Lo más difícil de pasar de la necesidad a la pregunta es saber elegir correctamente, porque se daban 3 temas y cada tema tenía un enfoque diferente, por lo que iban a dar una respuesta para un grupo determinado de personas, lo que implica dejar a los otros grupos afuera. También influye el valor ético, puesto que el informe presentado va a influir en decisiones de la Alcaldía que afectarán el estilo de vida de las personas.

Teniendo en cuenta esto, se evaluaron las distintas posibilidades de preguntas en cada tema y se eligió la que permite abarcar más personas. La pregunta elegida permite abarcar relaciones entre diferentes tipos de bienestar para llegar a un bienestar general; también abarca el bienestar y la diversidad de la población, puesto que el análisis se enfoca en agrupar por etnias a las personas encuestadas; y finalmente abarca las características de la población, para poder dar un análisis más completo. En conclusión, fue difícil elegir la pregunta correcta que pudiera abarcar las necesidades de la Alcaldía y, al mismo tiempo, ser relevante para ayudar a la población.

Por otro lado, también fue difícil definir cómo medir el bienestar: qué factores tener en cuenta y qué porcentaje darle a cada uno, para así definir un bienestar en el cual basar el análisis.

### ¿Qué decisiones tuvieron que tomar durante la selección y preparación de los datos y cómo influyeron estas decisiones en el análisis?

Después de aplicar los filtros pedidos, se hizo la preparación de los datos para eliminar columnas que no brindaban información relevante para el análisis planeado y para estandarizar valores.

Primero se eliminaron las columnas innecesarias para el análisis planteado o de las que no había descripción para entender qué información brindaban. Con este proceso se pasó de 78 columnas a 14, lo cual resultó bastante útil, puesto que varias columnas tenían valores vacíos ya que sus preguntas no abarcaban respuestas posibles para todas las personas encuestadas, lo que permitió que el dataset final no contuviera valores nulos.

Finalmente se estandarizaron los valores, ya que la información estaba codificada únicamente en números, lo cual resultaba confuso y tedioso porque había que revisar constantemente qué era cada cosa. Por eso se convirtieron los números en las categorías que representaban y se cambiaron los nombres de las columnas, que también venían en código numérico.

Como resultado, el dataset final tiene **172.025 filas y 14 columnas**, sin valores nulos, y presenta el valor de cada pregunta de una manera más legible, permitiendo un mejor análisis.

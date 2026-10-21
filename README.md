En este taller usaremos **Apache Spark (PySpark)** para explorar los datos públicos de IMDb desde nuestro propio computador. Se va a dar una demostración durante el taller, pero opcionalmente, al seguir estas instrucciones (idealmente antes del taller, o en caso contrario cuando puedas), puedes también correr todo desde tu computador.

Las instrucciones sirven para **Windows, macOS y Linux**.

> Esitimo que se necesitará en el orden de **20 GB de espacio libre en disco** (idealmente más).

## 1. Descargar los datos de IMDb

Descarga los siguientes siete archivos desde <https://datasets.imdbws.com/>:

- `name.basics.tsv.gz`
- `title.akas.tsv.gz`
- `title.basics.tsv.gz`
- `title.crew.tsv.gz`
- `title.episode.tsv.gz`
- `title.principals.tsv.gz`
- `title.ratings.tsv.gz`

Guárdalos juntos en una carpeta, por ejemplo, `big-data-cine/data/`.

**No es necesario descomprimirlos:** Spark puede leer directamente los archivos `.tsv.gz`.

Los archivos contienen datos en formato **TSV** (*tab-separated values*, similar a CSV, pero con tabs como separadores). La descripción de cada archivo está en la [documentación de los conjuntos de datos no comerciales de IMDb](https://data.imdb.com/non-commercial-datasets/). (Solo se pueden usar estos datos para fines no comerciales.)

## 2. Comprobar Java

Necesitamos **Java 17 o una versión posterior compatible con Spark**. Abre una terminal (en Windows, **PowerShell**) y ejecuta:

```bash
java -version
```

Deberías ver algo parecido a:

```text
openjdk version "17.0.x" ...
```

Si aparece una versión 17 o superior, puedes continuar. Si Java no está instalado o la versión es anterior, instala un JDK desde [Eclipse Temurin (Adoptium)](https://adoptium.net/).

En Windows, durante la instalación, habilita la opción **“Set or override JAVA_HOME variable”** y, si está disponible, la opción para agregar Java a `PATH`. Cierra y vuelve a abrir PowerShell antes de repetir `java -version`.

Si todavía hay problemas, comprueba `JAVA_HOME`:

**Windows (PowerShell):**

```powershell
$env:JAVA_HOME
```

**macOS o Linux:**

```bash
printenv JAVA_HOME
```

La ruta debería apuntar a la carpeta del JDK, por ejemplo, `C:\Program Files\Eclipse Adoptium\jdk-17...` en Windows. La ubicación exacta depende del sistema. Verifica de nuevo con `java -version`.

## 3. Comprobar Python

Necesitamos **Python 3.11 o posterior**.

Prueba uno de estos comandos en la terminal:

```bash
python --version
```

```bash
python3 --version
```

En Windows también puedes probar:

```powershell
py --version
```

Si aparece una versión adecuada (por ejemplo, `Python 3.12.x`), continúa. Si no, instala una versión estable desde <https://www.python.org/downloads/>.

> **Importante:** en los siguientes pasos usamos `python` como ejemplo. Si en tu computador funciona `python3` o `py`, reemplaza `python` por ese comando.

## 4. Instalar PySpark y JupyterLab

En la terminal, ejecuta:

```bash
python -m pip install pyspark==4.2.0 jupyterlab
```

La instalación puede tardar algunos minutos.

Si aparece una advertencia similar a esta, **puedes ignorarla para este taller**:

```text
WARNING: The script ... is installed in ... which is not on PATH.
```
(Otras advertencias o errores, sí podrían requerir atención en el contexto de tu sistema operativo o tu computador. Ojalá no haya.)

> **Nota:** si tu sistema no permite instalar paquetes en el entorno global de Python, utiliza un entorno virtual (`venv`).

## 5. Iniciar JupyterLab

Desde la terminal, ejecuta:

```bash
python -m jupyterlab
```

JupyterLab debería abrirse automáticamente en tu navegador. Si no ocurre, copia en el navegador la dirección local que aparezca en la terminal (normalmente comienza con `http://localhost:8888/`). **No cierres la terminal** mientras estés usando JupyterLab.

## 6. Comprobar que Spark funciona

En JupyterLab, selecciona **Notebook → Python 3** (o el kernel de Python disponible).

Copia el siguiente código en la primera celda:

```python
from pyspark.sql import SparkSession

spark = (
    SparkSession.builder
    .master("local[*]")
    .appName("big-data-cine")
    .getOrCreate()
)

spark.range(2).count()
```

Haz clic en el botón **▶** o presiona **Shift + Enter**.

Espera unos segundos (la primera ejecución puede tardar un poco más). Debajo de la celda debería aparecer:

```text
2
```

**¡Eso!** Si aparece `2`, ya tienes PySpark y JupyterLab funcionando y queda todo listo para el taller.

## ¿Problemas?

- **`python` no se reconoce:** prueba `python3` o, en Windows, `py`.
- **`No module named pip` o `No module named jupyterlab`:** revisa que estés usando el mismo intérprete de Python para instalar y ejecutar JupyterLab.
- **`java` no se reconoce:** revisa la instalación de Java, `PATH` y `JAVA_HOME`; después abre una terminal nueva.
- **¿Otros?** Al usar nuestros propios computadores, es difícil controlar todas las permutaciones de sistemas operativos, software ya instalado, recursos disponibles, etc. Puedes intentar Googlear o (con cuidado) usar la IA para resolver el problema. Pero es opcional: durante el taller vamos a ver una demostración.

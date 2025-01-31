# IllustrativeMath-GrupoLEMA

Prototipo del material de [Matemáticas Ilustrativas](https://curriculum.illustrativemathematics.org) en versión [Pretext](https://pretextbook.org).

## Para generar versiones web(html), pdf, latex

Debe tener [Pretext](https://pretextbook.org) instalado. Desde la terminal en la carpeta principal (la que contiene este archivo y el archivo `project.ptx`), ejecutar los siguientes comandos.

Todos los archivos que se generan se guardan en la carpeta `output/`.

### generar versiones del estudiante

Para generar la página web (`gra3-uni4` significa `grado3-unidad4`):

```bash
pretext build gra3-uni4-web-est
```

Para ver localmente la página web:

```bash
pretext view gra3-uni4-web-est
```

Para generar el pdf:

```bash
pretext build gra3-uni4-print-est
```

Para generar el código latex que produce el pdf:

```bash
pretext build gra3-uni4-print-latex-est
```

### generar versiones del profesor

Cambiar `-est` por `-prof` en todos los comandos.

Ambas versiones (estudiante y profesor) se generan a partir de los mismos archivos fuente. Los elementos que contienen `component="profesor"` solo son visibles en la versión del profesor.


## Componentes en el código fuente
El en source, para crear un elemento que solo 
*  ve el profesor, poner `component="profesor"`
*  ve el estudiante, poner `component="estudiante"`
*  se ve en la versión web, poner `component="web"`

Por ejemplo:
```xml
<objectives component="estudiante">
  <ul>
    <li>Exploremos las fichas de dos colores y los tableros de 5.</li>
  </ul>
</objectives>

<objectives component="profesor">
  <ul>
    <li>Usar fichas de dos colores y los tableros de 5.</li>
    <li>Parafrasear ideas matemáticas de un compañero.</li>
  </ul>
</objectives>

<ul>
  <li>fichas de dos colores</li>
  <li>tableros de 5 <url component="web" href="external/blm/pdf-source/tableros-de-5.pdf">(ver pdf)</url></li>
</ul>
```


## Ajustes globales

### Preludios y postludios a las actividades en las guías del docente
Los comentarios para los profesores en las guías del docente para cada actividad se encuentran dentro de las tags `<prelude>` y `<postlude>`. Por defecto, todo lo que está en `<prelude>` aparece antes del enunciado, y todo lo que está en `<postlude>` aparece después del enunciado, pero esto se puede ajustar. Si se quiere que ambas aparezcan después del enunciado de la actividad, se debe cambiar el archivo `pretext-html.xsl` de PreTexT que determina como se produce la página web. Busque las líneas que incluyen el `<prelude>`
```xml
<!-- prelude beforehand, when original -->
<xsl:if test="$b-original">
    <xsl:apply-templates select="prelude">
        <xsl:with-param name="b-original" select="$b-original" />
    </xsl:apply-templates>
</xsl:if>
```
 y muévalas justo antes del `<postlude>`. Se debe hacer lo mismo con el archivo `pretext-latex.xsl` para ajustar el orden en el que aparecen en el código latex que produce Pretext.

## Generar archivos de imágenes
Las imágenes por lo general es mejor insertarlas sin extensión. Así;
```xml
<image source="svg-source/tikz-file-147472">
   <shortdescription>30 objetos en grupos de 5</shortdescription>
</image>
```
Cuando no se incluye la extensión, Pretext se encarga automáticamente de agregar la extensión `.svg` para las imágenes en la página web y la extensión `.pdf` para las imágenes para generar el pdf para impresión con LaTeX (en este caso, la página web buscaría la imagen `tikz-file-147472.svg` y latex buscaría la imagen `tikz-file-147472.pdf`). En particular, se deben tener todos los formatos necesarios para una misma imagen.

En el caso de las imágenes que son originalmente `svg`, la conversión a `pdf` es mejor hacerla con `rsvg-convert` usando el comando
```bash
rsvg-convert --format=pdf --zoom=1.333333 tikz-file-147472.svg > tikz-file-147472.pdf
```
(el `--zoom=1.333333` es por un asunto de conversión entre pt (puntos) y px (pixeles) para preservar el mismo tamaño del archivo `svg`)

Para instalar `rsvg-convert` en MacOS usar `brew install librsvg` con `homebrew`.

En la carpeta de `scripts` hay varios scripts para automatizar estas conversiones.

También se podría usar ImageMagick, pero la conversión de svg a pdf produce archivos muy grandes.

Si no se quiere usar como formato por defecto `pdf` para las imágenes en latex, se debe editar el archivo `pretext-latex.xsl` de la instalación de Pretext. Busque `.pdf` para encontrar las líneas que determinan la extensión que se le agrega a las imágenes:
 ```xml
<xsl:if test="$extension = ''">
    <xsl:text>.pdf</xsl:text>
</xsl:if>
```
Por ejemplo, si prefiere usar imágenes en png, cambie el `.pdf` a `.png`.

## Licencia

Mientras se descifran los detalles de copyright y licencias, este material es ©Enrique Acosta ([enriqueacosta.github.io](https://enriqueacosta.github.io)) y se publica bajo una licencia Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY NC SA 4.0). En breve e incompleto (los detalles están en las licencias), **tiene toda libertad para adaptar, copiar y distribuir este material siempre y cuando no lo use para fines comerciales y le mantenga la misma licencia y dé la atribución correspondiente (mencione al Grupo LEMA(www.grupolema.org) y a Illustrative Mathematics)** . 

Ver una copia de la licencia en [creativecommons.org](https://creativecommons.org/licenses/by-nc-sa/4.0/).

Adaptado de IM K–5 Math v.I, © 2021 Illustrative Mathematics® [illustrativemathematics.org](https://curriculum.illustrativemathematics.org) en su versión en español en [im.kendallhunt.com](https://im.kendallhunt.com/K5_ES/curriculum.html), distribuido con una licencia Creative Commons Attribution 4.0 International License (CC BY 4.0). Ver detalles de esta licencia en https://creativecommons.org/licenses/by/4.0/.

Soluciones en español adaptadas de Open Up Resources © 2022, [openupresources.org](https://access.openupresources.org/curricula/our-k5-math). Publicadas bajo una licencia Creative Commons Attribution-NonCommercial 4.0 International license.

**Nota:** Las traducciones anteriormente mencionadas fueron lideradas y coordinadas por miembros del Grupo LEMA. Ver detalles en: [illustrativemathematics.org](https://curriculum.illustrativemathematics.org/k5/teachers/grade-1/course-guide/contributors.html).

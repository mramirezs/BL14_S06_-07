# Comandos básicos II y III

# Comandos Básicos de Linux para Bioinformática

Este repositorio contiene material educativo sobre comandos básicos de Linux utilizados frecuentemente en análisis bioinformáticos.

## Tabla de Contenidos

- [Introducción](#introducción)
- [Comandos de Búsqueda y Manipulación de Texto](#comandos-de-búsqueda-y-manipulación-de-texto)
  - [grep](#grep)
  - [sort](#sort)
  - [uniq](#uniq)
  - [tr](#tr)
  - [wc](#wc)
  - [rev](#rev)
  - [fold](#fold)
- [Editores de Flujo de Texto](#editores-de-flujo-de-texto)
  - [sed](#sed)
  - [awk](#awk)
- [Ejercicios Prácticos](#ejercicios-prácticos)

## Introducción

Este material está diseñado para estudiantes de Biología en el curso de Principios de Programación y Bioinformática. Estos comandos son fundamentales para la manipulación de archivos de datos biológicos, como secuencias FASTA, archivos de anotación de genes, y resultados de análisis genómicos.

## Comandos de Búsqueda y Manipulación de Texto

### grep

`grep` (globally search for a regular expression and print matching lines) es una potente herramienta para buscar patrones en archivos de texto.

**Sintaxis básica:**
```bash
grep PATRON archivo # Los patrones se ponen entre comillas dobles
```

**Opciones comunes:**

| Opción | Función |
|--------|---------|
| `-v` | Invierte la coincidencia (líneas que NO contienen el patrón) |
| `--color` | Colorea el texto coincidente |
| `-F` | Interpreta el patrón como cadena literal |
| `-H`, `-h` | Imprime o no el nombre del archivo |
| `-i` | Ignora mayúsculas/minúsculas |
| `-l` | Lista los nombres de archivos que contienen el patrón |
| `-n` | Imprime el número de línea |
| `-c` | Cuenta el número de coincidencias |
| `-o` | Solo imprime el patrón que coincide |
| `-w` | Fuerza a que el patrón coincida con palabras completas |
| `-x` | Coincidencia con líneas completas |
| `-f` | Obtiene patrones desde un archivo |
| `-E` | Interpreta el patrón como expresión regular extendida |

#### Actividad 1

```bash
nano references.txt
seq1 chr1 exp-AC02
seq1 CHR1 exp-CC33
seq2 chr1 exp-HT33
seq3 chr2 exp-FG90
seq3 chr2 exp-FG90
seq3 chr2 exp-FG90
seq3 chr2 exp-FG90
seq5 chr12 exp-0011
seq5 chr12 exp-0011
seq5 chr12 exp-0011
seq7 chr8 exp-0TT1
seq7 chr8 exp-0TT1
seq7 chr8 exp-0TT1
```

```bash
# Buscar "seq1" en un archivo
grep "seq1" references.txt

# Mostrar número de línea
grep -n "seq1" references.txt
grep -no "seq1" references.txt

# Contar apariciones
grep -c "seq1" references.txt

# Invertir búsqueda
grep -v "seq1" references.txt
```

```bash

# Usar expresiones regulares
grep -E '^a' palabras.txt     # Líneas que comienzan con 'a'
grep -E 'ab+c' palabras.txt   # Patrón con una o más 'b'
grep -E 'ab?c' palabras.txt   # Patrón con cero o una 'b'
grep -E '.r' palabras.txt     # Cualquier carácter seguido de 'r'

# Buscar sitios de reconocimiento de enzimas en archivos FASTA
grep -E "GT[CT][AG]AC" *.fasta
```

**Caso práctico: Búsqueda de sitios de restricción**

La enzima HincII tiene una secuencia de reconocimiento "degenerada" GT(Y)(R)AC, donde:
- Y puede ser C o T
- R puede ser G o A

Para buscar estos sitios en un archivo FASTA:
```bash
# Opción 1: Enumerar todas las posibilidades
grep -E "GTCAAC|GTCGAC|GTTAAC|GTTGAC" *.fasta

# Opción 2: Usar clases de caracteres
grep -E "GT[CT][AG]AC" *.fasta
```

### sort

`sort` permite ordenar líneas de archivos según diferentes criterios.

**Sintaxis básica:**
```bash
sort [opciones] [archivo]
```

**Opciones comunes:**

| Opción | Función |
|--------|---------|
| `-c` | Verifica si los datos están ordenados |
| `-r` | Ordena en orden inverso |
| `-n` | Ordena numéricamente |
| `-u` | Elimina duplicados al ordenar |
| `-k` | Ordena por columna específica |

**Ejemplos:**

```bash
# Verificar si un archivo está ordenado
sort -c genes.txt

# Ordenar y eliminar duplicados
sort -u genes.txt

# Guardar resultado en nuevo archivo
sort -u genes.txt -o genes_output.txt

# Ordenar en orden inverso
sort -r genes_output.txt

# Ordenar números
sort -n numeros.txt

# Ordenar por la segunda columna
sort -n -k 2 ordenar.txt
```

### uniq

`uniq` se utiliza para identificar o eliminar líneas duplicadas consecutivas.

**Sintaxis básica:**
```bash
uniq [opciones] [archivo]
```

**Opciones comunes:**

| Opción | Función |
|--------|---------|
| `-c` | Muestra el número de repeticiones |
| `-d` | Muestra solo líneas duplicadas |
| `-u` | Muestra solo líneas únicas (no repetidas) |
| `-i` | Ignora mayúsculas/minúsculas |

**Ejemplos:**

```bash
# Eliminar duplicados consecutivos
uniq uniq.txt

# Ignorar mayúsculas/minúsculas
uniq -i uniq.txt

# Contar ocurrencias
uniq -c uniq.txt

# Mostrar solo líneas únicas
uniq -u uniq.txt

# Mostrar solo duplicados
uniq -d uniq.txt
```

### tr

`tr` (translate) se usa para traducir, reemplazar o eliminar caracteres.

**Sintaxis básica:**
```bash
tr [opciones] conjunto1 conjunto2
```

**Ejemplos:**

```bash
# Reemplazar 'a' por 'A'
echo "Esto es una ejemplo" | tr 'a' 'A'

# Reemplazar todas las vocales por mayúsculas
echo "Esto es una ejemplo" | tr 'aeiou' 'AEIOU'

# Convertir todo a mayúsculas
echo "Esto es una ejemplo" | tr 'a-z' 'A-Z'

# Eliminar caracteres
echo "Esto es un ejemplo" | tr -d "e"

# Eliminar espacios
echo "AAA TTTTT GGGG" | tr -d " "

# Reemplazar caracteres no coincidentes
echo "AAA TTTTT GGGG" | tr -c "ATGN" "-"
```

### wc

`wc` (word count) cuenta líneas, palabras y caracteres en archivos.

**Sintaxis básica:**
```bash
wc [opciones] [archivo]
```

**Opciones comunes:**

| Opción | Función |
|--------|---------|
| `-c` | Cuenta bytes |
| `-m` | Cuenta caracteres |
| `-l` | Cuenta líneas |
| `-w` | Cuenta palabras |
| `-L` | Muestra la longitud de la línea más larga |

**Ejemplos:**

```bash
# Contar líneas, palabras y caracteres
wc sequence.txt

# Contar solo caracteres
wc -m sequence.txt

# Contar solo palabras
wc -w sequence.txt
```

### rev

`rev` invierte el orden de los caracteres en cada línea.

**Sintaxis básica:**
```bash
rev [archivo]
```

**Ejemplos:**

```bash
# Invertir una secuencia
echo "ATGC" | rev  # Produce "CGTA"

# Invertir secuencias en un archivo
rev sequence.txt
```

### fold

`fold` divide líneas largas en un ancho específico.

**Sintaxis básica:**
```bash
fold [opciones] [archivo]
```

**Ejemplos:**

```bash
# Descargar un genoma y formatearlo a 50 caracteres por línea
curl -s https://eutils.ncbi.nlm.nih.gov/entrez/eutils/efetch.fcgi?id=AF086833.2\&db=nuccore\&rettype=fasta > Ebola_genome.fasta
grep -v ">" Ebola_genome.fasta | fold -w 50

# Cambiar a 30 caracteres por línea
grep -v ">" Ebola_genome.fasta | fold -w 30
```

## Editores de Flujo de Texto

### sed

`sed` (stream editor) es un editor de flujo de texto para realizar transformaciones.

**Sintaxis básica:**
```bash
sed [opciones] 'orden' [archivo]
```

**Opciones comunes:**

| Opción | Función |
|--------|---------|
| `-n` | Suprime la impresión automática |
| `-e` | Permite múltiples comandos |
| `-i` | Edita archivos in situ |
| `-f` | Lee comandos desde un archivo |

**Órdenes comunes:**

| Orden | Función |
|--------|---------|
| `p` | Imprime líneas |
| `d` | Elimina líneas |
| `s` | Sustituye texto |
| `i\` | Inserta texto antes de una línea |
| `a\` | Agrega texto después de una línea |
| `c\` | Cambia líneas completas |

**Ejemplos:**

```bash
# Imprimir todas las líneas (duplicadas por default)
sed 'p' dias.txt

# Imprimir líneas específicas
sed -n '1p' dias.txt          # Primera línea
sed -n '1,5p' dias.txt        # Líneas 1-5
sed -n '1~2p' dias.txt        # Líneas impares

# Eliminar líneas
sed '1d' dias.txt             # Elimina primera línea
sed '1,5d' dias.txt           # Elimina líneas 1-5
sed '1~2d' dias.txt           # Elimina líneas impares

# Insertar texto
sed '7i\Sabado_again' dias.txt    # Inserta antes de línea 7
sed '7a\Sabado_again' dias.txt    # Inserta después de línea 7
sed '7c\Sabado_again' dias.txt    # Reemplaza línea 7

# Sustituir texto
sed 's/Lunes/Inicio de semana/' dias.txt              # Primera ocurrencia
sed 's/Lunes/Inicio de semana/g' dias.txt             # Todas las ocurrencias
sed 's/on/forward/2' song.txt                         # Segunda ocurrencia

# Modificar archivo original (con backup)
sed -i.back '1~2d' dias.txt

# Combinar con expresiones regulares
sed -E 's/^.*at/REPLACED/' song.txt                   # Reemplaza desde inicio hasta 'at'
sed -E '3,5 s/it/IT/' song.txt                        # Reemplaza 'it' en líneas 3-5
```

**Ejemplos adicionales:**

```bash
# Reemplazar 'chr' con 'CHR' en todo el archivo
sed 's/chr/CHR/g' data.txt

# Imprimir líneas específicas
sed -n -e '3p' -e '5p' data.txt
sed -n '3p;5p' data.txt

# Insertar texto en posición específica
sed '3i\seq5 chr5 33' data.txt

# Eliminar líneas vacías
sed -E '/^$/d' data.txt
```

### awk

`awk` es un lenguaje de programación y procesador de texto para manipular datos estructurados.

**Sintaxis básica:**
```bash
awk 'patrón {acción}' archivo
```

**Características principales:**
- Opera línea por línea
- Divide cada línea en campos ($1, $2, ...)
- $0 representa la línea completa
- Permite condiciones y operaciones complejas

**Variables internas útiles:**

| Variable | Significado |
|----------|-------------|
| `NF` | Número de campos en la línea actual |
| `NR` | Número de registro (línea) actual |
| `FS` | Separador de campo (por defecto espacio) |
| `RS` | Separador de registro (por defecto nueva línea) |
| `OFS` | Separador de campo de salida |
| `ORS` | Separador de registro de salida |

**Ejemplos:**

```bash
# Imprimir archivo completo
awk '{print}' song.txt

# Imprimir líneas que coinciden con un patrón
awk '/people/' song.txt

# Imprimir campos específicos
awk '/s$/{print $1}' song.txt    # Primer campo de líneas que terminan en 's'

# Usar separador personalizado
awk -F '-' '/^a/{print $1}' word_guion.txt    # Primer campo usando '-' como separador

# Contar campos
awk '{print NF}' references2.txt

# Filtrar por número de campos
awk 'NF>4 {print $0}' references2.txt

# Filtrar por valor de campo
awk '{if ($4 > 20) print $0}' references2.txt

# Realizar cálculos
awk 'BEGIN{suma=0}{suma += $4}END{print suma}' references2.txt    # Suma los valores del campo 4
```

**Ejemplos adicionales:**

```bash
# Filtrar líneas por valor de campo
awk '{if($3!=33) print $0}' data.txt          # Valores diferentes de 33
awk '{if($3==182) print $0}' data.txt         # Valores iguales a 182
awk '{if($3>=50) print $0}' data.txt          # Valores mayores o iguales a 50

# Calcular promedio
sed '/^$/d' data.txt | awk 'BEGIN{avg=0}{avg +=$3}END{print avg/NR}'
```

## Ejercicios Prácticos

### Ejercicio 1: Análisis de datos genómicos

Descargar datos de la base de datos SGD (Saccharomyces Genome Database):

```bash
# Crear directorio de trabajo
mkdir sgd_analysis && cd sgd_analysis

# Descargar archivos
wget http://downloads.yeastgenome.org/curation/chromosomal_feature/SGD_features.tab
wget http://downloads.yeastgenome.org/curation/chromosomal_feature/SGD_features.README

# Verificar descarga
ls -la

# Ver datos página a página
less SGD_features.tab

# Contar líneas en el archivo
wc -l SGD_features.tab

# Ver primeras líneas
head SGD_features.tab

# Buscar gen específico
grep "YAL060W" SGD_features.tab > YAL060W_info.txt

# Colorear la búsqueda
grep --color "YAL060W" SGD_features.tab

# Contar líneas que NO contienen "Dubious"
grep -cv "Dubious" SGD_features.tab
```

### Ejercicio 2: Manipulación de secuencias

Crear un archivo de secuencias y realizar operaciones:

```bash
# Crear archivo con secuencias
cat > sequences.fasta << EOF
>seq1
ATGGCTAGCTAGCTAGCTACGTAGCTACGTAGC
>seq2
CGTACGTAGCTAGCTAGCTATCGATCGATCGAT
>seq3
GCATGCATGCATGCATGCATGCATGCATGCATG
EOF

# Formatear secuencias a 20 caracteres por línea
grep -v ">" sequences.fasta | fold -w 20

# Invertir secuencias
grep -v ">" sequences.fasta | rev

# Contar frecuencia de nucleótidos
grep -v ">" sequences.fasta | grep -o . | sort | uniq -c

# Reemplazar T por U (DNA a RNA)
grep -v ">" sequences.fasta | tr 'T' 'U'
```

### Ejercicio 3: Filtrado de datos

Crear un archivo de datos tabulados y realizar filtros:

```bash
# Crear archivo de datos
cat > gene_expression.tsv << EOF
gene_id	sample1	sample2	sample3	fold_change	p_value
GENE001	245.6	189.3	201.5	1.2	0.03
GENE002	56.7	59.2	61.8	0.9	0.78
GENE003	1024.5	3560.2	2487.9	3.1	0.0001
GENE004	78.3	82.1	79.5	1.0	0.92
GENE005	567.8	123.4	345.6	0.4	0.02
EOF

# Filtrar genes con fold_change > 1
awk -F '\t' '$5 > 1 {print $0}' gene_expression.tsv

# Filtrar genes estadísticamente significativos
awk -F '\t' '$6 < 0.05 {print $1, $5, $6}' gene_expression.tsv

# Calcular promedio de expresión en sample1
awk -F '\t' 'NR>1 {sum+=$2} END {print sum/(NR-1)}' gene_expression.tsv
```

---

## Recursos Adicionales

- [GNU Coreutils Documentation](https://www.gnu.org/software/coreutils/manual/)
- [Bioinformatics Data Skills](https://www.oreilly.com/library/view/bioinformatics-data-skills/9781449367480/)
- [Linux Command Line for Bioinformatics](https://bioinformaticsworkbook.org/Appendix/Unix/unix-basics-1.html)

---

*Material preparado por Frank Guzman Escudero y Manuel Ramírez Sáenz, 2025-01*

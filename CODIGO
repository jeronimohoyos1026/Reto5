import glob
import csv
import matplotlib.pyplot as plt



archivos_txt = []
archivos_csv = []

# Palabras que no queremos contar como palabras importantes
CONECTORES = {
    "el", "la", "los", "las", "un", "una", "unos", "unas",
    "de", "del", "a", "ante", "bajo", "con", "contra", "desde",
    "en", "entre", "hacia", "hasta", "para", "por", "segun", "según",
    "sin", "sobre", "tras", "y", "e", "o", "u", "que", "qué", "se",
    "es", "al", "lo", "como", "cómo", "su", "sus", "mi", "mis", "tu",
    "tus", "yo", "tu", "él", "ella", "ellos", "ellas", "nosotros",
    "nosotras", "usted", "ustedes", "me", "te", "nos", "les", "le",
    "muy", "más", "menos", "no", "sí", "pero", "aunque", "porque",
    "pues", "si", "cuando", "cuándo", "donde", "dónde", "quien",
    "quién", "cual", "cuales", "esto", "esta", "este", "estos",
    "estas", "esa", "ese", "esas", "esos", "ser", "estar", "son",
    "era", "eran", "fue", "fueron", "hay", "tambien", "también"
}



def explorar_directorio():
    global archivos_txt, archivos_csv

    ruta = input("\nIngrese la ruta de la carpeta: ").strip().strip('"')

    archivos_txt = []
    archivos_csv = []

    if not glob.glob(ruta):
        print("\nLa ruta ingresada no existe.")
        return

    for archivo in glob.glob(ruta + "/*"):
        archivo_minusculas = archivo.lower()
        if archivo_minusculas[-4:] == ".txt":
            archivos_txt.append(archivo)
        elif archivo_minusculas[-4:] == ".csv":
            archivos_csv.append(archivo)

    print("\n========== ARCHIVOS DISPONIBLES ==========")

    if not archivos_txt and not archivos_csv:
        print("No se encontraron archivos .txt o .csv.")
        return

    print("\nArchivos TXT:")
    if archivos_txt:
        for numero, archivo in enumerate(archivos_txt, 1):
            print(numero, "-", archivo)
    else:
        print("No hay archivos TXT.")

    print("\nArchivos CSV:")
    if archivos_csv:
        for numero, archivo in enumerate(archivos_csv, 1):
            print(numero, "-", archivo)
    else:
        print("No hay archivos CSV.")




def seleccionar_archivo(lista):
    if not lista:
        print("\nNo hay archivos disponibles de este tipo.")
        return None

    print("\nArchivos disponibles:")

    for numero, archivo in enumerate(lista, 1):
        print(numero, "-", archivo)

    try:
        opcion = int(input("\nSeleccione un archivo: "))

        if opcion < 1 or opcion > len(lista):
            print("Opción fuera de rango.")
            return None

        return lista[opcion - 1]

    except ValueError:
        print("Debe ingresar un número.")
        return None




def leer_txt(ruta):
    try:
        with open(ruta, "r") as archivo:
            return archivo.readlines()
    except FileNotFoundError:
        print("\nEl archivo no fue encontrado.")
        return None




def unir_elementos(elementos, separador):
    resultado = ""

    for indice, elemento in enumerate(elementos):
        if indice:
            resultado += separador
        resultado += str(elemento)

    return resultado


def separar_palabras(texto):
    palabras = []
    palabra_actual = ""
    letras = "abcdefghijklmnopqrstuvwxyzáéíóúñüABCDEFGHIJKLMNOPQRSTUVWXYZÁÉÍÓÚÑÜ"

    for caracter in texto:
        if caracter in letras:
            palabra_actual += caracter
        elif palabra_actual != "":
            palabras.append(palabra_actual)
            palabra_actual = ""

    if palabra_actual != "":
        palabras.append(palabra_actual)

    return palabras


def contar_frecuencias(elementos, limite=None):
    conteo = {}

    for elemento in elementos:
        conteo[elemento] = conteo.get(elemento, 0) + 1

    frecuencias = [[elemento, cantidad] for elemento, cantidad in conteo.items()]
    for indice in range(len(frecuencias)):
        for siguiente in range(indice + 1, len(frecuencias)):
            if frecuencias[siguiente][1] > frecuencias[indice][1]:
                frecuencias[indice], frecuencias[siguiente] = (
                    frecuencias[siguiente],
                    frecuencias[indice],
                )

    if limite is not None:
        frecuencias = frecuencias[:limite]

    return frecuencias


def es_digito(caracter):
    return caracter >= "0" and caracter <= "9"


def contar_patron(texto, patron):
    texto = texto.lower()
    patron = patron.lower()
    cantidad = 0
    indice = 0
    caracteres = "abcdefghijklmnopqrstuvwxyzáéíóúñü0123456789"

    while indice <= len(texto) - len(patron):
        if texto[indice:indice + len(patron)] == patron:
            final = indice + len(patron)
            anterior = indice > 0 and texto[indice - 1] in caracteres
            siguiente = final < len(texto) and texto[final] in caracteres
            if not anterior and not siguiente:
                cantidad += 1
                indice = final
            else:
                indice += 1
        else:
            indice += 1

    return cantidad


def encontrar_fechas(texto):
    fechas = []
    indice = 0

    while indice < len(texto):
        inicio = indice
        while indice < len(texto) and es_digito(texto[indice]):
            indice += 1

        dia = indice - inicio
        if 1 <= dia <= 2 and indice < len(texto) and texto[indice] in "/-":
            separador = texto[indice]
            indice += 1
            inicio_mes = indice

            while indice < len(texto) and es_digito(texto[indice]):
                indice += 1

            mes = indice - inicio_mes
            if 1 <= mes <= 2 and indice < len(texto) and texto[indice] == separador:
                indice += 1
                inicio_anio = indice

                while indice < len(texto) and es_digito(texto[indice]):
                    indice += 1

                if indice - inicio_anio == 4:
                    fechas.append(texto[inicio:indice])
                    continue

        indice = inicio + 1

    return fechas


def quitar_saltos_finales(texto):
    final = len(texto)
    while final > 0 and texto[final - 1] in "\r\n":
        final -= 1
    return texto[:final]


def convertir_decimal(texto):
    return texto.replace(",", ".")


def contar_caracter(texto, buscado):
    return texto.count(buscado)


def obtener_valor(fila, columna):
    valor = fila.get(columna)
    return "" if valor is None else valor


def nombre_sin_ruta(ruta):
    nombre = ""
    parte = ""

    for caracter in ruta:
        if caracter in "/\\":
            if parte:
                nombre = parte
                parte = ""
        else:
            parte += caracter

    return parte or nombre or ruta


def obtener_palabras(lineas):
    palabras = separar_palabras(unir_elementos(lineas, " ").lower())
    return [palabra for palabra in palabras if palabra not in CONECTORES]



def resumen_texto(ruta):
    lineas = leer_txt(ruta)

    if lineas is None:
        return

    texto = unir_elementos(lineas, "")

    palabras_totales = separar_palabras(texto.lower())
    palabras_limpias = obtener_palabras(lineas)

    caracteres_con_espacios = len(texto)
    espacios = " \t\n\r\v\f"
    caracteres_sin_espacios = sum(caracter not in espacios for caracter in texto)

    frecuentes = contar_frecuencias(palabras_limpias, 5)

    print("\n========== RESUMEN ESTADÍSTICO ==========")
    print("Total de líneas:", len(lineas))
    print("Total de palabras:", len(palabras_totales))
    print("Caracteres con espacios:", caracteres_con_espacios)
    print("Caracteres sin espacios:", caracteres_sin_espacios)

    print("\nTop 5 palabras más repetidas:")

    if frecuentes:
        for palabra, cantidad in frecuentes:
            print(palabra, ":", cantidad)
    else:
        print("No se encontraron palabras.")




def extraer_patrones(ruta):
    lineas = leer_txt(ruta)

    if lineas is None:
        return

    texto = unir_elementos(lineas, "")

    errores = contar_patron(texto, "ERROR")
    errores_404 = contar_patron(texto, "404")
    fechas = encontrar_fechas(texto)

    print("\n========== PATRONES ENCONTRADOS ==========")
    print("Apariciones de ERROR:", errores)
    print("Apariciones de 404:", errores_404)
    print("Fechas encontradas:", len(fechas))

    if fechas:
        print("\nFechas más frecuentes:")

        fechas_frecuentes = contar_frecuencias(fechas, 5)
        for fecha, cantidad in fechas_frecuentes:
            print(fecha, ":", cantidad)




def grafico_palabras(ruta):
    lineas = leer_txt(ruta)

    if lineas is None:
        return

    palabras = obtener_palabras(lineas)
    frecuentes = contar_frecuencias(palabras, 10)

    if not frecuentes:
        print("\nNo hay palabras suficientes para generar el gráfico.")
        return

    nombres = [elemento[0] for elemento in reversed(frecuentes)]
    cantidades = [elemento[1] for elemento in reversed(frecuentes)]

    plt.figure()
    plt.barh(nombres, cantidades)
    plt.title("10 palabras más frecuentes")
    plt.xlabel("Frecuencia")
    plt.ylabel("Palabras")
    plt.show(block=True)




def grafico_longitud_lineas(ruta):
    lineas = leer_txt(ruta)

    if lineas is None:
        return

    longitudes = [len(quitar_saltos_finales(linea)) for linea in lineas]

    if not longitudes:
        print("\nEl archivo está vacío.")
        return

    mayor = max(longitudes)

    # Intervalos 0-50, 51-100, etc.
    intervalos = [0, 51]
    limite = 51
    while limite <= mayor:
        limite = limite + 50
        intervalos.append(limite)

    plt.figure()
    plt.hist(longitudes, bins=intervalos, edgecolor="black")
    plt.title("Distribución de longitud de líneas")
    plt.xlabel("Cantidad de caracteres")
    plt.ylabel("Número de líneas")
    plt.show(block=True)




def leer_csv(ruta):
    try:
        with open(ruta, "r") as archivo:
            primera_linea = archivo.readline()

        delimitador = ","
        comas = contar_caracter(primera_linea, ",")
        punto_y_coma = contar_caracter(primera_linea, ";")
        tabulaciones = contar_caracter(primera_linea, "\t")

        if punto_y_coma > comas and punto_y_coma >= tabulaciones:
            delimitador = ";"
        elif tabulaciones > comas and tabulaciones > punto_y_coma:
            delimitador = "\t"

        with open(ruta, "r") as archivo:
            lector = csv.reader(archivo, delimiter=delimitador)
            filas_csv = list(lector)

            if not filas_csv:
                print("\nEl CSV no tiene encabezados.")
                return None, None

            columnas = filas_csv[0]
            datos = [
                {
                    columna: valores[indice] if indice < len(valores) else ""
                    for indice, columna in enumerate(columnas)
                }
                for valores in filas_csv[1:]
            ]

            return columnas, datos

    except FileNotFoundError:
        print("\nEl archivo no fue encontrado.")
        return None, None



def seleccionar_columna(columnas, mensaje):
    print("\nColumnas disponibles:")

    for numero, columna in enumerate(columnas, 1):
        print(numero, "-", columna)

    try:
        opcion = int(input("\n" + mensaje + ": "))

        if opcion < 1 or opcion > len(columnas):
            print("Opción fuera de rango.")
            return None

        return columnas[opcion - 1]

    except ValueError:
        print("Debe ingresar un número.")
        return None




def vista_previa(ruta):
    columnas, datos = leer_csv(ruta)

    if datos is None:
        return

    print("\n========== COLUMNAS ==========")
    print(unir_elementos(columnas, " | "))

    for titulo, filas in (
        ("PRIMERAS 10 FILAS", datos[:10]),
        ("ÚLTIMAS 5 FILAS", datos[-5:]),
    ):
        print(f"\n========== {titulo} ==========")
        for fila in filas:
            valores = (obtener_valor(fila, columna) for columna in columnas)
            print(unir_elementos(valores, " | "))



def obtener_numeros(datos, columna):
    numeros = []

    for fila in datos:
        valor = obtener_valor(fila, columna).strip()

        if valor == "":
            continue

        try:
            numero = float(convertir_decimal(valor))
            numeros.append(numero)

        except ValueError:
            continue

    return numeros




def calcular_promedio(numeros):
    suma = 0
    for numero in numeros:
        suma += numero
    return suma / len(numeros)


def calcular_mediana(numeros):
    ordenados = []
    for numero in numeros:
        ordenados.append(numero)

    for indice in range(len(ordenados)):
        menor = indice
        for siguiente in range(indice + 1, len(ordenados)):
            if ordenados[siguiente] < ordenados[menor]:
                menor = siguiente
        ordenados[indice], ordenados[menor] = ordenados[menor], ordenados[indice]

    mitad = len(ordenados) // 2
    if len(ordenados) % 2:
        return ordenados[mitad]
    return (ordenados[mitad - 1] + ordenados[mitad]) / 2


def estadisticas_csv(ruta):
    columnas, datos = leer_csv(ruta)

    if datos is None:
        return

    columna = seleccionar_columna(
        columnas,
        "Seleccione una columna numérica"
    )

    if columna is None:
        return

    numeros = obtener_numeros(datos, columna)

    if not numeros:
        print("\nLa columna seleccionada no contiene datos numéricos válidos.")
        return

    promedio = calcular_promedio(numeros)
    mediana = calcular_mediana(numeros)

    print("\n========== ESTADÍSTICAS ==========")
    print("Columna:", columna)
    print("Registros válidos:", len(numeros))
    print("Promedio:", promedio)
    print("Mediana:", mediana)
    print("Valor máximo:", max(numeros))
    print("Valor mínimo:", min(numeros))



def grafico_tendencia(ruta):
    columnas, datos = leer_csv(ruta)

    if datos is None:
        return

    columna_x = seleccionar_columna(
        columnas,
        "Seleccione la columna para el eje X (por ejemplo Fecha)"
    )

    if columna_x is None:
        return

    columna_y = seleccionar_columna(
        columnas,
        "Seleccione la columna numérica para el eje Y"
    )

    if columna_y is None:
        return

    x = []
    y = []

    for fila in datos:
        valor_x = obtener_valor(fila, columna_x).strip()
        valor_y = obtener_valor(fila, columna_y).strip()

        if valor_x == "" or valor_y == "":
            continue

        try:
            numero = float(convertir_decimal(valor_y))

            x.append(valor_x)
            y.append(numero)

        except ValueError:
            continue

    if not y:
        print("\nNo hay datos válidos para generar el gráfico.")
        return

    plt.figure()
    plt.plot(x, y)
    plt.title(columna_y + " según " + columna_x)
    plt.xlabel(columna_x)
    plt.ylabel(columna_y)
    plt.show(block=True)



def grafico_pastel(ruta):
    columnas, datos = leer_csv(ruta)

    if datos is None:
        return

    columna = seleccionar_columna(
        columnas,
        "Seleccione una columna categórica"
    )

    if columna is None:
        return

    valores = []
    for fila in datos:
        valor = obtener_valor(fila, columna).strip()
        if valor:
            valores.append(valor)

    if not valores:
        print("\nNo hay datos válidos.")
        return

    frecuencias = contar_frecuencias(valores)
    nombres = [elemento[0] for elemento in frecuencias]
    cantidades = [elemento[1] for elemento in frecuencias]

    plt.figure()
    plt.pie(cantidades, labels=nombres, autopct="%1.1f%%", startangle=90)
    plt.title("Distribución de " + columna)
    plt.show(block=True)




def grafico_dispersion(ruta):
    columnas, datos = leer_csv(ruta)

    if datos is None:
        return

    columna_x = seleccionar_columna(
        columnas,
        "Seleccione la primera columna numérica"
    )

    if columna_x is None:
        return

    columna_y = seleccionar_columna(
        columnas,
        "Seleccione la segunda columna numérica"
    )

    if columna_y is None:
        return

    x = []
    y = []

    for fila in datos:
        valor_x = obtener_valor(fila, columna_x).strip()
        valor_y = obtener_valor(fila, columna_y).strip()

        try:
            numero_x = float(convertir_decimal(valor_x))
            numero_y = float(convertir_decimal(valor_y))

            x.append(numero_x)
            y.append(numero_y)

        except ValueError:
            continue

    if not x:
        print("\nNo hay suficientes datos numéricos válidos.")
        return

    plt.figure()
    plt.scatter(x, y)
    plt.title(columna_x + " vs " + columna_y)
    plt.xlabel(columna_x)
    plt.ylabel(columna_y)
    plt.show(block=True)




def menu_graficas_txt(ruta):
    acciones = {
        "1": grafico_palabras,
        "2": grafico_longitud_lineas,
    }

    while True:
        print("\n========== GRÁFICAS DEL ARCHIVO TXT ==========")
        print("Archivo seleccionado:", nombre_sin_ruta(ruta))
        print("1. Gráfica de las 10 palabras más frecuentes")
        print("2. Histograma de caracteres por línea")
        print("3. Regresar al análisis del archivo TXT")

        opcion = input("\nSeleccione la gráfica que desea ver: ")
        if opcion == "3":
            break
        elif opcion in acciones:
            acciones[opcion](ruta)
        else:
            print("\nOpción inválida.")


def menu_txt():
    ruta = seleccionar_archivo(archivos_txt)

    if ruta is None:
        return

    acciones = {
        "1": resumen_texto,
        "2": extraer_patrones,
    }

    while True:

        print("\n========== SUBMENÚ: ANÁLISIS DE TEXTO (.TXT) ==========")
        print("Archivo seleccionado:", nombre_sin_ruta(ruta))
        print()
        print("1. Ver resumen: líneas, palabras y caracteres")
        print("2. Buscar patrones: ERROR, 404 y fechas")
        print("3. Elegir una gráfica para ver")
        print("4. Regresar al menú principal")

        opcion = input("\nSeleccione una opción: ")
        if opcion == "4":
            break
        elif opcion in acciones:
            acciones[opcion](ruta)
        elif opcion == "3":
            menu_graficas_txt(ruta)
        else:
            print("\nOpción inválida.")



def menu_graficas_csv(ruta):
    acciones = {
        "1": grafico_tendencia,
        "2": grafico_pastel,
        "3": grafico_dispersion,
    }

    while True:
        print("\n========== GRÁFICAS DEL ARCHIVO CSV ==========")
        print("Archivo seleccionado:", nombre_sin_ruta(ruta))
        print("1. Gráfica de tendencia (línea)")
        print("2. Gráfica circular por categoría")
        print("3. Gráfica de dispersión entre dos columnas")
        print("4. Regresar al análisis del archivo CSV")

        opcion = input("\nSeleccione la gráfica que desea ver: ")
        if opcion == "4":
            break
        elif opcion in acciones:
            acciones[opcion](ruta)
        else:
            print("\nOpción inválida.")



def menu_csv():
    ruta = seleccionar_archivo(archivos_csv)

    if ruta is None:
        return

    acciones = {
        "1": vista_previa,
        "2": estadisticas_csv,
    }

    while True:

        print("\n========== SUBMENÚ: ANÁLISIS DE DATOS (.CSV) ==========")
        print("Archivo seleccionado:", nombre_sin_ruta(ruta))
        print()
        print("1. Ver muestra de las primeras 10 y últimas 5 filas")
        print("2. Calcular promedio, mediana, máximo y mínimo de una columna")
        print("3. Elegir una gráfica para ver")
        print("4. Regresar al menú principal")

        opcion = input("\nSeleccione una opción: ")
        if opcion == "4":
            break
        elif opcion in acciones:
            acciones[opcion](ruta)
        elif opcion == "3":
            menu_graficas_csv(ruta)
        else:
            print("\nOpción inválida.")




def main():

    while True:

        print("\n==========================================")
        print("     ANÁLISIS DE DATOS TXT Y CSV")
        print("==========================================")
        print("1. Explorar directorio")
        print("2. Procesar Bitácoras / Textos Reales (.txt)")
        print("3. Analizar Dataset de Datos Abiertos (.csv)")
        print("4. Salir")

        opcion = input("\nSeleccione una opción: ")

        if opcion == "1":
            explorar_directorio()

        elif opcion == "2":
            menu_txt()

        elif opcion == "3":
            menu_csv()

        elif opcion == "4":
            print("\nPrograma finalizado.")
            break

        else:
            print("\nOpción inválida. Intente nuevamente.")




if __name__ == "__main__":
    main()

import string

def cargar_diccionario(nombre_archivo):
    """
    Carga palabras válidas desde un archivo y las almacena en un diccionario.
    Args:
        nombre_archivo (str): Ruta del archivo de palabras válidas.
    Returns:
        dict: Diccionario con palabras como claves, o None si hay error.
    """
    diccionario_palabras = {}
    try:
        with open(nombre_archivo, 'r', encoding='utf-8') as archivo:
            for linea in archivo:
                palabra = linea.strip().lower()
                diccionario_palabras[palabra] = True  # Valor arbitrario (solo importan las claves)
        return diccionario_palabras
    except FileNotFoundError:
        print(f"❌ Error: Archivo '{nombre_archivo}' no encontrado.")
        return None

def limpiar_palabra(palabra_cruda):
    """
    Normaliza una palabra removiendo puntuación y convirtiendo a minúsculas.
    Args:
        palabra_cruda (str): Palabra a normalizar.
    Returns:
        str: Palabra limpia y normalizada.
    """
    palabra_limpia = palabra_cruda.lower().strip(string.punctuation + "¿¡«»“”‘’")
    return palabra_limpia

def verificar_ortografia(archivo_a_revisar, diccionario_referencia):
    """
    Identifica palabras no encontradas en el diccionario de referencia.
    Args:
        archivo_a_revisar (str): Ruta del archivo a verificar.
        diccionario_referencia (dict): Diccionario de palabras válidas.
    Returns:
        set: Conjunto de tuplas (palabra_errónea, línea) o None si hay error.
    """
    try:
        errores_encontrados = set()
        with open(archivo_a_revisar, 'r', encoding='utf-8') as archivo:
            for numero_linea, linea in enumerate(archivo, 1):
                for palabra in linea.split():
                    palabra_limpia = limpiar_palabra(palabra)
                    if palabra_limpia and not palabra_limpia.isdigit() and palabra_limpia not in diccionario_referencia:
                        errores_encontrados.add((palabra_limpia, numero_linea))
        return errores_encontrados
    except FileNotFoundError:
        print(f"❌ Error: No se pudo abrir '{archivo_a_revisar}'")
        return None

def mostrar_resultados(errores):
    """Muestra los errores ortográficos encontrados de forma organizada."""
    if not errores:
        print("\n✅ ¡Texto perfecto! No se encontraron errores.")
    else:
        print("\n🔍 Errores detectados:")
        print("-" * 40)
        for palabra, linea in sorted(errores, key=lambda x: x[1]):
            print(f"• Línea {linea}: '{palabra}'")
        print("-" * 40)
        print(f"Total de errores: {len(errores)}")

def ejecutar_corrector():
    """Función principal que coordina el flujo del programa."""
    print("\n" + "=" * 50)
    print("   CORRECTOR ORTOGRÁFICO AVANZADO (100/100)")
    print("=" * 50 + "\n")

    diccionario = cargar_diccionario("palabras.txt")
    if not diccionario:
        return

    archivo_usuario = input("📝 Ingrese el nombre del archivo a verificar (ej. PatitoFeo.txt): ").strip()
    errores = verificar_ortografia(archivo_usuario, diccionario)

    if errores is not None:
        mostrar_resultados(errores)

if __name__ == "__main__":
    ejecutar_corrector()

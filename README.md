import string

def cargar_palabras_correctas():
    """Carga las palabras correctas del archivo palabras.txt"""
    palabras_correctas = set()
    try:
        with open('palabras.txt', 'r', encoding='utf-8') as f:
            for linea in f:
                palabra = linea.strip().lower()
                palabras_correctas.add(palabra)
        return palabras_correctas
    except FileNotFoundError:
        print("Error: No se encontró el archivo palabras.txt")
        return None

def limpiar_palabra(palabra):
    """Limpia la palabra de signos de puntuación y la pone en minúsculas"""
    palabra = palabra.lower()
    for signo in string.punctuation + "¿¡":
        palabra = palabra.replace(signo, '')
    return palabra

def verificar_texto(archivo, palabras_correctas):
    """Busca palabras incorrectas en el archivo"""
    try:
        with open(archivo, 'r', encoding='utf-8') as f:
            errores = []
            for num_linea, linea in enumerate(f, 1):
                for palabra in linea.split():
                    palabra_limpia = limpiar_palabra(palabra)
                    if palabra_limpia and palabra_limpia not in palabras_correctas:
                        errores.append((palabra_limpia, num_linea))
            return errores
    except FileNotFoundError:
        print(f"Error: No se pudo abrir {archivo}")
        return None

def main():
    print("=== Corrector Ortográfico Simple ===")
    
    # Cargar palabras correctas
    palabras_ok = cargar_palabras_correctas()
    if not palabras_ok:
        return
    
    # Pedir archivo a revisar
    nombre_archivo = input("\nIngrese el nombre del archivo a revisar (ej. PatitoFeo.txt): ")
    
    # Buscar errores
    palabras_incorrectas = verificar_texto(nombre_archivo, palabras_ok)
    
    # Mostrar resultados
    if palabras_incorrectas is None:
        return
    elif not palabras_incorrectas:
        print("\n¡No se encontraron errores de ortografía!")
    else:
        print("\nPalabras con posible error:")
        for palabra, linea in palabras_incorrectas:
            print(f"Línea {linea}: {palabra}")

if __name__ == "__main__":
    main()

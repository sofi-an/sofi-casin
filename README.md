import random
APUESTA_MINIMA = 1
SALDO_INICIAL = 100
MULTIPLICADOR_NUMERO = 20
MULTIPLICADOR_SECCION = 5
MULTIPLICADOR_COLOR = 2


secciones = {
    "1-12": list(range(1, 13)),
    "13-24": list(range(13, 25)),
    "25-36": list(range(25, 37))
}

menu_juegos = """1. Casino
2. Ruleta
3. Salir
Elige: """

sintesis_casino = """Casino: apuesta y gira la ruleta de las siguientes opciones:
Multiplicar la apuesta x 2
Restar 50 % a la apuesta
Ganar una cantidad fija
Perder una cantidad fija
Perder toda la apuesta"""

sintesis_ruleta = """Ruleta: se puede apostar por:
1. Un número específico (1-36). Si aciertas, ganas 20 veces lo apostado.
2. Una sección (1-12, 13-24, 25-36). Si aciertas, ganas 5 veces lo apostado.
3. Un color (rojo o negro). Si aciertas, duplicas lo apostado."""

def solicitar_dinero_apostar(saldo):
    dinero_apostado = 0
    while dinero_apostado < APUESTA_MINIMA or dinero_apostado > saldo:
        dinero_apostado = float(input(f"¿Cuánto apuestas? Debe ser al menos {APUESTA_MINIMA} y no debe superar el saldo ({saldo}): "))
    return dinero_apostado

def solicitar_numero():
    numero = 0
    while numero < 1 or numero > 36:
        numero = int(input("Elige un número entre 1 y 36: "))
    return numero

def solicitar_color():
    color = ""
    while color not in ["rojo", "negro"]:
        color = input("Elige un color (rojo o negro): ").lower()
    return color

def jugar_ruleta(saldo):
    total_apostado = 0
    total_ganado = 0
    total_juegos = 0
    total_ganados = 0

    while True:
        print(f"Saldo disponible: {saldo}")
        tipo_apuesta = input("¿Qué tipo de apuesta deseas realizar? (1. Número, 2. Sección, 3. Color, 4. Volver): ")

        if tipo_apuesta == "4":
            break

        if tipo_apuesta == "1":
            apuesta = solicitar_dinero_apostar(saldo)
            numero_usuario = solicitar_numero()
            total_apostado += apuesta
            total_juegos += 1

            numero_aleatorio = random.randint(1, 36)
            print(f"Número obtenido: {numero_aleatorio}")

            if numero_usuario == numero_aleatorio:
                ganancia = apuesta * MULTIPLICADOR_NUMERO
                saldo += ganancia
                total_ganado += ganancia
                total_ganados += 1
                print(f"¡Ganaste! Tu ganancia es: {ganancia}")
            else:
                saldo -= apuesta
                print("Perdiste la apuesta.")

        elif tipo_apuesta == "2":
            apuesta = solicitar_dinero_apostar(saldo)
            seccion_usuario = input("Elige una sección (1-12, 13-24, 25-36): ")
            total_apostado += apuesta
            total_juegos += 1

            numero_aleatorio = random.randint(1, 36)
            print(f"Número obtenido: {numero_aleatorio}")

            if numero_aleatorio in secciones[seccion_usuario]:
                ganancia = apuesta * MULTIPLICADOR_SECCION
                saldo += ganancia
                total_ganado += ganancia
                total_ganados += 1
                print(f"¡Ganaste! Tu ganancia es: {ganancia}")
            else:
                saldo -= apuesta
                print("Perdiste la apuesta.")

        elif tipo_apuesta == "3":
            apuesta = solicitar_dinero_apostar(saldo)
            color_usuario = solicitar_color()
            total_apostado += apuesta
            total_juegos += 1

            numero_aleatorio = random.randint(1, 36)
            color_aleatorio = "rojo" if numero_aleatorio % 2 == 0 else "negro"
            print(f"Número obtenido: {numero_aleatorio}, Color obtenido: {color_aleatorio}")

            if color_usuario == color_aleatorio:
                ganancia = apuesta * MULTIPLICADOR_COLOR
                saldo += ganancia
                total_ganado += ganancia
                total_ganados += 1
                print(f"¡Ganaste! Tu ganancia es: {ganancia}")
            else:
                saldo -= apuesta
                print("Perdiste la apuesta.")

        else:
            print("Opción no válida. Intenta de nuevo.")

     
        print(f"Saldo actual: {saldo}")
        print(f"Total apostado: {total_apostado}")
        print(f"Total ganado: {total_ganado}")
        if total_juegos > 0:
            promedio_exito = total_ganados / total_juegos
            print(f"Promedio de éxito: {promedio_exito:.2f}")

    return saldo

def main():
    saldo_global = SALDO_INICIAL
    eleccion = ""
    while eleccion != "3":
        eleccion = input(menu_juegos)
        if eleccion == "1":
            print(sintesis_casino)
            print(f"Dinero disponible: {saldo_global}")
          
        elif eleccion == "2":
            saldo_global = jugar_ruleta(saldo_global)
        elif eleccion == "3":
            print("Saliendo del juego...")
        else:
            print("Opción no válida. Intenta de nuevo.")

    print(f"Saldo final: {saldo_global}")

if __name__ == "__main__":
    main()
    
    saldo_global = 50000
    eleccion = ""
    while eleccion != "3":
        eleccion = input(menu_juegos)
        if eleccion == "1":
            if saldo_global < APUESTA_MINIMA_CASINO:
                print(
                    f"Necesita contar con un saldo de al menos {APUESTA_MINIMA_CASINO} para jugar al casino")
                continue
            print(sintesis_casino)
            print(f"Dinero disponible: {saldo_global}")

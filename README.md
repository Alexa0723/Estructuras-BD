import random
import time
import bisect
import sys
sys.setrecursionlimit(11000)

n = 10000

# generar IDs
ids_ordenados = list(range(n))
ids_aleatorios = ids_ordenados.copy()
random.shuffle(ids_aleatorios)

#Lista
def prueba_lista(lista):
    inicio = time.time()
    for _ in range(100):
        x = random.randint(0, n-1)
        x in lista
    fin = time.time()
    return fin - inicio

# ABB (árbol binario)
class Nodo:
    def __init__(self, valor):
        self.valor = valor
        self.izq = None
        self.der = None

def insertar(raiz, valor):
    if raiz is None:
        return Nodo(valor)
    if valor < raiz.valor:
        raiz.izq = insertar(raiz.izq, valor)
    else:
        raiz.der = insertar(raiz.der, valor)
    return raiz

def buscar(raiz, valor):
    if raiz is None or raiz.valor == valor:
        return raiz
    if valor < raiz.valor:
        return buscar(raiz.izq, valor)
    else:
        return buscar(raiz.der, valor)

def prueba_abb(datos):
    raiz = None
    for x in datos:
        raiz = insertar(raiz, x)

    inicio = time.time()
    for _ in range(100):
        x = random.randint(0, n-1)
        buscar(raiz, x)
    fin = time.time()
    return fin - inicio


#B+
def prueba_bplus(lista):
    inicio = time.time()
    for _ in range(100):
        x = random.randint(0, n-1)
        i = bisect.bisect_left(lista, x)
        if i < len(lista) and lista[i] == x:
            pass
    fin = time.time()
    return fin - inicio

print("ORDENADOS")
print("Lista:", prueba_lista(ids_ordenados))
print("ABB:", prueba_abb(ids_ordenados))  # peor caso
print("B+ :", prueba_bplus(ids_ordenados))

print("\nALEATORIOS")
print("Lista:", prueba_lista(ids_aleatorios))
print("ABB:", prueba_abb(ids_aleatorios))  # mejor caso
print("B+ :", prueba_bplus(ids_ordenados))

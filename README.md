# Quiz 1 Lenguajes
# Ejercicio 1
 Diseñe un sistema que permita a los usuarios navegar por las páginas web, incluida la capacidad de volver a la página anterior y avanzar a la página siguiente, de forma similar a como funcionan los navegadores web modernos.
# Diagrama de Flujo 
<img width="2651" height="3840" alt="Untitled diagram _ Mermaid Chart-2025-08-04-204255" src="https://github.com/user-attachments/assets/491b2868-e22c-495a-8fed-1faba3f7bba8" />

# Codigo 
  ```
Python
    class Navegador:
    def __init__(self):
        self.atras = []
        self.adelante = []
        self.actual = None

    def abrir(self, url):
        if self.actual:
            self.atras.append(self.actual)
        self.actual = url
        self.adelante.clear()
        print(f"Página actual: {self.actual}")

    def ir_atras(self):
        if not self.atras:
            print("No hay página anterior.")
            return
        self.adelante.append(self.actual)
        self.actual = self.atras.pop()
        print(f"Página actual: {self.actual}")

    def ir_adelante(self):
        if not self.adelante:
            print("No hay página siguiente.")
            return
        self.atras.append(self.actual)
        self.actual = self.adelante.pop()
        print(f"Página actual: {self.actual}")

    def mostrar_pagina(self):
        print(f"Estás en: {self.actual if self.actual else 'ninguna página'}")
#Ejemplo de Uso
nav = Navegador()
nav.abrir("google.com")
nav.abrir("spotify.com")
nav.abrir("github.com")

nav.ir_atras()      # openai.com
nav.ir_atras()      # google.com
nav.ir_adelante()   # spotify.com
nav.abrir("spotify.com")  # se borra pila adelante
nav.ir_adelante()   #  No hay página siguiente

```
# Punto 2
Desarrollar un sistema para una entidad pública que atienda a sus clientes por orden de llegada.
# Diagrama de Flujo
<img width="516" height="679" alt="image" src="https://github.com/user-attachments/assets/b03104a8-11ca-44db-84cb-ad85c44e33fa" />


# Codigo
  ```
Python
      from collections import deque
       import time
        class AtencionCiudadana:
    def __init__(self):
        self.cola = deque()

    def add_customer(self, cliente):
        """Agregar cliente a la cola"""
        self.cola.append(cliente)
        print(f"[+] Cliente '{cliente}' agregado a la cola.")

    def serve_customer(self):
        """Atender siguiente cliente"""
        if self.cola:
            cliente = self.cola.popleft()
            print(f"[✓] Atendiendo al cliente: {cliente}")
        else:
            print("[!] No hay clientes para atender.")

    def hay_clientes(self):
        return len(self.cola) > 0

    def esperar_nuevos_clientes(self):
        print("Esperando llegada de nuevos clientes...\n")
        time.sleep(1)

    def iniciar_servicio(self, modo_manual=True):
        print("\n=== INICIO DE ATENCIÓN AL CLIENTE ===\n")
        while True:
            if modo_manual:
                llegada = input("¿Llega un nuevo cliente? (s/n): ").strip().lower()
                if llegada == 's':
                    nombre = input("Nombre del cliente: ").strip()
                    self.add_customer(nombre)
            else:
                # Simulación automática de llegada cada 2 iteraciones
                self.esperar_nuevos_clientes()

            if self.hay_clientes():
                self.serve_customer()
            else:
                self.esperar_nuevos_clientes()

            if modo_manual:
                fin = input("¿Desea finalizar la atención? (s/n): ").strip().lower()
                if fin == 's':
                    print("\n=== FIN DE LA ATENCIÓN ===")
                    break
            else:
                # Fin automático tras atender todos
                if not self.hay_clientes():
                    print("\n=== FIN DE LA ATENCIÓN ===")
                    break

 ```
# PUNTO 3 

Cree una función de autocompletar que, dado un prefijo, devuelva todas las palabras posibles que comiencen con ese prefijo, similar a un motor de búsqueda.3

# DIAGRAMA DE FLUJO

<img width="2306" height="3840" alt="Untitled diagram _ Mermaid Chart-2025-08-04-205716" src="https://github.com/user-attachments/assets/b2d71854-cebb-4328-8ef7-2d3d7ece803c" />

# CODIGO

```
Python
   class TrieNode:
    def __init__(self):
        self.children = {}     # Diccionario de letras hijas
        self.is_end_of_word = False  # Marca el final de una palabra


class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        """Inserta una palabra en el trie"""
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True

    def _dfs(self, node, prefix, results):
        """Búsqueda en profundidad desde el nodo actual"""
        if node.is_end_of_word:
            results.append(prefix)
        for char, child_node in node.children.items():
            self._dfs(child_node, prefix + char, results)

    def autocomplete(self, prefix):
        """Devuelve todas las palabras que comienzan con el prefijo dado"""
        node = self.root
        for char in prefix:
            if char not in node.children:
                return []  # Prefijo no encontrado
            node = node.children[char]
        
        results = []
        self._dfs(node, prefix, results)
        return results


# === EJEMPLO DE USO ===
if __name__ == "__main__":
    trie = Trie()
    palabras = ["auto", "automóvil", "autobús", "autor", "autopsia", "avión", "aviso", "animal", "anillo"]
    
    for palabra in palabras:
        trie.insert(palabra)

    while True:
        prefijo = input("Escribe un prefijo para autocompletar (o 'salir'): ").strip().lower()
        if prefijo == "salir":
            break
        sugerencias = trie.autocomplete(prefijo)
        if sugerencias:
            print("Sugerencias:", sugerencias)
        else:
            print("No se encontraron coincidencias.")

```

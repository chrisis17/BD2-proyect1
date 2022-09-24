# BD2-proyect1

## Integrantes:
      -Canto Vidal, Harold Alexis
      -Rojas Rojas, Christian Kevin
      -Zapata Gallegos, Neo Marcelo

## Objetivo:
-El objetivo principal del proyecto es analizar y experimentar sobre los accesos a memoria secundaria que hacen en el extendible hash y en el sequential file.

**Metodos**
```cpp
        vector<Registro> search(T key)
```

```cpp
      vector<Registro> rangeSearch(T begin-key, T end-key) 
```

```cpp
      bool remove(T key) 
```

```cpp
      bool add(Registro registro)
```

            
**Insertion**   
-Para la insertion hacemos empleo de la función  **`insideSearch()`** que nos permite usar nuestro **`binarySearch()`** lo que hace que hagamos O (log(n)) accesos a memoria secundaria. Luego, localizamos la posición donde será insertado el nuevo registro, si es que el espacio está libre, entonces insertamos, si no insertarmos el resgitro en el  **`aux.dat`** que es nuestro espacio auxiliar. Seguido de esto actualizamos los punteros.

 ```cpp
    void insert(TRecord &record, int &accesos)
```

**Busqueda**
-Para este caso usamos el algoritmo **`binarySearch()`** este se usar para ubicar un registro en el archivo dado un valor busqueda, esto los hacemos en O(log(n))accesos a memoria secundaria. Además durante la busqueda si es que un archivo fue marcado como eliminado antes se descarta en la busqueda.

 ```cpp
   TRecord *search(TKey key, int &accesos)
```

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
        vector<Registro> rangeSearch(T begin-key, T end-key) 
        bool remove(T key) 
        bool add(Registro registro)
```
            
**Insertion**   
-Para la insertion hacemos empleo de la función  **`insideSearch()`** que nos permite usar nuestro **`binarySearch()`** lo que hace que hagamos N log(n) accesos a memoria secundaria. Luego, localizamos la posición donde será insertado el nuevo registro, si es que el espacio está libre, entonces insertamos, si no insertarmos el resgitro en el  **`aux.dat`** que es nuestro espacio auxiliar. Seguido de esto actualizamos los punteros.

 ```cpp
    void insert(TRecord &record, int &accesos)
```

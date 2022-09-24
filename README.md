# BD2-proyect1

<p align="center">
<img width="460" height="400" src=
https://upload.wikimedia.org/wikipedia/commons/thumb/7/7a/UTEC.jpg/600px-UTEC.jpg>
</p>

## Integrantes:
      -Canto Vidal, Harold Alexis
      -Rojas Rojas, Christian Kevin
      -Zapata Gallegos, Neo Marcelo

## Objetivo:
>El objetivo principal del proyecto es analizar, experimentar e implementar el extendible hash y el sequential file.
(El link del video se encuentra al final)
## Métodos
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

## Sequential File
**Estructura**
```cpp
    template <typename TRecord, typename TKey>
    class Sequential
      {
      private:
    string fileName;
    string auxName;
    long sizeData = 0;
    long sizeAux = 0;

    const long sizeRecord = sizeof(TRecord);
```        
**Inserción**
>Para la insertion hacemos empleo de la función  **`insideSearch()`** que nos permite usar nuestro **`binarySearch()`** lo que hace que hagamos O (log(n)) accesos a memoria secundaria. Luego, localizamos la posición donde será insertado el nuevo registro, si es que el espacio está libre, entonces insertamos, si no insertarmos el resgitro en el  **`aux.dat`** que es nuestro espacio auxiliar. Seguido de esto actualizamos los punteros.

 ```cpp
    void insert(TRecord &record, int &accesos)
```

**Búsqueda**
>Para este caso usamos el algoritmo **`binarySearch()`** este se usar para ubicar un registro en el archivo dado un valor busqueda, esto los hacemos en O(log(n))accesos a memoria secundaria. Además durante la busqueda si es que un archivo fue marcado como eliminado antes se descarta en la busqueda.

 ```cpp
   TRecord *search(TKey key, int &accesos)
```
**Búqueda por rangos**
>Para la busqueda por rangos lo que hacemos es buscar ubicar donde está el begin y el end dentro de los archivos **`aux.dat`** y **`dat.dat`** y retornar los valores que estan dentro del invervalo sugerido verificando donde se ubican los elementos de manera ordenada por los punteros, ya que podria haber data que no esté dentro del mismo **`dat.dat`** si no que haya sido movido al **`aux.dat`** con su respectiva referencia.

 ```cpp
   vector<TRecord> search(TKey begin, TKey end)
```
**Remover**
>Para remover usamos las funcion **`erase()`** que permite ubicar el registro por key dentro de los archivos **`aux.dat`** y **`dat.dat`** se remueve y para mantener ordenada la data se hace de manera fisica un **`refactor()`** para mantener ordenada la data y mantener la referencia de los punteros luego de la remocion del registro. Además está función nos permite verificar si es posible remover o no en caso este vacio o de no existir el registro pedido por el usuario.
```cpp
   bool erase(TKey key)
```
## Extendible hash
**Estructura**

```cpp
template <typename TKey>
class ExtendibleHash
{
private:
    string fileName;
    string indexName;
    unordered_map<string, long> index;
    unordered_map<string, long> index_size;
    hash<TKey> _hash;
    long freeListBucket = -1;
    string hash_function(TKey key)
    {
        auto hash_code = _hash(key);
        string hash_code_string = bitset<HASH_HEIGHT>(hash_code).to_string();
        return hash_code_string;
    }
    void initIndex();
    void readIndex();
    void writeIndex();
    void insertInFreeList(fstream &f, int &accesos);
    void divideBucket(fstream &f, Bucket<TKey> &bucket, long pos, int &accesos);
    bool exists(TKey key, int &accesos);
    bool is_brother(string ref, string bro, long d);
```

**Inserción**
>Para la inserción aplicamos la función **`hash_function(key)`** sobre el key, que nos retorna una posición en el archivo Index.dat, Index.dat retorna la posición física donde insertaremos el registro en el archivo Data.dat. Luego se accede a esa posición y existen 2 posibilidades, la primera es que no exista un Bucket (conjunto de registros), en ese caso creamos uno e insertamos el registro en el nuevo bucket (tomando 2 accesos extra), el segundo caso es que ya exista un bucket, entonces revisamos si hay espacio en el bucket para insertar, caso contrario creamos un nuevo bucket, lo enlazamos e insertamos el registro en el nuevo bucket. (De esta forma la estructura crece dinámicamente, porque creamos nuevos buckets según sea necesario.

```cpp
    void insert(RecordHash<TKey> record, int &accesos)
```

**Búsqueda**
>Para realizar el search, se utiliza la función **`hash_function(key)`** sobre la llave, de esta forma se obtiene la posición del registro en el archivo Data.dat, accedemos a esa posición en el archivo de datos. Si no encontramos un bucket (pos = -1) entonces no existe el registro. Caso contrario, se carga el bucket a memoria principal y se recorre de forma sequencial junto con todos los buckets enlazados hasta encontrar el registro en cuestión o llegar al final de la lista de buckets enlazados.

```cpp
    RecordHash<TKey> *search(TKey key, int &accesos)
```

**Remover**
>Para realiza el erase, se realiza una búsqueda y se elimina el registro del bucket dejando un espacio vacío en el bucket, y se presentan los siguientes casos. Si es el último bucket (de la lista enlazada de buckets) no hay problema. Caso contrario, tenemos que mover el último registro del último bucket en la lista enlazada a la posición que hemos dejado vacía. De esta forma, solo quedaría revisar el último bucket de la lista, si está vacío se elimina, caso contrario se deja como esta. De esta forma, se cumple la naturaleza del Dynamic hashing, de crecer y reducir su tamaño conforme se inserten o eliminen registros.

```cpp
    bool erase(TKey key)
```

**Gráficas**
>Inserción

| Cuadro comparativo | | | | | |
|:-------------------:|---|---|---|---|---|
| Registros| 500| 1000| 1500| 2000|2500|
| Hash | 1578| 3490| 5732| 7871| 8148|
| Sequential | 2496| 20252| 30988| 28135|34365|

>Búsqueda

| Cuadro comparativo | | | | | |
|:-------------------:|---|---|---|---|---|
| Registros |500 | 1000| 1500| 2000| 2500|
| Hash | 1| 1| 1| 1| 1|
| Sequential |11 |11 |11 | 11| 11|

**Video**
link: https://drive.google.com/file/d/1cLoQ44ux-m1oPGeKlAhTJ4XPEvKTtOMV/view?usp=sharing


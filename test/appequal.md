[appequal]()
# Присваивание

Синтаксис выражения:

1. Когда одному объекту присваивается один идентификатор, то интерпретатор возвращает ссылку на объект.
\\*`x = ?R # -> "R"`

1. Когда одному объекту присваивается несколько идентификаторов, то интерпретатор сначала вызывает для объекта метод `.to_ary`, а затем  идентификаторы присваиваются элементам возвращаемого индексного массива. В результате выполнения выражения возвращается ссылка на индексный массив.
```
  x, y  =  [ 1, 2 ] # -> [ 1, 2 ]
  x # -> 1
  y # -> 2
```

1. Когда нескольким объектам присваивается один идентификатор, то интерпретатор присваивает идентификатор массиву, содержащему ссылки на все переданные объекты. В результате возвращается ссылка на индексный массив.
```
  x  =  ?Y, ?N # -> [ "Y", "N" ]
  x # -> "Y"
```

1. Когда нескольким объектам присваивается такое же количество идентификаторов, то интерпретатор выполняет параллельное присваивание. В результате возвращается ссылка на индексный массив, содержащий все использованные объекты.
```
  x, y  =  ?Y, ?N # -> [ "Y", "N" ];
  x # -> "Y"
  y# -> "N"

  x, y  =  y, x # -> [ "N", "Y" ]
  x # -> "N"
  y # -> "Y"
```

1. Когда после идентификатора записана запятая, то интерпретатор считает, что после нее указан еще один идентификатор. Этот идентификатор участвует в присваивании, даже если не был задан.
```
  x,  =  ?Y, ?N # -> [ "Y", "N" ]
  x# -> "Y"
```
  
1. Когда объектов меньше, чем идентификаторов, то интерпретатор выполняет параллельное присваивание, объявляя лишние идентификаторы (но не инициализируя их). В результате возвращается ссылка на индексный массив, содержащий все использованные объекты.
```
  x, y, z  =  ?Y, ?N  # -> [ "Y", "N" ]
  x # -> "Y"
  y # -> "N"
  z # -> nil
```

1. Когда объектов больше, чем идентификаторов, то интерпретатор выполняет параллельное присваивание. Лишним объектам идентификаторы не присваиваются. В результате возвращается ссылка на индексный массив, содержащий все использованные объекты.
```
  x, y  =  ?Y, ?N, ?Q # -> [ "Y", "N", "Q" ]
  x # -> "Y"
  y # -> "N"
```

1. Когда перед объектом в качестве приставки использован символ звездочки (*), то объект обрабатывается как составной. Интерпретатор извлекает для присваивания все элементы объекта. В результате возвращается ссылка на индексный массив, содержащий все использованные объекты. Таким образом интерпретатор может извлекать элементы любого составного объекта, для которого существует метод `.to_splat`.

   Запись двух символов звездочки подряд приводит к вызову ошибки. При извлечении элементов вложенных массивов символ звездочки необходимо записывать перед каждым вложенным массивом.
```
  x, y  =  *(1..3) # -> [ 1, 2, 3 ]
  x # -> 1
  y # -> 2
```

1. Когда перед идентификатором в качестве приставки использован символ звездочки (*), то интерпретатор присвоит этот идентификатор массиву, содержащему все лишние объекты. В результате возвращается ссылка на индексный массив, содержащий все использованные объекты.
```
  *x, y  =  ?Y, ?N # -> [ "Y", "N" ]
  x # -> [ "Y" ]
  y # -> "N"

  *x, y  =  ?Y, ?N, ?Q # -> [ "Y", "N", "Q" ]
  x # -> [ "Y", "N" ]
  y # ->"Q"

  x, *y, z  =  ?Y, ?N, ?Q, ?R # -> [ "Y", "N", "Q", "R" ]
  x # ->"Y"
  y # ->[ "N", "Q" ]
  z # ->"R"
```

1. Когда группа идентификаторов ограничена круглыми скобками, то одновременно выполняется несколько выражений присваивания:

   + В первом выражении группа идентификаторов обрабатывается как одна логическая единица;
   + Во втором выражении интерпретатор извлекает идентификаторы из группы.

 В результате возвращается ссылка на индексный массив, содержащий все использованные объекты.
```
  x, (y, z)  =  ?Y, ?N # -> x = ?Y
               (y, z) = ?N # -> y, z = ?N -> y = ?N
                  z = nil # -> [ "Y", "N" ]
```                  
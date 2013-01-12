# Типы данных

## Логические величины

Классы TrueClass, FalseClass и NilClass - это собственные классы объектов true, false и nil соответственно. Также существуют константы TRUE, FALSE, NIL, которые могут быть переопределены (но зачем?).

```longtable{ | * {3} { l |}}\hline
NilClass & FalseClass & TrueClass \\ \hline
nil \& obj -> false & false \& obj -> false & true \& bool -> bool \\ \hline
nil \textasciicircum\-, bool -> bool & false \textasciicircum\-, bool -> bool & true \textasciicircum\-, bool -> !bool \\ \hline
nil | bool -> bool & false | bool -> bool & true | object -> true \\ \hline
nil.to_s -> "" & false.to_s -> "false" & true.to_s -> "true" \\ \hline
```

`nil.nil? # -> true`

`nil.inspect # -> "nil"`

`nil.rationalize # -> (0/1)`
\alias{to_r}

`nil.to_a # -> []`

`nil.to_c # -> (0+0i)`

`nil.to_f # -> 0.0`

`nil.to_i # -> 0`

## Symbol

Добавленные модули: Comparable

Большинство методов сначала преобразует объект в текст.

`::all_symbols # -> array`
Для получения массива всех существующих экземпляров класса.

### Приведение типов

`.to_s # -> string`
\alias{id2name}
Для получения текстового значения.
`:Ruby.to_s # -> "Ruby"`

`.inspect # -> string`
Для получения информации об объекте.
`:Ruby.inspect # -> ":Ruby"`

`.to_sym # -> symbol`
\alias{intern}

`.to_proc # -> proc`
Для получения подпрограммы, выполняющей то же, что и метод с аналогичным идентификатором.
```
  1.next # -> 2
  :next.to_proc.call(1) # -> 2
```

### Операторы

`symbol <=> object`
Выполняемое выражение: `symbol.to_s <=> object`.

`symbol =~ object`
Выполняемое выражение: `symbol.to_s =~ object`.

`symbol[*object]`
\alias{slice}
Выполняемое выражение: `symbol.to_s[*object]` или `symbol.to_s.slice(*object)`.

### Изменение регистра

`.capitalize # -> symbol`
Выполняемое выражение: `self.to_s.capitalize.to_sym`.

`.swapcase # -> symbol`
Выполняемое выражение: `self.to_s.swapcase.to_sym`.

`.upcase # -> symbol`
Выполняемое выражение: `self.to_s.upcase.to_sym`.

`.downcase # -> symbol`
Выполняемое выражение: `self.to_s.downcase.to_sym`.

### Остальное

`.encoding # -> encoding`
Для получения кодировки текстового значения.
`:Ruby.encoding # -> #<Encoding:US-ASCII>`

`.empty?`
Выполняемое выражение: `self.to_s.empty?`.

`.length # -> integer`
\alias{size}
Выполняемое выражение: `self.to_s.length` или `self.to_s.size`.

`.casecmp(object)`
Выполняемое выражение: `self.to_s.casecmp(object)`.

`.next # -> symbol`
\alias{succ}
Выполняемое выражение: `self.to_s.next.to_sym` или `self.to_s.succ.to_sym`

## Структуры

Добавленные модули: Enumerable

Структура данных - это объект, имеющий только свойства. Иногда структуры также используют при необходимости передавать множество аргументов.

Для облегчения создания структур в Ruby предоставлен класс Struct.

`::new( name = nil, *attribute ) # -> class`
Для создания в теле Struct нового класса структур и объявления переданных свойств (ссылающихся на nil). Если название класса не передано, то создается анонимный класс. Анонимный класс получит собственный идентификатор, если ему будет присвоена константа.

Для нового класса определяется конструктор, принимающий объекты, инициализирующие свойства.
```
  Struct.new "Kлюч", :объект # -> Struct::Kлюч 
  # только для примера. Никогда так больше не делайте :) 
  Struct::Kлюч.new [ 1, 2, 3 ] 
  # ->  #<struct Struct::Kлюч объект=[1, 2, 3]> 
```

### Приведение типов

`.to_s # -> string`
\alias{inspect} 
Для получения информации об объекте.
```
  Struct::Kлюч.new( [ 1, 2, 3 ] ).to_s 
  # -> "#<struct Struct::Kлюч объект=[1, 2, 3]>"
```

`.to_a # -> array`
\alias{values}
Для получения массива значений свойств.
`Struct::Kлюч.new( [ 1, 2, 3 ] ).to_a # -> [ [ 1, 2, 3 ] ]`

### Элементы структур

Элементами структур считаются свойства. Для доступа к свойствам определены операторы `[]` и `[]=`. Они принимают идентификаторы или индексы свойств. Индексация свойств начинается с 0 и соответствует порядку, использовавшемуся при создании структуры. Индекс может быть отрицательным (индексация, начинается с -1).

Использование несуществующего свойства считается ошибкой.

`.[attr] # -> object`
Для получения значения свойства.
```
  Struct::Kлюч.new( [ 1, 2, 3 ] )[ "объект" ] # -> [ 1, 2, 3 ]
  Struct::Kлюч.new( [ 1, 2, 3 ] )[ 0 ] # -> [ 1, 2, 3 ] 
  Struct::Kлюч.new( [ 1, 2, 3 ] )[ -1 ] # -> [ 1, 2, 3 ]
```

`.[attr]=(object) # -> object`
Для изменения свойства.
```
  Struct::Kлюч.new( [ 1, 2, 3 ] )[ "объект" ] = :array # -> :array
  Struct::Kлюч.new( [ 1, 2, 3 ] )[ 0 ] = :array # -> :array 
  Struct::Kлюч.new( [ 1, 2, 3 ] )[ -1 ] = :array # -> :array
```

`.values_at(*integer) # -> array`
Для получения массива значений свойств с переданными индексами. При вызове без аргументов возвращается пустой массив.
```
  Struct::Kлюч.new( [ 1, 2, 3 ] ).values_at 0, -1 
  # -> [ [ 1, 2, 3 ], [ 1, 2, 3 ] ]
```

### Итераторы

`.each { |object| } # -> self`
Перебор значений свойств.

`.each_pair { | name, object | } # -> self`
Перебор идентификаторов свойств и их значений.

### Остальное

`.hash # -> integer`
Для получения цифрового кода объекта.
`Struct::Kлюч.new( [ 1, 2, 3 ] ).hash # -> -764829164`

`.size # -> integer`
\alias{length}
Для получения количества свойств.
`Struct::Kлюч.new( [ 1, 2, 3 ] ).size # -> 1`

`.members # -> array`
Для получения массива идентификаторов свойств.
`Struct::Kлюч.new( [ 1, 2, 3 ] ).members # -> [ :объект ]`
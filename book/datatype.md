# Типы данных

## Логические величины

Классы TrueClass, FalseClass и NilClass - это собственные классы объектов true, false и nil соответственно. Также существуют константы TRUE, FALSE, NIL, которые могут быть переопределены (но зачем?).

~~~~~ longtable
{ | * {3} { l |}}
NilClass            & FalseClass              & TrueClass
nil \& obj -> false & false \& obj -> false   & true \& bool  -> bool
nil ~ bool -> bool  & false ~ bool -> bool    & true ~ bool   -> !bool
nil | bool -> bool  & false | bool -> bool    & true | object -> true
nil.to_s -> ""      & false.to_s   -> "false" & true.to_s     -> "true"
~~~~~

~~~~~ ruby
  nil.nil? # -> true
  nil.inspect # -> "nil"
  nil.rationalize # -> (0/1)
  nil.to_r # -> (0/1)
  nil.to_a # -> []
  nil.to_c # -> (0+0i)
  nil.to_f # -> 0.0
  nil.to_i # -> 0
  nil.to_h # -> {} [Ruby 2.0]
~~~~~

## Symbol

**Добавленные модули: Comparable**

Большинство методов сначала преобразует объект в текст.

`::all_symbols # -> array` Массив всех существующих экземпляров класса.

### Приведение типов

`.to_sym # -> symbol` Синонимы: `intern`

`.to_s # -> string`  
Синонимы: `id2name`

Используется для получения текстовое значения.  
`:Ruby.to_s # -> "Ruby"`

`.inspect # -> string`

Информация об объекте.  
`:Ruby.inspect # -> ":Ruby"`

`.to_proc # -> proc`

Используется для получения подпрограммы, выполняющей то же, что и метод с аналогичным идентификатором.

~~~~~ ruby
  1.next # -> 2
  :next.to_proc.call(1) # -> 2
~~~~~

### Операторы

~~~~~ ruby
symbol <=> object # -> symbol.to_s <=> object
symbol =~ object # -> symbol.to_s =~ object
symbol[*object] # -> symbol.to_s[*object]
symbol.slice(*object) # -> symbol.to_s.slice(*object)
~~~~~

### Изменение регистра

`.capitalize # -> symbol` Выполняемое выражение: `self.to_s.capitalize.to_sym`.

`.swapcase # -> symbol` Выполняемое выражение: `self.to_s.swapcase.to_sym`.

`.upcase # -> symbol` Выполняемое выражение: `self.to_s.upcase.to_sym`.

`.downcase # -> symbol` Выполняемое выражение: `self.to_s.downcase.to_sym`.

### Остальное

`.encoding # -> encoding`

Кодировка.  
`:Ruby.encoding # -> #<Encoding:US-ASCII>`

`.empty? # -> bool` Выполняемое выражение: `self.to_s.empty?`.

`.length # -> integer`  
Синонимы: `size`

Выполняемое выражение: `self.to_s.length` или `self.to_s.size`.

`.casecmp(object)` Выполняемое выражение: `self.to_s.casecmp(object)`.

`.next # -> symbol`  
Синонимы: `succ`

Выполняемое выражение: `self.to_s.next.to_sym` или `self.to_s.succ.to_sym`

## Структуры

**Добавленные модули: Enumerable**

Структура данных - это объект, имеющий только свойства. Иногда структуры также используют при необходимости передавать несколько аргументов.

Для создания структур используется класс Struct.

`::new( name = nil, *attribute ) # -> class`

Используется для создания в теле Struct нового класса структур и объявления переданных свойств (ссылающихся на nil). Если название класса не передано, то создается анонимный класс. Анонимный класс получит идентификатор, только если  будет присвоен константе.

Для нового класса определяется конструктор, принимающий значения для объявленных свойств.

~~~~~ ruby
  Struct.new "Kлюч", :объект # -> Struct::Kлюч
  # только для примера. Никогда так больше не делайте :)
  Struct::Kлюч.new [1, 2, 3]
  # ->  #<struct Struct::Kлюч объект=[1, 2, 3]>
~~~~~

### Приведение типов

`.to_s # -> string`  
Синонимы: `inspect`

Информация об объекте.

~~~~~ ruby
  Struct::Kлюч.new( [1, 2, 3] ).to_s
  # -> "#<struct Struct::Kлюч объект=[1, 2, 3]>"
~~~~~

`.to_a # -> array`  
Синонимы: `values`

Массив значений свойств.  
`Struct::Kлюч.new( [1, 2, 3] ).to_a # -> [ [1, 2, 3] ]`

`.to_h # -> hash [Ruby 2.0]`

Массив свойств и их значений.

~~~~~ ruby
  Customer = Struct.new :name, :address, :zip
  joe = Customer.new "Joe Smith", "123 Maple, Anytown NC", 12345
  joe.to_h[:address] # -> "123 Maple, Anytown NC"
~~~~~

### Элементы

Элементами структур считаются свойства. Для доступа к свойствам определены операторы `[]` и `[]=`. Они принимают идентификаторы или индексы свойств. Индексация свойств начинается с 0 и соответствует порядку, использовавшемуся при создании структуры. Индекс может быть отрицательным (индексация, начинается с -1).

Использование несуществующего свойства считается исключением.

`.[attr] # -> object`

Используется для получения значения свойства.

~~~~~ ruby
  Struct::Kлюч.new( [1, 2, 3] )["объект"] # -> [1, 2, 3]
  Struct::Kлюч.new( [1, 2, 3] )[0] # -> [1, 2, 3]
  Struct::Kлюч.new( [1, 2, 3] )[-1] # -> [1, 2, 3]
~~~~~

`.[attr]=(object) # -> object`

Используется для изменения свойства.

~~~~~ ruby
  Struct::Kлюч.new( [1, 2, 3] )["объект"] = :array # -> :array
  Struct::Kлюч.new( [1, 2, 3] )[0] = :array # -> :array
  Struct::Kлюч.new( [1, 2, 3] )[-1] = :array # -> :array
~~~~~

`.values_at(*integer) # -> array`

Используется для получения массива значений свойств с переданными индексами. При вызове без аргументов возвращается пустой массив.

~~~~~ ruby
  Struct::Kлюч.new( [1, 2, 3] ).values_at 0, -1
  # -> [ [1, 2, 3], [1, 2, 3] ]
~~~~~

### Итераторы

`.each { |object| } # -> self`

Перебор значений свойств.

`.each_pair { | name, object | } # -> self`

Перебор идентификаторов свойств вместе с значениями.

### Остальное

`.hash # -> integer`

Цифровой код объекта.  
`Struct::Kлюч.new( [1, 2, 3] ).hash # -> -764829164`

`.size # -> integer`  
Синонимы: `length`

Количество свойств.  
`Struct::Kлюч.new( [1, 2, 3] ).size # -> 1`

`.members # -> array`

Массив идентификаторов свойств.  
`Struct::Kлюч.new( [1, 2, 3] ).members # -> [:объект]`
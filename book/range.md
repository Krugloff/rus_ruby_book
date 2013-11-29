## Range (диапазоны)

Диапазоны необходимо ограничивать круглыми скобками. В противном случае метод будет вызван только для конечной границы.

`::new( first, last, include_last = false ) # -> range`

Используется для создания диапазона с переданными границами. Логическая величина влияет на обработку конечной границы.  
`Range.new 1, 5, 0 # -> 1...5`

### Приведение типов

`.inspect # -> string`

Преобразование диапазона в текст, с помощью вызова метода `.inspect` для каждой границы.  
`(1..3).inspect # -> "1..3"`

`.to_s # -> string`
Преобразование диапазон в текст.  
`(1..3).to_s # -> "1..3"`

### Элементы

`.begin # -> object`

Первый элемент диапазона.  
`(1..3).begin # -> 1`

`.end # -> object`

Последний элемент диапазона.  
`(1..3).end # -> 3`

`.last( size = nil ) # -> array`

Последний элемент или последний фрагмент.  
`(1..3).last 2 # -> [ 2, 3 ]`

### Операторы

`.===(object)`  
Синонимы: `cover?, member?, include?`

Проверяка входит ли объект в диапазон.
`(1..3) === 2 # -> true`

### Итераторы

`.each { |object| } # -> self` Перебор элементов с  помощью метода `.succ`.

`.step( step = 1 ) { |object| } # -> self`


Перебор элементов с заданным шагом, либо прибавляя его после каждой итерации, либо используя метод `.succ`.

### Остальное

`.exclude_end? # -> bool`

Проверка входит ли конечная граница в диапазон.  
`(1..3).exclude_end? # -> false`

`.bsearch { |x| } # -> elem [Ruby 2.0]`

Реализация двоичного поиска (метода деления пополам, дихотомии). Используется для нахождения элемента, который отвечает заданному условию за O(log n), где n - это размер диапазона. Алгоритм выполняет поиск элемента, используя дробление диапазона на половины.

Метод реализован как в классическом варианте, так и для использования бисекции.

+ Поиск элемента

  Блок должен возвращать логическую величину для каждого элемента. Диапазон должен содержать значение x, так что:

  + Блок возвращает false для любого элемента меньше чем x.
  + Блок возвращает true для любого элемента, больше или равного x.

  В результате возвращается x или nil.

  ~~~~~ ruby
    ary = [0, 4, 7, 10, 12]
    (0...ary.size).bsearch { |i| ary[i] >= 4 } # -> 1
    (0...ary.size).bsearch { |i| ary[i] >= 6 } # -> 2
    (0...ary.size).bsearch { |i| ary[i] >= 8 } # -> 3
    (0...ary.size).bsearch { |i| ary[i] >= 100 } # -> nil

    (0.0...Float::INFINITY).bsearch { |x| Math.log(x) >= 0 } # -> 1.0
  ~~~~~

+ Метод бисекции (метод деления отрезка пополам)

  Простейший численный метод для решения нелинейных уравнений вида `F(x) = 0`.

  Блок должен возвращать число для каждого элемента. Диапазон должен содержать элементы x и y (`x <= y`), так что:

  + Блок возвращает положительное число для элементов меньше чем x.
  + Блок возвращает ноль для элементов из диапазона `x...v`.
  + Блок возвращает отрицательное число для элементов больше иди равным y.

  В результате возвращается любой из элементов из диапазона `x...y`. Если нет элементов, отвечающих условию, возвращается nil.

  ~~~~~ ruby
    ary = [0, 100, 100, 100, 200]
    (0..4).bsearch { |i| 100 - ary[i] } # -> 1, 2 или 3
    (0..4).bsearch { |i| 300 - ary[i] } # -> nil
    (0..4).bsearch { |i|  50 - ary[i] } # -> nil
  ~~~~~

`.hash # -> integer`

Цифровой код объекта.
`(1..3).hash # -> -337569967`
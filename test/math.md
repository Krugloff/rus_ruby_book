## Math

Модуль содержит определение различных математических функций. Все методы могут быть вызваны либо как методы класса (для модуля Math), либо как частные методы экземпляров (для любого класса, включающего модуль Math).

Переданные аргументы, преобразуются в десятичные дроби. Поэтому в результате вызова метода также возвращается десятичная дробь.

##### Константы:

`Math::PI` - число π (пи);

`Math::E` - число e.

*****

`.acos(number) # -> float` Арккосинус.

`.acosh(number) # -> float` Гиперболический косинус.

`.asin(number) # -> float` Арксинус.

`.asinh(number) # -> float` Гиперболический синус.

`.atan(number) # -> float` Арктангенс.

`.atan2( first_number, second_number ) # -> float` Арктангенс.

`.atanh(number) # -> float` Гиперболический тангенс.

`.cbrt(number) # -> float` Кубический корень.

`.cos(number) # -> float` Косинус (в радианах).

`.cosh(number) # -> float` Гиперболический косинус (в радианах).

`.erf(number) # -> float` Функция ошибки.

`.erfc(number) # -> float` Дополнительная функция ошибки.

`.exp(number) # -> float` Возведение e в степень.

`.frexp(number) # -> array`

Индексный массив вида `[ float, integer]`,  
где `float * 2**integer == number`.

`.gamma(number) # -> float` Гамма функция из числа.

`.hypot( first_number, second_number ) # -> float` Длина гипотенузы.

`.ldexp( float, integer ) # -> float`

Аналогично выполнению `float * 2**integer`.

`.lgamma(number) # -> array`

Индексный массив вида  
`[ Math.log( Math.gamma(number).abs), Math.gamma(number) < 0 ? -1: 1 ]`.

`.log( number, base = Math::E ) # -> float` Логарифм

`.log10(number) # -> float` Десятичный логарифм.

`.log2(number) # -> float` Логарифм по основанию 2.

`.sin(number) # -> float` Синус (в радианах).

`.sinh(number) # -> float` Гиперболический синус (в радианах).

`.sqrt(number) # -> float` Квадратный корень.

`.tan(number) # -> float` Тангенс (в радианах).

`.tanh(number) # -> float` Гиперболический тангенс (в радианах).
## Кодировка

### Кодировка текста (Encoding)

В классе определены константы для каждой поддерживаемой кодировки. Вместо них также могут использоваться заранее определенные синонимы.  
`Encoding::UTF_8 # -> #<Encoding:UTF-8>`

#### Поддерживаемые кодировки

`::default_external # -> encoding`

Внешняя кодировку, используемая по умолчанию.


`::default_external=(encoding) # -> encoding`

Используется для изменения внешней кодировки, используемой по умолчанию.

`::default_internal # -> encoding`

Внутренняя кодировкf, используемая по умолчанию.


`::default_internal=(encoding) # -> encoding`

Используется для изменения внутренней кодировки, используемой по умолчанию. Для удаления кодировки передается nil.

`::locale_charmap # -> string`

Системная кодировка.

`::list # -> array`

Массив всех поддерживаемых кодировок.

`::name_list # -> array`

Массив всех синонимов.

`::aliases # -> hash`

Массив синонимов, ассоциируемых с экземплярами класса.

`::find(aliases) # -> encoding`

Используется для поиска кодировки, взаимосвязанной с переданным синонимом. Отсутствие синонима считается исключением (для внутренней кодировки может возвращаться nil).

###### Синонимы:

+ _"external"_ - внешняя кодировка;  
  _"internal"_ - внутренняя кодировка;
  _"locale"_ - локальная кодировка пользователя;
  _"filesystem"_ - кодировка файловой системы.

`::compatible?( first_string, second_string ) # -> encoding`

Используется для проверки совместимости кодировок в текстах. Когда они совместимы, возвращается кодировка, поддерживаемая обоими. В другом случае возвращается nil.  
`Encoding.compatible? "асции", "utf-8" # -> #<Encoding:UTF-8>`

#### Экземпляры

`.inspect # -> string`

Информация об объекте.  
`Encoding::UTF_8.inspect # -> "#<Encoding:UTF-8>"`

`.name # -> string`  
Синонимы: `to_s`

Информация о кодировке.  
`Encoding::UTF_8.name # -> "UTF-8"`

`.names # -> array`

Массив всех доступных синонимов.

~~~~~ ruby
  Encoding::UTF_8.names
  # -> ["UTF-8", "CP65001", "locale", "external", "filesystem"]
~~~~~

`.ascii_compatible? # -> bool`

Проверка совместимости с ASCII.  
`Encoding::UTF_8.ascii_compatible? # -> true`

`.dummy? # -> ruby`

Проверка относится ли кодировка к фиктивным. Для фиктивных кодировок обработка символов не реализована должным образом. Метод используется для динамичных кодировок.  
`Encoding::UTF_8.dummy? # -> false`

`.replicate(name) # -> encoding`

Используется для копирования кодировки. Использование уже существующего имени считается исключением.  
`Encoding::UTF_8.replicate "утф8" # -> #<Encoding:утф8>`

### Преобразование кодировок (Encoding::Converter)

##### Константы:

`::INVALID_MASK` - некоректные байты считаются исключением;

`::INVALID_REPLACE` - некорректные байты заменяются;

`::UNDEF_MASK` - неопределенные символы считаются исключением;

`::UNDEF_REPLACE` - неопределенные символы заменяются;

`::UNDEF_HEX_CHARREF` - неопределенные символы заменяются на байты `&xHH`;

`::UNIVERSAL_NEWLINE_DECORATOR` - замена CR (`\r`) и CRLF (`\r\n`) на LF (`\n`);

`::CRLF_NEWLINE_DECORATOR` - замена LF (`\n`) на CRLF (`\r\n`);

`::CR_NEWLINE_DECORATOR` - замена LF (`\n`) на CR (`\r`);

`::XML_TEXT_DECORATOR`

`::XML_ATTR_CONTENT_DECORATOR`

`::XML_ATTR_QUOTE_DECORATOR`

`::PARTIAL_INPUT` - обработка исходного текста как части другого объекта;

`::AFTER_OUTPUT` - цикличное преобразование исходного текста.

*****

`::new( source_enc, dest_enc, options = nil ) # -> converter`

`(conv_path) # -> converter`

Используется для создания преобразователя. Принимаются исходная кодировка и требуемая. Дополнительные опции описаны в [приложении](appencode).

Массив считается путем преобразования и должен содержать двухэлементные подмассивы для каждого отдельного преобразования. Дополнительными элементами могут влиять на преобразование.

#### Поиск необходимых кодировок

`::asciicompat_encoding(encoding) # -> encoding`

Используется для получения кодировки, совместимой с переданной и с ASCII. Если это переданная кодировка, то возвращается nil.

~~~~~ ruby
  Encoding::Converter.asciicompat_encoding "utf-8" # -> nil
  Encoding::Converter.asciicompat_encoding "utf-16le"
  # -> #<Encoding:UTF-8>
~~~~~

`::search_convpath( source_enc, dest_enc, options = nil ) # -> array`

Путь преобразования.

~~~~~ ruby
  Encoding::Converter.search_convpath "ISO-8859-1", "EUC-JP",
    universal_newline: true
  # -> [ [ #<Encoding:ISO-8859-1>, #<Encoding:UTF-8> ],
  #   [ #<Encoding:UTF-8>, #<Encoding:EUC-JP> ],
  #   "universal_newline" ]
~~~~~

#### Статистика

`.inspect # -> string`

Информация об объекте.

~~~~~ ruby
  Encoding::Converter.new( "ISO-8859-1", "EUC-JP" ).inspect
  # -> "#<Encoding::Converter: ISO-8859-1 to EUC-JP>"
~~~~~

`.convpath # -> array`

Путь преобразования.

~~~~~ ruby
  Encoding::Converter.new( "ISO-8859-1", "EUC-JP" ).convpath
  # -> [ [ #<Encoding:ISO-8859-1>, #<Encoding:UTF-8> ],
  #   [ #<Encoding:UTF-8>, #<Encoding:EUC-JP> ] ]
~~~~~

`.source_encoding # -> encoding`

Исходная кодировка.

~~~~~ ruby
  Encoding::Converter.new("ISO-8859-1",
    "EUC-JP").source_encoding
  # -> #<Encoding:ISO-8859-1>
~~~~~

`.destination_encoding # -> encoding`

Требуемая кодировка.

~~~~~ ruby
  Encoding::Converter.new("ISO-8859-1",
    "EUC-JP").destination_encoding
  # -> #<Encoding:EUC-JP>
~~~~~

`.replacement # -> string`

Текст для замены некорректных байтов или неопределенных символов.

~~~~~ ruby
  Encoding::Converter.new( "ISO-8859-1", "EUC-JP" ).replacement
  # -> "?"
~~~~~

`.replacement=(string) # -> string`

Используется для изменения текста, заменяющего некорректные байты или неопределенные символы.

`.last_error # -> error`

Последнее полученное исключение.

#### Преобразование

`.convert(text) # -> string`

Используется для преобразования кодировки. Некорректные байты или неопределенные символы считаются исключением в любом случае.

`.primitive_convert( text, result, start = nil, bytesize = nil, options = nil )# -> symbol`

Используется для преобразования кодировок без изменения значения объекта. Позволяется ограничивать фрагмент, в который сохраняется результат (по умолчанию в конец текста).

##### Опции:

`partial_input: true`, исходный текст может быть частью другого объекта;

`after_output: true`, после получения результата, ожидается новый исходный текст.

*****

Преобразование завершается при выполнении одного из следующих условий (в скобках указан возвращаемый результат):

+ Исходный текст содержит некорректные байты (`:invalid_byte_sequence`);

+ Неожиданный конец исходного текста.

  Это возможно, если `:partial_input` не задан (`:incomplete_input`);

+ Исходный текст содержит неопределенные символы (`:undefined_conversion`);

+ Данные выводятся до их записи.

  Это возможно, если ключ `:after_output` не задан (`:after_output`);

+ Буфер назначенного объекта полон.

  Это возможно, если bytesize не ссылается nil (`:destination_buffer_full`);

+ Исходный текст пуст.

  Это возможно, если `:partial_input` не задан (`:source_buffer_empty`);

+ Преобразование завершено (`:finished`).

`.primitive_errinfo # -> array`

Информация о последней ошибке преобразования в виде:

`[ symbol, source_enc, dest_enc, ivalid_byte, undef_char ]`,

где symbol - результат последнего вызова метода `.primitive_convert`.

Другие элементы имеют смысл только для `:invalid_byte_sequence`,  
`:incomplete_input` или `:undefined_conversion`.

`.putback # -> string`

Фрагмент текста, который будет преобразован при следующем вызове `.primitive_convert`.

`.insert_output(string) # -> nil`

Используется для добавления текста, необходимого к преобразованию. Когда требуемая кодировка сохраняет свое состояние, текст будет преобразован в соответствии с состоянием, которое будет обновлено после преобразования. Этот метод необходимо использовать только если при преобразовании получено исключение.

`.finish # -> string`

Используется для завершения преобразования.
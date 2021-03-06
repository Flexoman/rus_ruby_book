## Enumerable

Модуль содержит методы для работы с составными объектами, реализующими перебор своих элементов с помощью метода `.each`.

Также для некоторых методов может понадобиться определение оператора `<=>`.

Ассоциативные массивы преобразуются в индексные с помощью метода `.to_a`.

### Приведение типов

`.to_a # -> array`  
Синонимы: `entry`

Преобразование в индексный массив.

### Элементы

`.first( size = nil ) # -> object`

Используется для получения первого элемента или начального фрагмента. Если методу передается ноль, то возвращается пустой массив.  
`[1, 2, 3].first 2 # -> [1, 2]`

`.take(size) # -> array`

Используется для получения фрагмента заданного размера. Если методу передается ноль, то возвращается пустой массив.  
`[1, 2, 3].take 2 # -> [1, 2]`

`.drop(size) # -> array`

Используется для удаления фрагмента заданного размера.  
`[1, 2, 3].drop 2 # -> [3]`

### Сортировка и группировка

`.sort # -> array`

`{ |first, second| } # -> array`

Используется для сортировки элементов либо с помощью оператора `<=>`, либо на основе результатов итерации.  
`{ a: 1, b: 2, c: 3 }.sort # -> [ [:a, 1], [:b, 2], [:c, 3] ]`

`.sort_by { |first, second| } # -> array`

Используется для сортировки в восходящем порядке на основе результатов итерации.

~~~~~ ruby
  { a: 1, b: 2, c: 3 }.sort_by { |array| -array[1] }
  # -> [ [:c, 3], [:b, 2], [:a, 1] ]
~~~~~

`.group_by { |object| } # -> hash`

Используется для группировки элементов на основе результата итерации.  
`[1, 2].group_by { |elem| elem > 4 } # -> { false => [1, 2] }`

`.zip(*object) # -> array`

`(*object) { |array| } # -> nil`

Используется для группировки элементов с одинаковыми индексами. Группы могут передаваться в необязательный блок.

Количество групп равно размеру объекта, для которого метод был вызван. Остальные объекты при необходимости дополняются элементами, ссылающимися на nil.

~~~~~ ruby
  { a: 1, b: 2 }.zip [1, 2], [1]
  # -> [ [ [:a, 1], 1, 1 ], [ [:b, 2], 2, nil ] ]
~~~~~

`.chunk { |object| } # -> enum`

`(buffer) { |object, buffer| } # -> enum`

Используется для группировки элементов на основе результатов итерации.

Результат выполнения блока для дальнейшего использования может быть сохранен с помощью дополнительного аргумента.

Блок может возвращать специальные объекты:

+ *nil* или *:_* - игнорировать элемент;  
  *:_alone* - элемент будет единственным в группе.

`.slice_before(object) # -> enum`

`{ |object| } # -> enum`

`(buffer  { |object, buffer| } # -> enum`

Используется для группировки элементов. Новая группа начинается с элемента равного переданному аргументу (сравнение выполняется с помощью оператора `===`), или с положительным результатом итерации.

Первый элемент игнорируется.

В перечне каждая группа объектов сохраняется в виде индексного массива.

### Поиск элементов

`.count( object = nil ) # -> integer`

`{ |object| } # -> integer`

Используется для получения количества элементов, либо равных переданному аргументу, либо с положительным результатом итерации. При вызове без аргументов возвращает количество элементов.  
`[ 1, 2, 3 ].count { |elem| elem < 4 } # -> 3`

`.grep(object) # -> array`

`(object) { |object| } # -> array`

Используется для получения всех элементов, равных переданному аргументу (сравнение выполняется с помощью оператора `===`).

При получении блока вместо элементов возвращается результат их итерации.
`[ 1, 2, 3 ].grep(2) { |elem| elem > 4 } # -> [false]`

`.find_all { |object| } # -> array`  
Синонимы: `select`

Используется для получения всех элементов с положительным результатом итерации.  
`[1, 2, 3].find_all { |elem| elem > 4 } # -> [ ]`

`.reject { |object| } # -> array`

Используется для удаления всех элементов с положительным результатом итерации.  
`[1, 2, 3].reject { |elem| elem > 4 } # -> [1, 2, 3]`

`.partition { |object| } # -> array`

Используется для получения фрагментов с различными логическими результатами итерации.  
`[1, 2, 3].partition { |elem| elem > 2 } # -> [ [3], [1, 2] ]`

`.detect( default = nil ) { |elem| } # -> object`  
Синонимы: `find`

Используется для поиска первого элемента с положительным результатом итерации. Если искомый элемент не найден, то возвращается либо nil, либо значение по умолчанию.  
`[1, 2, 3].detect { |elem| elem > 4 } # -> nil`

`.find_index( object = nil ) # -> index`

`{ |object| } # -> index`

Используется для поиска индекса первого элемента, либо равного переданному аргументу, либо с положительным результатом итерации. Если элемент не найден, то возвращается nil.  
`[1, 2, 3].find_index { |elem| elem > 4 } # -> nil`

### Сравнение элементов

Сравнение выполняется с помощью оператора `<=>`.

`.min # -> object`

Наименьший элемент.  
`[1, 2, 3].min # -> 1`

`.max # -> object`

Наибольший элемент.  
`[1, 2, 3].max # -> 3`

`.minmax # -> array`

Наименьший и наибольший элементы.  
`[1, 2, 3].minmax # -> [1, 3]`

`.min_by { |object| } # -> object`

Элемент с наименьшим результатом итерации.  
`[1, 2, 3].min_by { |elem| -elem } # -> 3`

`.max_by { |object| } # -> object`

Элемент с наибольшим результатом итерации.  
`[1, 2, 3].max_by { |elem| -elem } # -> 1`

`.minmax_by { |object| } # -> array`

Элементы с наименьшим и наибольшим результатами итерации.  
`[1, 2, 3].minmax_by { |elem| -elem } # -> [3, 1]`

### Предикаты

`.include?(object)`  
Синонимы: `member?`

Проверка наличия элемента, равного переданному аргументу (сравнение выполняется с помощью оператора `==`).  
`[1, 2, 3].include? 4  # -> false`

`.all? { |object| }`

Проверка отсутствия элементов с отрицательным результатом итерации. При отсутствии блока проверяется каждый элемент.  
`[1, 2, 3].all? { |elem| elem < 4 } # -> true`

`.any? { |object| }`

Проверка наличия хотя бы одного элемента с положительным результатом итерации. При отсутствии блока проверяется каждый элемент.  
`[1, 2, 3].any? { |elem| elem < 4 } # -> true`

`.one? { |object| }`

Проверка наличия только одного элемента с положительным результатом итерации. При отсутствии блока проверяется каждый элемент.  
`[1, 2, 3].one? { |elem| elem < 4 } # -> false`

`.none? { |object| }`

Проверка отсутствия элементов с положительным результатом итерации. При отсутствии блока проверяется каждый элемент.  
`[1, 2, 3].none? { |elem| elem < 4 } # -> false`

### Итераторы

`.collect { |object| } # -> array`  
Синонимы: `map, collect_concat, flat_map`

Перебор элементов с сохранением результатов итерации.

~~~~~ ruby
  [1, 2, 3].collect { |elem| elem < 4 } # -> [true, true, true]
  (1..3).collect(&:next) * ?| # -> "2|3|4"
~~~~~

`.reverse_each( *arg ) { |object| } # -> self`

Перебор элементов в обратном порядке.

`.each_with_index { |object, index| } # -> self`

Перебор элементов вместе с индексами.

`.each_with_object(object) { |elem, object| } # -> object`

Перебор элементов вместе с переданным объектом.

`.each_slice(size) { |array| } # -> nil`

Перебор фрагментов заданного размера.

`.each_cons(size) { |array| } # -> nil`

Перебор фрагментов заданного размера. После каждой итерации из начала фрагмента будет удаляется элемент, а в конец будет добавлен следующий элемент составного объекта.

`.each_entry { |object| } # -> nil`

Перебор элементов. Несколько объектов, переданных инструкции yield в теле метода `.each`, сохраняются в индексном массиве.

### Циклы

`.drop_while { |object| } # -> array`

Выполнение блока для всех элементов кроме первого вплоть до получения отрицательного результата итерации. Возвращаются элементы, итерация которых не выполнялась. Иными словами элементы удаляются пока результат итерации положителен.
`[1, 2, 3].drop_while { |elem| elem < 4 } # -> [ ]`

`.take_while { |object| } # -> array`

Выполнение блока для всех элементов кроме первого вплоть до получения отрицательного результата итерации. Возвращаются элементы, итерация которых выполнялась. Иными словами элементы сохраняются пока результат итерации положителен.  
`[1, 2, 3].take_while { |elem| elem < 4 } # -> [1, 2, 3]`

`.cycle( step = nil ) { |object| } # -> nil`

Выполнение блока для всех элементов в бесконечном цикле. Выполнение может быть остановлено с помощью инструкций или ограничения количества циклов. Если методу передано отрицательное число, то вызов метода завершается до выполнения.

### Остальное

`.lazy # -> a_lazy_enumerator [Ruby 2.0]`

Используется для создания перечня, позволяющего выполнять [отложенные вычисления](lazy).

`.inject(method) # -> object`

`(first, method) # -> object`

`(first = nil) { |buffer, first| } # -> buffer`  
Синонимы: `reduce`

Используется для объединения элементов либо с помощью указанного метода, либо передавая результат каждой итерации первому параметру блока. Выполнение начинается с первого элемента или с дополнительно переданного аргумента.

Часто используется в функциональном стиле программирования и известно как "свертка".  
`[1, 2, 3].inject( 100, :+ ) # -> 106`
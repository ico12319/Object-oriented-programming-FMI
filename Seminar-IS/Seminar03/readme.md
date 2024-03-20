# Трети семинар по обектно ориентирано програмиране - информационни системи.

## Състояния на поток
При работа с потоци е възможно да възникнат различен тип грешки. За да имаме максимално ясна представа какво се случва с потока ни разполагаме с четири състояния на потоците:
* good - Няма грешки.
* bad  - Грешка при четене/писане в потока.
* fail - Последната входно/изходна операция е невалидна.
* eof  - Достигнат е края на потока.

Четирите състояния можем да проверим използвайки предоставени функциите:

```cpp
std::cin.good();  // дали потока е в състояние good?
std::cin.bad();   // дали потока е в състояние bad?
std::cin.fail();  // дали потока е в състояние fail?
std::cin.eof();   // дали потока е в състояние eof?
```

Понякога може да чуете състоянията да бъдат наричани битове (примерно `badbit`). Това е с причина - всеки път когато състояние се промени бит се вдига или сваля. Потоците пазят число, чрез което, проверявайки битовете му, разбират какво е състоянието на потока.

Ако искаме да изчистим състоянието на потока (да го върнем към състояние `good`) можем да използваме функцията `clear()`.

## Позициониране във файлове
Когато искаме да четем от файл, потокът вътрешно пази докъде сме стигнали. След като сме прочели веднъж донякъде част от потока можем да се върнем назад и да я прочетем отново.

* Входните потоци `istream` пазят `get` указател.
* Изходните потоци `ostream` пазят `put` утазател.

Разбира се, можем да ги манипулираме използвайки следния интерфейс:
* istream::tellg() - Връща позицията на `get` указателя.
* ostream::tellp() - Връща позицията на `put` указателя.
* istream::seekg(std::streamoff offset, std::ios::seekdir dir) - променя позицията на `get` указателя.
* ostream::seekp(std::streamoff offset, std::ios::seekdir dir) - променя позицията на `put` указателя.

Какво точно представлява `std::ios::seekdir` dir?

То има три стойности:
* std::ios::beg - началото на потока.
* std::ios::cur - текущата позиция на потока.
* std::ios::end - края на потока.

А какво представлява `std::streamoff`? То е някакво число (може и отрицателно). Тази комбинация ни казва точно къде да позиционираме указателя.

Ако искаме да го върнем в началото:
`ifs.seekg(0, std::ios::beg)` - премести get указателя 0 позиции от началото

Ако искаме да го поставим в края:
`ifs.seekg(0, std::ios::end)`

Ако искаме да го пренасочим 2 символа напред:
`ifs.seekg(2, std::ios::cur)`

Или 2 символа назад:
`ifs.geekg(-2, std::ios::cur)`

## Двоични файлове
В семинар 2 разгледахме работа с текстови файлове. Readme файла, който четете вмомента, е текстов файл. Текстовите файлове са четими от хората и са форматирани за тях по подходящ начин.

Примерно, ако си мислим за файла като последователност от байтове, то новия ред също е байт. Но когато отваряме файл не виждаме този символ а текстът се премества на нов ред. Операционната система знае формата на този файл и го представя пред нас по подходящ начин.

Не винаги обаче имаме нужда от подходящо форматирани файлове. Понякога имаме нужда единствено да запазим информацията, без тази информация да бъде четима за човек. Това представляват и двоичните файлове - последователност от байтове, които не са четими за хората.

Пишейки c++ код .cpp файла е текстов. Той е форматиран добре за да можем да се ориентираме лесно.
От друга страна, .exe файла, който се получава след компилация е двоичен. Той е файлов формат, който операционната система може да изпълни, но човек не може да прочете.

Отваряйки поток за работа с файлове, по подразбиране очакваме да работим с текстови файлове. Можем експлицитно да окажем, че искаме да работим с двоични файлове по следния начин:

```cpp
std::ifstream inFile("file.dat", std::ios::binary);
```
Втория аргумент на потока оказва, че потока ще работи в двоичен режим.

## Режими на потока
Разбира се, има и още режими на потоците. Нека разгледаме кои са те и какво правят:
* std::ios::app - Премества `put` указателя в края на всяка операция по писане. Съкратено от append
* std::ios::binary - Отваря потока в двоичен режим
* std::ios::in - Отваря потока в режим за четене
* std::ios::out - Отваря потока в режим за писане
* std::ios::trunc - Премахва цялото съдържания на файла. Съкратено от truncate.
* std::ios::ate - Премества `put` указателя в край единствено при отваряне.

Но защо са ни std::ios::in и std::ios::out? Нали по подразбиране имахме поток за вход и поток за изход?
Съществува поток, който може едновременно да пише и да чете от файл. Този поток се казва `std::fstream`.

* std::fstream - По подразбиране е в режим std::ios::in | std::ios::out.
* std::ifstream - По подразбиране е в режим std::ios::in.
* std::ofstream - По подразбиране е в режим std::ios::out.

Нека по-подробно разгледаме режима на `fstream` а именно std::ios::in | std::ios::out. Този оператор `|` е добре познатото ни побитово или. Това в какъв режим е файла се пази отново в число. По това кой бит е вдигнат и кой не разбираме какъв е режима на работа. Ако искаме да сложим повече от един режим просто правим побитово и.

```cpp
// Отваря поток за четене, в двоичен режим и изтрива цялото съдържание.
std::fstream fs(std::ios::out | std::ios::trunc | std::ios::binary);
```

## Писане и четене при потоци в двоичен режим.
Разбира се, имаме специални функции за писане и четене от файлове, когато потоците са в двоичен режим.

* read(char* buffer, size_t size) - прочита `size` байта от потока и ги записва в `buffer`
* write(const char* data, size_t size) - Записва `size` байта от `data` във файла.

Нека разгледаме примери:
```cpp
#include <iostream>
#include <fstream>
#include <cstring>
int main()
{
	// mode: std::ios::binary | std::out::in въпреки, че не сме
	// казали експлицитно, че е in.
	std::ofstream outFile("test.dat", std::ios::binary);

	if (!outFile.is_open())
		return 1;

	int x = 42;
	const char* text = "Dump me!!!";

	// We can write text:
	outFile.write(text, strlen(text) /* how many bytes? strlen bytes! */);

	// We can write variables (but with cast!)
	outFile.write((const char*)(&x), sizeof(int) /* how many bytes? sizeof(int) bytes! */);

	int arr[4] = { 1, 2, 3, 4 };

	// We can write arrays!!!
	outFile.write((const char*)(arr), 4 * sizeof(int)/* We have 4 ints here. */);
}
```
Нека погледнем как изглежда файла:
![](media/binary-file-example.jpg)

Можем да разберем част от данните, но не всички. Знаейки как сме го записали можем да го прочетем:

```cpp
#include <iostream>
#include <fstream>
#include <cstring>

int main()
{
	// mode: std::ios::binary | std::out::in въпреки, че не сме
	// казали експлицитно, че е in.
	std::ifstream inFile("test.dat", std::ios::binary);

	if (!inFile.is_open())
		return 1;

	int x;
	char text[64]{};

	// Read the text:
	inFile.read(text, 10);

	// Read back the variable
	inFile.read((char*)(&x), sizeof(int));

	int arr[4] = { 1, 2, 3, 4 };

    // Finally read back the array:
	inFile.read((char*)(arr), 4 * sizeof(int));

    std::cout << text << std::endl << x << std::endl;

    for (size_t i = 0; i < 4; i++)
        std::cout << arr[i] << ' ';    
}
```

## Бонус - четене на двоични файлове
Разбира се, човек, който няма достъп до кода няма как да звае в какъв формат сме записали данните. Това си знаем ние.

Удобно е да разглеждаме съдържанието на двоичния файл като Hex-View. Ако разгледаме число в шестнаддесетична бройна система съставено от две цифри, неговата минимална стойност е 0 (съставено от 00) а неговата максимална стойност е 255 (съставена от FF = 15*16 + 15 = 255).

Това прави такъв тип числа удобни за представяне на байтове. Hex-View на един файл е точно това - редица от байтовете, представени като числа в шестнаддесетична бройна система.

Нека разгледаме пример:
![](media/hex-dump.png)

В началото изглежда нелогично, но нека помислим. За наше удобство отдясно е записано как файла би се интерпретирал в ASCII encoding.

Първото число, което е записано в шестнаддесетична бройна система е 44. Стойността на това число в десетична бройна система е 68. Ако отворим и погледнем ASCII таблицата това е точно стойността на символа D. Аналогично за останалите ascii символи.

Лесно можем да проследим докъде приключва ascii стринга, който записахме. След това имаме `2A 00 00 00` което е точно int променливата х, която записахме със стойност 42. След това има още шестнаддесет байта - четири по четири за четирите числа, които записахме от масива.

## Задача първа
Да се напише функция, която намира големината на файл.

## Задача втора
Да се напише функция, която прочита двоичен файл, в който е записан масив от int променливи. Функцията да запише четните числа в един файл, а нечетните в друг.
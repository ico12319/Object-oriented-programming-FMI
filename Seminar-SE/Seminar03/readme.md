# Трети семинар по обектно ориентирано програмиране - 09.03.2024

## Преговорени теми от лекции:
* Двоични файлове.
* read & write функции.
* std::stringstream
* flush функцията.

## Задача първа
Да се напише функция `modifyColumn(const char* filePath, const char* columnName, const char* values, char separator)` където:
* filePath - път към файл във формат `.csv`
* columnName - колоната, която ще променяме
* values - стойностите, които ще записваме в колоната
* separator - символът, който разделя стойностите записани във `values` масива.

Ако стойностите са по - малко от дължината на колоната се записват всички. Ако са повече се записват докато колоната не се напълни.

## Задача втора
В двоичен файл е записан масив от числа. В два файла да се запишат нечетните и четните.

## Задача трета
Да се изведат на стандартния изход всички байтове, които не съществуват в двоичен файл.
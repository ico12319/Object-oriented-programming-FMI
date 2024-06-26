## Задача 1 

Реализирайте клас **Билет**, който описва билет за театрална постановка. Всеки билет има име на постановката и цена. Направете подходящи конструктори. 

Реализирайте клас **Студентски билет**, който е 2 пъти по-евтин. В конструктора си приема име и оригинална цена за постановката. 
Реализирайте клас **Групов билет**, който е с 20% по-евтин от нормалния. 
Направете подходящи функции за принтиране информацията на билетите.

## Задача 2

Създайте клас `CarPart`, описващ част за кола.
- идентификатор - уникално число > 0;
- име на производител - низ с произволна дължина;
- описание - низ с произволна дължина. 

Създайте класове за различни части за кола - `Гума`, `Двигател`, `Акумулатор`

`Гума`
- широчина в мм – между 155 и 365;
- профил – между 30 и 80;
- диаметър в инчове – между 13 и 21.

`Двигател`
- конски сили.

`Акумулатор`
- капацитет – в ампер-часове;
- battery id – идентификатор, независим от този за всяка част (низ с произволна дължина)

</br>

Създайте и подходящи оператори `<<`, които да извеждат информация за част в следния формат:
- за двигател:
```
(<id>) by <manufacturer> – <description> – <horsepower> hp
```
- за гума:
```
(<id>) by <manufacturer> – <description> – <width>/<profile>R<rim size>
```
- за акумулатор:
```
(<id>) by <manufacturer> – <description> – <amp hours> Ah
```

**Създайте операторите с минимална дупликация на код.**

Направете клас за горивото `FuelTank`, който има

- капацитет
- пълнота към момента

Създайте конструктор, приемащ капацитет на резервоара. Всеки нов резервоар първоначално е пълен.

Създайте методи:
- за използване на дадено количество гориво
- за зареждане на дадено количество гориво:

**При зареждане на прекалено много гориво, резервоарът остава пълен.**

При опит за използване на количество гориво, повече от наличното,
да се хвърля custom изключение `insufficient_fuel_error`, което е пряк наследник на `std::logic_error`и да може да се създава такова с единствен параметър – съобщение, описващо грешката.

Използвайте някой конструктор на `std::logic_error`, за да изградите този на `insufficient_fuel_error`.

Реализирайте клас `Car`, използвайки частите, които създадохте.

Една кола има:
- двигател;
- акумулатор;
- резервоар;
- 4 гуми;
- изминати километри (пробег);
- тегло (в кг).

Създайте конструктор за кола, приемащ всички нужни член данни, както и капацитет на резервоара.

Създайте get-ър за резервоара.

Създайте член-функция `drive(double km)`:
- добавя се дистанцията към изминатите километри;
- намалява се количеството гориво в резервоара с произведението на `km` и изразходваното гориво на 1 км **(разхода се счита, че е 1л/1км)**

**!!!** Ако колата няма достатъчно гориво, не се променя състоянието ѝ по никакъв начин.

**Бонус:**
Създайте външна за класа функция за драг състезание между две коли:
```
Car* dragRace(Car* car1, Car* car2);
```

To се случва на разстояние 0.4 km (1/4 mile).
Функцията трябва да връща `Car*`, сочейки към евентуалния победител от него.

- **Ако една от колите няма** достатъчно гориво, за да измине разстоянието, другата печели.

- **Ако и двете коли нямат** достатъчно гориво, да се върне `nullptr`.

- **Ако и двете коли имат** достатъчно гориво, победителят се определя от съотношението: тегло / конски сили на двигателя **(Колата с по-високо съотношение печели.)**


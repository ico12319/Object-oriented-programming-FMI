 ## Задача 1:
 
 Създайте клас `Submission`, който се описва от: 
 - Уникално ID, което не се подава в конструктора 
 - Име с произволна дължина
 - Състояние на текущата задача - `InDevelopment`, `Active`, `Expired`

 Създайте клас `JudgeSystem`, който управлява система за `Submission-и`. Представете си, че тя ще е много голяма и трябва да се подсигури само една инстанция от този клас. Имплементирайте функционалностите: 

 - В конструктора да се подаде число, съответстващо на максималния брой `Submission-и`, които могат да бъдат в системата
 - Добавяне на `Submission`чрез оператор `+=` (ако е достигнат максималния брой, да се хвърли подходящо изключение)
 - Премахване на `Submission` чрез оператор `-={name}` (ако не съществува такъв `Submission` или не е в състояние `Expired`, да се хвърли подходящо изключение). **Използвайте подходящо структуриране на данните, така че да не се правят копия при премахване на `Submission`**. При премахване да се добавят в класа `JudgeArchive` без да се създава инстанция на класа
 - оператор `[name]`, който връща `Submission` с това име и чрез него може той да бъде подменян с нов (ако не съществува с такова име да се хвърли изключение)
 - оператор `<<`, който изпечатва на съответния поток само тези `Submission-и`, които са в състояние `Active` и `Expired` и са в системата. След като бъдат изпечатани, да се изпечатат всички `Submission-и`, които са в `JudgeArchive` (измислете вие начин как да го направите)

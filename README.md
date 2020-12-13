Лабораторная работа 4
Каждая папка предсталяет из себя отдельный проект
На основании 5+ таблиц из базы данных AdventureWorks  была создана  модель PeronalInfo на стороне .net (в папке Modelss, Models.cs),в которой я собрал необходимый набор данных для последующей обработки.
Также (в папке Modelss, Models.cs) имеются модели FindCriteria(содержит набор параметров для запроса данных)и SearchRes(хранилище полученных данных с БД).
В папке Data Access Layer реализован слой интерфейсов с методами чтения данных,а именно:
Repositories.cs содержит класс для чтения данных с БД(класс Repository реализует соответсвующий интерфейс и использует классы из (DALconfig.cs)).Класс DALconfig содержит необходимые данные для запроса и подключения к БД.Класс SqlCommandExtensions используется для замены метода считывагия данных из БД(т.о. класс предоставляет самописные методы расширения ,с помощью которых мы указываем ,каким образом мы хотим считывать данные).ЗАПРОС ДАННЫХ ВЫПОЛНЯЕТСЯ В ВИДЕ ТРАЗАКЦИИ С ПРИМЕННЕНИЕМ ХРАНИМОЙ ПРОЦЕДУРЫ.

В папке Service Layer реализован набор интерфейсов с методпми для работы с API Data Access Layer:
в нем разработаны классы XmlPerson и Transactions,которые реализуют соотвествующие интерфесы и выполняют работу по созданию Xml файла и схемы из полученног набора данных и отправке их в папку,которую мониторит уже разработанный File Manager

Проекты Models,Data Access Layer,Service Layer выставлены как зависимости

В папке DataManager содержится класс DataManager и соотвествующая Windows-служба. DataManager содержит функционал по считыванию ,обработке и отправке данных.Перед стартом работы,данная модель наполняется необходимыми данными с помощью Configuration Manager(использует Xml/Json парсеры ,реализующие интерфейс с методом по предоставлению наполненной модели ,используя файлы конфигурации(папка ConfigurationManager)).Далле запускатся работа DataManager. (***)Считывается набор данных,обрабатывается,и отрапляется вместе с Xsd схемой.Для ограничения считывания данных введена пауза между запросами БД (т.о. периодически будет происходить вышеописанный процесс (***)

Также разработан класс ErrorLogger и реализован соотвествующий интерфейс для отлова ошибок службы и отправки информации о них на специально разработанную бд с таблицей для ошибок.
ЗАПРОС НА ДОБАВЛЕНИЕ НОВОЙ ИНФОРМАЦИИ ОБ ОШИБКАХ ОСУЩЕСТВЛЯЕТСЯ ЧЕРЕЗ ТРАЗАКЦИЮ С ИСПОЛЬЗОВАНИЕМ ХРАНИМЫХ ПРОЦЕДУР.
![Image alt](https://github.com/SoulHowl/Lab4/blob/main/AdventureWorks/photo/Errorpeg.png)


После того,как файл попал в папку ,которая мониторится,в работу вступает уже разработанный из 3 лабы FileManager(производится валидация файла,шифрование,архивирование,перемещение,деархивирование,дешифрование,и дополнительно сохранение архива файла в папке Archive)

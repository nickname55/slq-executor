## Project overview
Проект представляет собой API,
позволяющее отправить HTTP-запрос с параметрами и запустить этим выполнение запроса к базе данных.

В параметрах HTTP-запроса необходимо указать имя SQL-запроса 
и список параметров SQL-запроса 
и значения этих параметров.

Доступные для выполнения запросы с перечнем допустимых параметров и типов этих параметров описаны в конфигурационном файле
(application.conf)

В результате выполнения запроса формируется результат(ты), 
который вставляется в соответствующие переменные шаблона.

На основании шаблона и результатов запроса, формируется ответ отправляемый пользователю API.

Имя файла шаблона связанного с SQL-запросом указывается в конфигурационном файле.

Пример, вызова запроса с именем "query1"
и параметрами
* "id" (значение "5")
* "param2" (значение "33")
```
/sql?name=query1&id=5&param2=33
```

## Сборка и развертывание проекта
Сборка war-файла выполняется maven-плагином maven-war-plugin

Для запуска сборки выполните команду: 
```
mvn clean package
```  
При этом следует обратить внимание что MySQL JDBC Driver указан в зависимостях поекта как provided
и jar-файл драйвера соответствующей версии должен быть помещен в директорию TOMCAT_HOME/lib

## Запуск
Загрузите war в Tomcat

Сервис будет доступен по адресу http://localhost:8080/sql?name=query1&id=5

Пример передачи параметров:
```
http://localhost:8080/sql?name=query1&id=5&param2=33
```

## Конфигурационный файл

```
queries = [{
	QUERY_NAME = "query1"//имя запроса передающееся в запросе
	SQL = "select count(*) from cars where id > :id" //тело запроса, параметры указаны как :paramName
	PARAMS = [ //список параметров
	      { //первый параметр
              		NAME = "id" //имя параметра
              		TYPE = "BOOLEAN" //тип параметра JDBC типы из java.sql.Types 
              }
              ,
              { //второй параметр
                		NAME = "param2" //имя параметра
                		TYPE = "DECIMAL" //тип параметра JDBC типы из java.sql.Types 
              }
	]
	TEMPLATE = "templates/simple.vm" //имя файла шаблона, шаблоны находятся директории resources/templates
}
]
```
   

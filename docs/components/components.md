# Компонентная архитектура
<!-- Состав и взаимосвязи компонентов системы между собой и внешними системами с указанием протоколов, ключевые технологии, используемые для реализации компонентов.
Диаграмма контейнеров C4 и текстовое описание. 
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375783368
-->
## Контейнерная диаграмма

```plantuml
@startuml
'!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

!include <C4/C4_Container>

'AddElementTag("microService", $shape=EightSidedShape(), $bgColor="CornflowerBlue", $fontColor="white", $legendText="microservice")
'AddElementTag("storage", $shape=RoundedBoxShape(), $bgColor="lightSkyBlue", $fontColor="white")

Person(user_web, "Пользователь", "")
Person(user_android, "Пользователь", "")
Person(user_ios, "Пользователь", "")


System(web, "Web", "")
System(mobileios, "Mobile App\nAndroid", "")
System(mobileandroid, "Mobile App\niOS", "")

'Lay_D(user_web, web)
'Lay_D(user_ios, mobileios)
'Lay_D(user_android, mobileandroid)

Rel(user_web, web, "Использует")
Rel(user_ios, mobileios, "Использует")
Rel(user_android, mobileandroid, "Использует")

System_Boundary(c, "helloconf") {
    'Container(conference_service, "Conference API", "Java, Spring Boot", "Сервис реализующий API работы с конференциями", $tags = "microService")      
    ContainerDb(conference_db, "Conference DB", "PostgreSQL", "Хранение информации о конференциях", $tags = "storage")

    ContainerDb(storage, "Media Storage", "S3", "Файловое хранилище медиа данных")      

    'Container(sso, "SSO", , $tags = "microService")      
    ContainerDb(sso_db, "SSO DB", "PostgreSQL", "Хранение информации о пользователях", $tags = "storage")

    System_Boundary(sso, "SSO","Java, Spring Boot", "Сервис авторизации и регистрации пользователей") {
        System(user_agg, "Пользователь", "")       
    }

    Boundary(conference_service, "Conference API", "System", "Сервис реализующий API работы с конференциями", $tags = "microService") {
        System(conf_agg, "Выступление", "")
        System(stream_agg, "Стрим", "")
        System(attach_agg, "Материал", "")
    }
}

Rel_D(web, sso, "API","HTTPS")
Rel(mobileios, sso, "API","HTTPS")
Rel(mobileandroid, sso, "API","HTTPS")


Rel_R(sso, sso_db, "Данные пользователя", "SQL")
Rel_D(conference_service, conference_db, "Данные о конференциях", "SQL")
Rel_D(conference_service, storage, "Файловое хранилище медиаданных", "S3")

Rel_D(sso, conference_service, "Аутентифицирует", "HTTP")

Rel_U(attach_agg, conf_agg, "Ссылается")
Rel_U(stream_agg, conf_agg, "Ссылается")


SHOW_LEGEND()
@enduml
```

## Список компонентов
| Компонент             | Роль/назначение                  |
|:----------------------|:---------------------------------|
| SSO | Авторизация и регистрация пользователей, аутентификация запросов к API |
| Conference API | Сервис реализующий API работы с конференциями |
| Media Storage | Файловое хранилище медиаданных (записи видео/аудио, дополнительные материалы к встречам) |

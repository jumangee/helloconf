# Контекст решения
<!-- Окружение системы (роли, участники, внешние системы) и связи системы с ним. Диаграмма контекста C4 и текстовое описание. 
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375783261
-->
```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

LAYOUT_WITH_LEGEND()

Person(user_web, "Пользователь", "")
Person(user_mobile, "Пользователь", "")

System(web, "Web", "")
System(mobile, "Mobile App", "")

System(helloconf, "Helloconf", "")

Rel(user_web, web, "Использует")
Rel(user_mobile, mobile, "Использует")

Rel(mobile, helloconf, "API")
Rel(web, helloconf, "API")

@enduml
```

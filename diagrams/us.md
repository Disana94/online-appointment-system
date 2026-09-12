
@startuml
left to right direction
skinparam packageStyle rectangle

actor "Пользователь" as User
actor "Система" as Sys

rectangle "Модуль бронирования услуг" {
    usecase "Создать запись" as UC_Create
    usecase "Выбрать услугу" as UC_SelectService
    usecase "Выбрать мастера" as UC_SelectMaster
    usecase "Выбрать временной слот" as UC_SelectSlot
    usecase "Обработать конфликт бронирования\n(Слот уже занят)" as UC_Conflict
    
    ' Основной сценарий включает в себя подшаги
    UC_Create ..> UC_SelectService : <<include>>
    UC_Create ..> UC_SelectMaster : <<include>>
    UC_Create ..> UC_SelectSlot : <<include>>
    
    ' Альтернативный сценарий расширяет основной прецедент
    UC_Conflict ..> UC_Create : <<extend>>
}

User --> UC_Create
UC_Create --> Sys
@enduml

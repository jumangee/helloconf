# Модель предметной области
<!-- Логическая модель, содержащая бизнес-сущности предметной области, атрибуты и связи между ними. 
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375782602

Используется диаграмма классов UML. Документация: https://plantuml.com/class-diagram 
-->

```plantuml
@startuml
' Логическая модель данных в варианте UML Class Diagram (альтернатива ER-диаграмме).

hide empty members
'skinparam Linetype ortho
	

skinparam package {
  BorderColor     silver
}

namespace user {

    class User {
        id : string
        createdAt : datetime
        updatedAt : datetime
        status: Status
        role: Role
        name: string
        email: string
        password: string
    }

    enum Role {
        moderator
        reviewer
        admin
        superadmin
    }

    enum Status  {
        active
        blocked
        deleted
    }

    User -- Status
    User -- Role
}

namespace conference {
    namespace activity {
        class Activity {
            id : string
            createdAt: datetime
            updatedAt: datetime
            startAt: datetime
            speaker : User[]
            title: string
            description: string
            comments: Comment[]
            status: Status
            type: Type
            attachments: Attachment[]
            media: MediaSource
        }

        enum Status  {
            new
            scheduled
            active
            done
            deleted
        }

        Activity - Status
        Activity *-- "*" Comment
        Activity o-- "1..*" User

        namespace media {

            interface MediaSource {
                status: Status
            }

            enum Status  {
                active
                deleted
            }

            MediaSource - Status

            namespace video {
                interface Video {
                }

                class File {
                    video: File
                }

                class Stream {
                    output: OutputStream
                    input: UserStream[]
                }

                class UserStream {
                    input: InputStream[]
                    streamer: User
                }

                Stream .up.|> Video
                File .up.|> Video
                Stream *-- "*" UserStream
            }

            namespace podcast {
                interface Podcast {
                    presentation: File
                }

                class StreamPodcast {
                    output: OutputStream
                    input: InputStream
                    streamer: User
                }

                class FilePodcast {
                    audio: File
                }

                FilePodcast .up.|> Podcast
                StreamPodcast .up.|> Podcast
            }

            MediaSource <|.. video.Video
            MediaSource <|.. podcast.Podcast
        }

        namespace attachment {

            interface Attachment {
                id : string
                createdAt : datetime
                updatedAt : datetime
                author : User
                title: string
            }
            
            class ReviewAttachment {
                approve: boolean
                comment: string
            }

            class FileAttachment {
                file: File
            }

            Activity *-- "*" Attachment    

            ReviewAttachment .up.|> Attachment
            FileAttachment .up.|> Attachment
        }

        class Comment {
            id : string
            createdAt : datetime
            updatedAt : datetime
            author : User
            message: string
            visible: boolean
        }
    }

    class Conference {
        id : string
        createdAt : datetime
        updatedAt : datetime
        moderator : User[]
        scheduleStartAt: datetime
        title: string
        description: string
        schedule: Activity[]
        status: Status
    }

    enum Status {
        new
        scheduled
        active
        closed
    }

    Conference *-- "*" Activity
    Conference - Status
}  


conference.activity.Comment --*  User
Conference o-- "*" User
conference.activity.attachment.Attachment --* User
conference.activity.media.video.UserStream --* User
conference.activity.media.podcast.Podcast --* User
conference.activity.media.podcast.StreamPodcast --* User

conference.activity.media.podcast.StreamPodcast -- streaming.InputStream
conference.activity.media.video.Stream -- streaming.OutputStream
conference.activity.media.video.UserStream -- streaming.InputStream

conference.activity.Activity *--- "1" conference.activity.media.MediaSource

namespace storage {
    class File {
        id: string
        filename: string
        size: int
        fileServer: string
        filePath: string
        mimetype: string
        author: User
        createdAt: datetime
        status: Status
    }

    enum Status {
        active
        deleted
    }

    File -- Status
    File "*" -* User
}

conference.activity.media.video.File -- storage.File
conference.activity.attachment.FileAttachment -- storage.File
conference.activity.media.podcast.Podcast -- storage.File
conference.activity.media.podcast.FilePodcast -- storage.File

namespace streaming {
    class Server {
        id: string
        status: Status
        host: string
    }

    class OutputStream {
        id: string
        stream: InputStream[]
        storage: File
    }

    class InputStream {
        id: string
        accesstoken: string
        server: Server
    }

    enum Status {
        online
        offline
    }

    Server - Status
    Server *-- "*" InputStream
    Server *-- "*" OutputStream
    'OutputStream *- "*" InputStream
}

streaming.OutputStream -up-- storage.File

/'class ShoppingCart
{
    id : string
    createDate : datetime
    updateDate : datetime
    customer : Customer
    price : ShoppingCartPrice
    cartItems : CartItem[]
}

class ShoppingCartPrice
{
type : CartItemPrice
}
class CartItemPrice
{
type : CartItemPriceType
}

 enum CartPriceType
 {
  total
  grandTotal
  offeringDiscount
  couponsDiscount
 }

 class CartItem
 {
  id : string
  quantity : int
  offering : Offering
  relationship : CartItemRelationShip[]
  price : CartItemPrice[]
  status : CartItemStatus
 }

  class Customer
 {
  id : string
 }
 
 class Offering
 {
  id : string
  isQuantifiable : boolean
  actionType : OfferingActionType
  validFor : ValidFor
 }
  
 class ProductSpecificationRef
 {
  id : string
 }
 
 ShoppingCart *-- "1..*" ShoppingCartPrice
 ShoppingCartPrice -- CartPriceType
 ShoppingCart *-- "*" CartItem
 CartItem *-- "*" CartItemPrice
 CartItemPrice -- CartPriceType
 CartItem *-- "1" Offering
 Offering *-- "1" ProductSpecificationRef
 Offering *-- "0..1" ProductConfiguration
 ShoppingCart *-- "1" Customer
}

namespace Ordering {
 ProductOrder *-- OrderItem
 OrderItem *-- Product
 Product *-- ProductSpecificationRef
 ProductOrder *-- Party
}

namespace ProductCatalog {
 ShoppingCart.ProductSpecificationRef ..> ProductSpecification : ref
 Ordering.ProductSpecificationRef ..> ProductSpecification : ref
}

namespace CX {
 ShoppingCart.Customer ..> Customer : ref
 Ordering.Party ..> Customer : ref
}'/
@enduml
```

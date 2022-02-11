* Sangria cannot reuse case classes defined in our domain, it needs its own object of
type `ObjectType`. It allows us to decouple API/Sangria models from database
representation. This abstraction allows us to freely hide, add or aggregate fields.

REF: https://scalac.io/wp-content/uploads/GraphQL-Scala.pdf, https://www.howtographql.com/graphql-scala/3-handling-arguments/

* Go thru these demos in sequence to understand better https://github.com/OlegIlyenko/sangria-scalaio-demo/tree/master/src/main/scala/demos

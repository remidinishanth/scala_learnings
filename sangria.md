* Sangria cannot reuse case classes defined in our domain, it needs its own object of
type `ObjectType`. It allows us to decouple API/Sangria models from database
representation. This abstraction allows us to freely hide, add or aggregate fields.

REF: https://scalac.io/wp-content/uploads/GraphQL-Scala.pdf
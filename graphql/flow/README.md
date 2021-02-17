# GraphQL Flow
In this tutorial we shall try to understand the flow of a graphql query from frontend(like graphiql) to frontend.

One way of classifying data in graphql is scalar(single) types and custom(group of values) types.
* Scalar types are single values like an Int, Boolean, String, Float and ID. In other words it is not an object, which can have values inside it. 
* On the other hand, custom types are objects i.e they can contain group of scalar and custom types. You can find two custom(User and Post) types below.

First step to create graphql api is defining the type.

## define type two types, User and Post
```javascript
type User {
    _id: ID!
    name: String
    username: String
    email: String
}


type Post {
    _id: ID!,
    title: String!,
    content: String!,
    author: User
}
```

### What can we say seeing the type User?
* It is a custom type. It means, we need to provide a set of fields(It is called as **_Selection Set_** in graphql terminology), which we want to get back. Otherwise we will see an error called **_Field User of type User must have a selection of subfields_**
* We can request from 1 to 4 values (_id, name, username, email). We are saying atleast one because it is an object. Otherwise we see the error mentioned in the above statement.
* _id is mandatory. If _id is requested from the frontend client, then it cannot be null. Others can be null.
* Suppose there is a mutation declared like this ```createUser(name: String, username: String, email: String): User```. Here we are promising that the mutation will return a **_User_** type. It means that the following properties apply to the returned object:
    * If there are more fields, than those specified in the type are NOT returned. We can say that the extra(fields not specified in the type) fields are removed.
    * If there are less than 4 fields, null is returned for those fields which don't have a value. E.g if username is not present in the returned object, then its value is null.
    * if _id is requested and it is NOT present or its value is null, then graphql validation fails. This will throw  an error.

### What can we say seeing the type Post?
* It is a custom type. 
* It has 3 scalar fields and a custom field(author). Field author, returns a User object. So here we may need to provide two Selection Sets i.e one for Post and other for author(User).
* It can have upto four values (_id, tile, content, author)
* _id, title and content are mandatory. None of these three fields can be null, when requested from the frontend client. The only field which can be null, when requested is the author.
* Suppose there is a mutation declared like this ```createPost(title: String, content: String, author: String): Post```. Here i am promising that the mutation will return a **_Post_** type. It means that the following properties apply to the returned object:
    * If there are more fields, than those specified in the type are NOT returned. We can say that the extra(not specified in the type) fields are removed.
    * If there are less than 4 fields, null is returned for those fields which don't have a value. E.g the only field which can be null in this type is author. Other fields are assigned a value of null but then validation fails because they are mandatory.
    

# Naming REST Resources

Having a consistent naming strategy for REST resources will definitely prove to be one of the best design decisions in the long run.


> The key abstraction of information in REST is a resource. Any information that can be named can be a resource: a document or image, a temporal service (e.g. “today’s weather in Los Angeles”), a collection of other resources, a non-virtual object (e.g., a person), and so on. In other words, any concept that might be the target of an author’s hypertext reference must fit within the definition of a resource. A resource is a conceptual mapping to a set of entities, not the entity that corresponds to the mapping at any particular point in time.
<br>
[<p style='text-align: right;'>Roy Fielding's dissertation</p>](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm#sec_5_2_1_1)

REST APIs use [Uniform Resource Identifier](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier) (URI) to identify resources. A good and consistent naming strategy makes it easier to convey the resource model to client.

## Naming Conventions

1. **Use only lower case letters**


2. **Use forward slash (/) to indicate hierarchical relationships**

    The forward slash (/) character is used in the path portion of the URI to indicate a hierarchical relationship between resources. e.g

    > http://www.example.com/api/user-management
    http://www.example.com/api/user-management/users

    > http://www.example.com/api/user-management/users/{id}

    > http://www.example.com/api/post-management/users/{id}/posts
    
    > http://www.example.com/api/post-management/users/{id}/posts/{id}

3. **Use plural when refering to a collection resource**

    The plural form should be used when referring to groups of things. e.g.

    > http://www.example.com/api/user-management/users


4. **Use singular when refering to a singleton resource**
    
    We can identify a single user with the following URI:
    > http://www.example.com/api/user-management/users/{userId}


5. **Resources may contain sub-collection resources, conventions still apply**:

    > http://www.example.com/api/post-management/users/{userId}/posts

    We can also access nested singletons:

    > http://www.example.com/api/post-management/users/{userId}/posts/{postId}

6. **Do not use trailing forward slashes, they add no semantic value**

    As the last character within a URI’s path, a forward slash (/) adds no semantic value and may cause confusion. It’s better to drop them completely.

    **NOT THIS**
    > http://www.example.com/api/user-management/users/


    **THIS**
    > http://www.example.com/api/user-management/users

7. **Use kebab-case (hyphens), dont use snake_case (underscores)**

    **NOT THIS**
    > http://www.example.com/api/board-management/users/{userId}/project_boards

    **THIS**

    > http://www.example.com/api/board-management/users/{userId}/project-boards

8. **Do not use file extensions**

    File extensions look bad and do not add any advantage. Removing them decreases the length of URIs as well. No reason to keep them.

    Apart from above reason, if you want to highlight the media type of API using file extenstion then you should rely on the media type, as communicated through the Content-Type header, to determine how to process the body’s content.

    **NOT THIS**
    > http://www.example.com/api/board-management/users.php

    **THIS**
     > http://www.example.com/api/board-management/users

9. **Do not use CRUD function operations in URI**

    URIs should not be used to indicate that a CRUD function is performed. URIs should be used to uniquely identify resources and not any action upon them. [HTTP request methods](./README.md) should be used to indicate which CRUD function is performed.


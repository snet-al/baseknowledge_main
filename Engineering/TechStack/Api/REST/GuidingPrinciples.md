# Guiding principles of REST

1. **Client-Server**: The user interface is separated from the data storage. This improves portability on different systems and improves scalability by reducing the complexity of the server components.

2. **Stateless**: Each requests includes all the information required for completing said request. Session is managed entirely on the client side.

3. **Cacheable**: Each response (to a request) should be explicitly or implictly stated if it is cacheable or non-cacheable. In case of the former, the client may chose to reuse the response.

4. **Uniform Interface**: By applying software engineering principle of [generality](https://www.d.umn.edu/~tcolburn/cs4531/slides/software_engineering/principles/principles.xhtml), having a uniform interface allows the system to be simplified and the interactions be more visible. It is used as a mean to achieve complete decoupling. REST is defined by four interface constraints: identification of resources; manipulation of resources through representations; self-descriptive messages; and, hypermedia as the engine of application state.
    > **Note**: The hypermedia as the engine of application state (HATEOAS) should be taken with a [grain of salt](https://medium.com/@andreasreiser94/why-hateoas-is-useless-and-what-that-means-for-rest-a65194471bc8). Currently, not a lot of client choose to consume the REST APIs in such a way. In our standard, there client does not discover what REST APIs as it is browsing, but rather the REST API is [clearly documented](./DocumentingREST.md), and the client will choose to consume it according to its own needs.

5. **Layered System**: A system should be organized is layers and the constraint imposed on these layers is that should have no knowledge other than the immediate layer with which it is interacting.

6. **Code on demand (optional)**: REST can download scripts or applets which have precoded functionalities, reducing the number of features which need to be pre-implemented.
# Resource Methods / Rest Verbs

1. **POST** is used to create new resource (nested or not), it is not idempotent. Should return HTTP 201 with link to created resource in the location header.
2. **GET** is used only to read data, not used to perform modifications. Idempotent. Should return some data either json, xml etc etc with status code 200 for success, 404 for not found, or 400 if request was not performed the right way
3. **HEAD**. Like GET but no message body, only metadata
4. **PUT** is used for update, when updating the entirety of a resource. Not safe, but idempotent. Can be used to create resources if ID is selected by the front-end. return 200 on success update (or 204 when having no content body), 201 on success create (no need to set location header because the client created the resource). 409 if there is a conflict when updating (may have a versioning of resources). May not be idempotent if performs some sort of increment, in that case is recommended to use POST.
5. **PATCH** is used for partial updates. Usually not be idempotent. it may be helpful to use some kind of conditional request when performing PATCH so that it fails for some use cases.
6. **DELETE** Idempotent (end-result is the same, resource is deleted even if second time 404 is returned). return 200 with delete resource data, or some returned data, or 204 for no response.

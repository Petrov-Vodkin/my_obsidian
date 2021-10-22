2021-06-21 16:52
#tag
# Swagger

### Swagger Editor
[running from Docker](https://github.com/swagger-api/swagger-editor#docker)
```powershell
docker pull swaggerapi/swagger-editor
docker run -d -p 80:8080 swaggerapi/swagger-editor
```
Создавайте, описывайте и документируйте свой API в первом [редакторе](https://swagger.io/tools/swagger-editor/)
```bash
openapi: 3.0.2
info:
  title: TestSwag
  version: "1.0.0"
paths:
  /users:
    get:
      description: Get all users
      responses:
        '200':
          description: Описание ответа(получаем массив юзеров)
          content:
            application/json:
              schema:
                type: array
                items:
                  type: string
```

### Swagge UI
отображение документации

### Swagger Codegen
генерация исходного кода




rest API => OpenApiSpecification => Client
Swagger Codegen

OpenAPI-generator(fork Swagger Codegen)
OpenAPI-generator-maven-plugin генерация клиентов
_____________
#### Links

https://github.com/OpenAPITools
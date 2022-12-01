# Solución prueba wookie books - Fetcher



- [Ejercicio](#ejercicio)
  - [Implementación y tecnologías usadas](#Implementacion-y-tecnologias-usadas)
  - [Comentarios relevantes](#comentarios)
- [Setup](#setup)
  - [Instrucciones](#instrucciones)
  - [API Url](#API-Url)
  - [Servicios](#servicios)
- [Test](#test)
  - [Automaticos](#automaticos)
- [Inconvenientes](#Inconvenientes)
- [Mejoras](#Mejoras)

## Ejercicio

### Implementacion y tecnologias usadas

- Java 11
- H2
- JUnit
- Swagger-ui-express
- Maven
- GitHub
- Eureka
- JWT
- Api Gateway (Spring)
- Security (Spring)

### Comentarios

La solución propuesta se desarrolló en Java, ya que es un lenguaje en el que el desarrollador tiene experiencia, esto le permitió optimizar el tiempo para llegar a una solución exitosa del problema.

Al igual que java, la demás herramientas utilizadas se seleccionaron por diferentes características para llegar a la mejor solución posible. Esto complementado con ser herramientas ya utilizadas por el programador.

Se desarrolló bajo una arquitectura de microservicios, con un orquestador de servicios implementado con Eureka y un Api Gateway implementado con Spring, además de un sservidor propio de seguridad para la autenticación del usuario, que provee los tokens y refresh tokens necesarios para usar los micro servicios de users y books.

En la base de datos se utilizó H2 se escogió por ser una base de datos en memoria que el mismo proyecto correría, ya que inicialmente se desarrolló la solución usando MySQL pero al intentar realizar la dockerización, se presentaron problemas que obligaron al cambio de base de datos.

En la parte de los test unitarios se seleccionó Junit, que permite un amplio testeo de la aplicación e incluye Mockito lo que habilita las ventajas de mockear diferentes clases, métodos o interacciones.

Finalmente, en la parte de documentación se utilizó Swagger que es una herramienta de documentación muy sencilla de implementar y de usar y entender para el usuario final, esta se encuentra disponible en las siguientes direcciones:

- Users Microservice: http://localhost:8084/swagger-ui.html
- Books Microservice: http://localhost:8085/swagger-ui.html

## Setup

### Instrucciones

#### Prerequisitos

Para correr la aplicación en su máquina local es necesario tener instalado Java 11 y Maven.

#### Descarga

El proyecto se puede obtener desde github en la dirección otorgada por Fetcher.

Sin embargo para cada proyecto se crearon repositorios independientes para hacerlo más mantenible e independiente. En ellos se puede apreciar el histórico de los commits, para ver paso a paso como se fue dando la solución.

A continuación la lista de repositorios:

- Api gateway: https://github.com/CamiloHernandezC/Fetcher_ApiGateway
- Books microservice: https://github.com/CamiloHernandezC/Fetcher_BooksMicroservice
- Database: https://github.com/CamiloHernandezC/Fetcher_Database
- Security: https://github.com/CamiloHernandezC/Fetcher_Security
- Service register: https://github.com/CamiloHernandezC/Fetcher_ServiceRegister
- User microservice: https://github.com/CamiloHernandezC/Fetcher_UserMicroservice

#### Dependencias

La administración de dependencias se hizo utilizando maven. Por lo cual puede abrir una terminal, ir al folder del proyecto y utilizar el comando:

```
mvn clean install -U
```

#### Iniciación

Para iniciar la aplicación en la terminal puede utilizar el comando:

```
mvn  spring-boot:run
```

El orden sugerido para iniciar las aplicaciones es:

Database, este crea la base de datos en memoria, por lo cual los módulos de Security, User microservice y Books microservice lo necesitaran para iniciar correctamente sus conexiones JDBC.

Service Register, este crea el servicio de Eureka, por lo cual se sugiere iniciarlo antes de los siguiente módulos, para que puedan registrarse en este orquestador.

Luego de iniciar estos dos módulos podrá iniciar Api gateway, Security, User microservice y Books microservice indistintamente del orden.

Si todo está bien en el puerto 8080 (puerto utilizado por defecto) podrá realizar llamadas a los diferentes enpoints. 

Para comenzar puede utilizar hacer una petición POST al login, para obtener los tokens, con las siguientes credenciales:

- username: camilo
- password: password

Nota:

También se incluye una pequeña colección en Postman (que se encuentra en ./doc/Fetcher.postman_collection.JSON) para que pruebe algunos de los endpoints.

### API-Url

Como se mencionó anteriormente la dirección local (del API gateway) del proyecto se encuentra en:

http://localhost:8080/


### Servicios

Además de los servicios CRUD para los usuarios se tiene un endpoint para ver los libros publicados, en el cual podrá utilizar query params para filtrar los resultados por título, descripción, nombre del autor y precio.

### Test

#### Automaticos

Para los test se utilizó Junit y se trató de tener la mayor covertura posible, enfocandose en los controladores y la implementación de las interfaces utilizadas.

Esta es la covertura para el Users microservice:

![Coverage Image](doc/images/fetcher_test_coverage.PNG)

Esta es la covertura para el Books microservice:

![Coverage Image](doc/images/fetcher_test_coverage_books.PNG)

### Inconvenientes

- Como se mencionó anteriormente la dockerización no se pudo completar especialmente por problemas con la base de datos, por lo cual se cambio la aproximación para utilizar una base de datos en memoria (H2)


### Mejoras

- Tests de carga y de seguridad
- Dockerización
- Prevenir ataques tipo brutal force implementando el método unsuccessfulAuthentication en la clase CustomAuthenticationFilter
- Crear endpoint de health check
- Implementar la paginación para la busqueda especialmente de libros

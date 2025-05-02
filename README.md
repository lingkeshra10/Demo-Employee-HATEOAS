# Demo-Employee-HATEOAS

## What is HATEOAS?

HATEOAS stands for Hypermedia As The Engine Of Application State. It is a REST API constraint and part of the REST architectural style defined by Roy Fielding. HATEOAS enhances the discoverability and navigation of a REST API by including hypermedia links in the responses, allowing clients to understand what actions are possible next—without needing hardcoded knowledge of API routes.

✅ Real-World Analogy
Imagine you’re navigating a website. Each page gives you links to where you can go next—“Edit Profile”, “Delete Account”, or “Back to Home”. You don’t need to memorize or guess URLs because they’re provided. HATEOAS works similarly for REST clients.

🧩 HATEOAS in Your Spring Data REST App
You are already supporting HATEOAS in your application, without doing anything extra, because you're using Spring Data REST.

Let’s break it down using your setup:

🏗️ 1. Employee Entity
```
@Entity
@Table(name="employee")
public class Employee { ... }
```

This defines a JPA entity that maps to a table named employee. This is the data you want to expose via REST.

📦 2. EmployeeRepo
```
@RepositoryRestResource(path = "members")
public interface EmployeeRepo extends JpaRepository<Employee, Integer> { }
```

This is the core of HATEOAS support:
- Spring Data REST automatically generates REST endpoints (e.g., /magic-api/members) for this repository.
- It also adds hypermedia links in the JSON response body. These links let clients discover available actions (like GET, PUT, DELETE) dynamically.

Example Response from GET /magic-api/members/1:
```
{
  "id": 1,
  "firstName": "Lingkesh",
  "lastName": "Rajendram",
  "email": "ling@example.com",
  "_links": {
    "self": {
      "href": "http://localhost:8080/magic-api/members/1"
    },
    "members": {
      "href": "http://localhost:8080/magic-api/members"
    }
  }
}
```

The _links section is HATEOAS in action — it tells the client:
- Here's the direct link to this resource (self)
- Here's where to find more of this kind (members)

⚙️ 3. application.properties
```
spring.data.rest.base-path=/magic-api
spring.data.rest.default-page-size=20
```

This changes the base path of all REST endpoints from / to /magic-api. Your list endpoint becomes:
```
GET /magic-api/members
```
And Spring Data REST paginates responses with 20 items per page by default—again, automatically including HATEOAS links for navigation.

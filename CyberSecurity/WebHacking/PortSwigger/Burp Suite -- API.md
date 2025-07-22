
# API Recon

you should identify API endpoints. These are locations where an API receives requests about a specific resource on its server. For example, consider the following `GET` request:

`GET /api/books HTTP/1.1 Host: example.com`

The API endpoint for this request is `/api/books`. This results in an interaction with the API to retrieve a list of books from a library. Another API endpoint might be, for example, `/api/books/mystery`, which would retrieve a list of mystery books.

Once you have identified the endpoints, you need to determine how to interact with them. This enables you to construct valid HTTP requests to test the API. For example, you should find out information about the following:

- The input data the API processes, including both compulsory and optional parameters.
- The types of requests the API accepts, including supported HTTP methods and media formats.
- Rate limits and authentication mechanisms.

**Finding Hidden Parameters**

When you're doing API recon, you may find undocumented parameters that the API supports. You can attempt to use these to change the application's behavior. Burp includes numerous tools that can help you identify hidden parameters:

- Burp Intruder enables you to automatically discover hidden parameters, using a wordlist of common parameter names to replace existing parameters or add new parameters. Make sure you also include names that are relevant to the application, based on your initial recon.
- The Param miner BApp enables you to automatically guess up to 65,536 param names per request. Param miner automatically guesses names that are relevant to the application, based on information taken from the scope.
- The Content discovery tool enables you to discover content that isn't linked from visible content that you can browse to, including parameters.
  


# API Documentation

-> usually in format of **JSON** or **Xml**
-> also find it online 
-> if not found try looking **manually** like
Look for endpoints that may refer to API documentation, for example:

- `/api`
- `/swagger/index.html`
- `/openapi.json`
You can also use a list of common paths to find documentation using Intruder.

-> **Alternatives such as :**  
You can use a range of automated tools to analyze any machine-readable API documentation that you find.

You can use Burp Scanner to crawl and audit OpenAPI documentation, or any other documentation in JSON or YAML format. You can also parse OpenAPI documentation using the OpenAPI Parser BApp.

You may also be able to use a specialized tool to test the documented endpoints, such as Postman or SoapUI.

# Identifying and interacting with API endpoints


**Burp Scanner** automatically extracts some endpoints during crawls, but for a more heavyweight extraction, use the **JS Link Finder BApp**. You can also manually review JavaScript files in Burp.

Once api endpoints identified we use Repeater or Intruder to observe its behaviour.....

- `GET` - Retrieves data from a resource.
- `PATCH` - Applies partial changes to a resource.
- `OPTIONS` - Retrieves information on the types of request methods that can be used on  a resource.

try using  **/api/**

:FabAdn:
**Note**:

``You can use the built-in HTTP verbs list in Burp Intruder to automatically cycle through a range of methods .                                           When testing different HTTP methods, target low-priority objects. This helps make sure that you avoid unintended consequences, for example altering critical items or creating excessive records.``

**Try changing Content Type 
A api can be vulnerable to various diff types of content
ex if api is secure with json it can be vuln with xml file type try changing it

To change the content type, modify the `Content-Type` header, then reformat the request body accordingly. You can use the Content type converter BApp to automatically convert data submitted within requests between XML and JSON.


**## Lab: Finding and exploiting an unused API endpoint**

1. In Burp's browser, access the lab and click on a product.

2. In **Proxy > HTTP history**, notice the API request for the product. For example, `/api/products/3/price`.

3. Right-click the API request and select **Send to Repeater**.

4. In the **Repeater** tab, change the HTTP method for the API request from `GET` to `OPTIONS`, then send the request. Notice that the response specifies that the `GET` and `PATCH` methods are allowed.

5. Change the method for the API request from `GET` to `PATCH`, then send the request. Notice that you receive an `Unauthorized` message. This may indicate that you need to be authenticated to update the order.

6. In Burp's browser, log in to the application using the credentials `wiener:peter`.

7. Click on the **Lightweight "l33t" Leather Jacket** product.

8. In **Proxy > HTTP history**, right-click the `API/products/1/price` request for the leather jacket and select **Send to Repeater**.

9. In the **Repeater** tab, change the method for the API request from `GET` to `PATCH`, then send the request. Notice that this causes an error due to an incorrect `Content-Type`. The error message specifies that the `Content-Type` should be `application/json`.

10. Add a `Content-Type:application/json` .

11. Add an empty JSON object `{price}` as the request body, then send the request. Notice that this causes an error due to the request body missing a `price` parameter. //`{"price":0}`

12. Add a `price` parameter with a value of `0` to the JSON object `{"price":0}`. Send the request.

13. In Burp's browser, reload the leather jacket product page. Notice that the price of the leather jacket is now `$0.00`.

14. Add the leather jacket to your basket.

15. Go to your basket and click **Place order** to solve the lab.

->Using Intruder
*Once you have identified some initial API endpoints, you can use Intruder to uncover hidden endpoints. For example, consider a scenario where you have identified the following API endpoint for updating user information:*

*`PUT /api/user/update`*

*To identify hidden endpoints, you could use Burp Intruder to find other resources with the same structure. For example, you could add a payload to the `/update` position of the path with a list of other common functions, such as `delete` and `add`.*

*When looking for hidden endpoints, use wordlists based on common API naming conventions and industry terms. Make sure you also include terms that are relevant to the application, based on your initial recon.*      



# Finding Hidden Parameters 
refer api recon last part
# Mass assignment vulnerabilities

Mass assignment (also known as auto-binding) can inadvertently create hidden parameters
It occurs when software frameworks automatically bind request parameters to fields on an internal object.


LAB

![[Pasted image 20250502153013.png]]

![[Pasted image 20250502153030.png]]



# Preventing vulnerabilities in APIs

When designing APIs, make sure that security is a consideration from the beginning. In particular, make sure that you:

- Secure your documentation if you don't intend your API to be publicly accessible.
- Ensure your documentation is kept up to date so that legitimate testers have full visibility of the API's attack surface.
- Apply an allowlist of permitted HTTP methods.
- Validate that the content type is expected for each request or response.
- Use generic error messages to avoid giving away information that may be useful for an attacker.
- Use protective measures on all versions of your API, not just the current production version.

To prevent mass assignment vulnerabilities, allowlist the properties that can be updated by the user, and blocklist sensitive properties that shouldn't be updated by the user.

# Server-side parameter pollution or HTTP parameter pollution

**Server-Side Parameter Pollution** happens when:

- A web server takes **user input** (like query parameters or form fields),
    
- And **forwards it to an internal API or service**, often within the same network (not directly accessible to the internet),
    
- **Without properly validating or sanitizing that input**.
    

This can allow an attacker to **inject extra parameters** or **modify existing ones**, which could alter how the internal system behaves.

### Testing for server-side parameter pollution in the query string
To test for server-side parameter pollution in the query string, place query syntax characters like **`#`, `&`, and `=`** in your input and observe how the application responds.
`GET /userSearch?name=peter&back=/home`

To retrieve user information, the server queries an internal API with the following request:

`GET /users/search?name=peter&publicProfile=true`




### Truncating the response
You're looking at a technique attackers may use to exploit **server-side parameter pollution or request manipulation** by **truncating query strings** using a **URL-encoded `#` character**. Let's break it down step by step:

---

#### 🔧 **What is the Goal Here?**

To **manipulate the internal request** made by the server, and ideally **cut off (truncate)** part of the query string so that **some parameters are ignored** by the internal API.

---

#### 🧪 **Key Concept: Truncating with `#`**

- In a **URL**, anything after a `#` is considered a **fragment identifier** — it's **never sent to the server**.
    
- BUT: if you URL-encode the `#` as `%23`, it **is sent** as part of the query string to the server.
    
- The idea is that **some internal systems might mistakenly treat `%23` as a real `#`**, and truncate everything after it.
    

---

#### 🔍 **Example Breakdown**

You craft this URL:

```
GET /userSearch?name=peter%23foo&back=/home
```

#### On the Front-End:

This gets interpreted by the browser (correctly), and it sends the request **including** the `%23`:

```
GET /userSearch?name=peter%23foo&back=/home
```

#### Internal Request:

If the server **blindly forwards** parameters to an internal API like this:

```
GET /users/search?name=peter#foo&publicProfile=true
```

Then **everything after the `#` is ignored** by the internal system — the request becomes:

```
GET /users/search?name=peter
```

So now:

- `publicProfile=true` is **not sent** to the internal API.
    
- The server may respond with **non-public user data** (which should have only been allowed if `publicProfile=true` was present).
    

---

#### 🧠 **Why This Works (Sometimes)**

- Many systems aren’t designed with careful encoding rules when passing URLs internally.
    
- If the internal server or proxy treats `%23` as `#`, it stops parsing at that point, thinking it’s a fragment.
    
- The attacker’s goal is to **bypass access control or filter logic** that exists **after** the truncation point.
    

---

#### ✅ **How to Detect It**

When you try:

```
/userSearch?name=peter%23foo&back=/home
```

Check the response:

- If the response only includes **user `peter`**, and the `publicProfile=true` logic is missing, it suggests the **truncation worked**.
    
- If you get an error like **"Invalid name"**, and it says `peterfoo`, it means the server **didn’t truncate**, and treated `%23` as part of the string.
    

---
###  Injecting invalid parameters
You can use an URL-encoded `&` character to attempt to add a second parameter to the server-side request.

For example, you could modify the query string to the following:

`GET /userSearch?name=peter%26foo=xyz&back=/home`

This results in the following server-side request to the internal API:

`GET /users/search?name=peter&foo=xyz&publicProfile=true`


and

You're now looking at a classic technique to **test for Server-Side Parameter Pollution (SSPP)** by **injecting duplicate parameters with the same name** — in this case, to try to **override or manipulate a key parameter** used by the backend.

---

### 🔍 **Duplicate Parameter Injection**

You're trying a URL like:

```
GET /userSearch?name=peter%26name=carlos&back=/home
```

Where:

- `%26` is a URL-encoded `&`, so this becomes:
    
    ```
    name=peter&name=carlos
    ```
    

This would build a server-side request like:

```
GET /users/search?name=peter&name=carlos&publicProfile=true
```

You now have **two `name` parameters**, and how the backend interprets this **depends on the language/framework**.

---

#### 🧠 **How Different Tech Stacks Handle This**

| Framework             | Behavior                          | Result                                              |
| --------------------- | --------------------------------- | --------------------------------------------------- |
| **PHP**               | Uses the **last** occurrence      | `name = carlos`                                     |
| **Node.js / Express** | Uses the **first** occurrence     | `name = peter`                                      |
| **ASP.NET**           | Combines values (comma-separated) | `name = peter,carlos`                               |
| **Python (Flask)**    | Often stores all values in a list | Depends on implementation (`name[0]` or `name[-1]`) |

---

#### 🧨 **Exploitation Potential**

If you can **override or influence the original parameter**, then:

- You might **impersonate another user** by injecting `name=administrator`
    
- You could **bypass filters**, such as a hardcoded check for `publicProfile=true`
    
- You might **interfere with authorization**, fetching private data, etc.
    

---

#### 🧪 **Testing Strategy**

1. **Send the request** with two identical parameters:
    
    ```
    GET /userSearch?name=guest%26name=admin
    ```
    
2. **Observe the response**:
    
    - If you see results for `admin`, the last value was used.
        
    - If you see results for `guest`, the first was used.
        
    - If you get an error like “Invalid username,” the app might have combined both values.
        
3. **Repeat** with other sensitive values, like:
    
    - `name=administrator`
        
    - `role=admin`
        
    - `isAdmin=true`
        

---

**LAB**
![[Pasted image 20250504005938.png]]



## Testing for server-side parameter pollution in REST paths

RESTful API uses URL paths to identify resources, such as `/api/users/123` for a specific user. In some applications, user input is used to construct these paths. For example, a request like `GET /edit_profile.php?name=peter` may result in a back-end request to `/api/private/users/peter`. An attacker can exploit this by submitting a path traversal payload like `peter/../admin`, which may be URL-decoded and normalized to 
``GET /edit_profile.php?name=peter%2f..%2fadmin``
`/api/private/users/admin`, potentially granting unauthorized access to another user's data.

## Testing for server-side parameter pollution in structured data formats

Consider an application that enables users to edit their profile, then applies their changes with a request to a server-side API. When you edit your name, your browser makes the following request:

`POST /myaccount name=peter`

This results in the following server-side request:

`PATCH /users/7312/update {"name":"peter"}`

You can attempt to add the `access_level` parameter to the request as follows:

`POST /myaccount name=peter","access_level":"administrator`

If the user input is added to the server-side JSON data without adequate validation or sanitization, this results in the following server-side request:

`PATCH /users/7312/update {name="peter","access_level":"administrator"}`

This may result in the user `peter` being given administrator access.


You can attempt to add the `access_level` parameter to the request as follows:

`POST /myaccount {"name": "peter\",\"access_level\":\"administrator"}`

If the user input is decoded, then added to the server-side JSON data without adequate encoding, this results in the following server-side request:

`PATCH /users/7312/update {"name":"peter","access_level":"administrator"}`


## Testing with automated tools

Burp includes automated tools that can help you detect server-side parameter pollution vulnerabilities.

You can also use the Backslash Powered Scanner BApp to identify server-side injection vulnerabilities. The scanner classifies inputs as boring, interesting, or vulnerable. You'll need to investigate interesting inputs using the manual techniques outlined above. For more information, see the Backslash Powered Scanning: hunting unknown vulnerability classes whitepaper.

## Preventing server-side parameter pollution

To prevent server-side parameter pollution, use an allowlist to define characters that don't need encoding, and make sure all other user input is encoded before it's included in a server-side request. You should also make sure that all input adheres to the expected format and structure.
# Nginx Basic Concepts

## Project Setup:
* Replace the `<PROJECT_PATH>` in `nginx.conf` file with the absolute path of this project 
* Place this `nginx.conf` file in the nginx root dirctory (path where nginx is installed)
* Access the application : `http://localhost:8088`

### Static content serving
* All Mime types e.g. text/css, text/html etc are mapped in `mime.types` file in nginx root directory.
* To support different mime types in the application, `include mime.types;` is included in `nginx.conf`. 

### Location Context
* Multiple locations/paths can be mapped in the application using `location` directive in `nginx.conf`.
* Examples:
```
location /bikes {
    alias   <PROJECT_PATH>/server/bikes;
    index  bikes-index.html;
}

location /cars {
    alias   <PROJECT_PATH>/server/cars;
    index  cars-index.html;
}
```
* Access `http://localhost:8088/cars` for car page and `http://localhost:8088/bikes` for bike page.
* Paths/Locations with regular expressions can also be configured as the location. See example `http://localhost:8088/result/2/`

#### `root` vs `alias`
* In the `location` directive, we can configure the directory path under `root` or `alias`.
* `root`:
    * Example:
        ```
        location /bikes {
            root   <PROJECT_PATH>/server;
        }
        ```
    * The server root path `<PROJECT_PATH>/server` is configured as value and the location path is configured as `/bikes`. 
    * The absolute path is automatically constructed by `server root path`+`location path` e.g. `<PROJECT_PATH>/server/bikes`.
    * This should be used when the directory name is same as the location/path name.
* `alias`:
    * Example:
        ```
        location /bikes {
            alias   <PROJECT_PATH>/server/bikes;
        }
        ```
    * The absolute path is directly configured as value. 
    * This is flexible as the directory name does not depend on the location/path name.

### Redirect and Rewrite 
Check the `nginx.conf` file to see how redirect and rewrite are implemented.

#### 1. Redirect:
* Redirect will simply redirect the current endpoint to a new endpoint with client/browser knowing about it. The browser path/URL is also changed to the new endpoint.
* Example: `http://localhost:8088/city-bikes` is redirected to `http://localhost:8088/bikes` to show the bike page.

#### 2. Rewrite:
* Rewrite will redirect the current endpoint to a new endpoint internally without client/browser knowing about it. The browser path/URL is not changed to the new endpoint.
* Example: `http://localhost:8088/mountain-bikes` is redirected internally to `http://localhost:8088/bikes` to show the bike page but the browser url still points to the same `http://localhost:8088/mountain-bikes`.
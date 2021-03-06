# Backend Test
Design and implement a RESTful API (including data model and the backing implementation) for
money transfers between accounts.
### 1. How to start? ###
1. In project directory run `gradlew clean build` on Windows or `./gradlew clean build` on UN*X to compile, run tests and package the application.
2. Go to `build/libs` and run the application via `java -jar transfer-service.jar`. It will start server on 8080 port (if you want to change the port, you should run application via `java -Dhttp.port=other_port_number -jar transfer-service.jar`, where `other_port_number` - number of http port which you prefer).

### 2. Implementation details ###
#### Third-party libraries ####
1. [Javalin](https://javalin.io/) - A simple web framework
2. [H2](http://www.h2database.com/html/main.html) - Java SQL database
3. [Flyway](https://flywaydb.org) - A database migration tool
4. [Guice](https://github.com/google/guice) - DI framework
5. [Lombok](https://projectlombok.org) - An annotation-based code generator
#### Problems ####
1. H2 is used as data storage. This causes a file system to become clogged with database stuff (doesn't take up much space, because when the application starts and stops, flyway is clearing the data).
2. The application supports transactions only on DAO-level.

### 3. API ###
All requests to the API are relative ```http://localhost:port```
#### Endpoints ####
<table>
    <thead>
        <tr>
            <th>Endpoint</th>
            <th>Description</th>
            <th>Parameters</th>
            <th>Success Response/Status Code</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>POST</code><br/><code>/account/:balance</code></td>
            <td>Creates new account with specified balance</td>
            <td><code>:balance</code> - balance on account</td>
            <td><pre>
{
    "id": id,
    "balance": balance
}</pre><br/>201 CREATED
        <tr>
            <td><code>GET</code><br/><code>/account/:id</code></td>
            <td>Returns account by id</td>
            <td><code>:id</code> - account's id</td>
            <td><pre>
{
    "id": id,
    "balance": balance
}</pre><br/>200 OK
	    </td>
        </tr>
        <tr>
	    <td><code>DELETE</code><br/><code>/account/:id</code></td>
            <td>Deletes account by id</td>
            <td><code>:id</code> - account's id</td>
	    <td>204 NO CONTENT</td>
        </tr>
        <tr>
            <td><code>POST</code><br/><code>/account/:fromId/transfer/:toId/:amount</code></td>
            <td>Transfers specified amount of money from account to another account</td>
            <td>
            	<br/><code>:fromId</code> - account's id that balance will be reduced
                <br/><code>:toId</code> - account's id that balance will be increased
                <br/><code>:amount</code> - amount of money which will be transferred
            </td>
	    <td>204 NO CONTENT</td>
        </tr>
    </tbody>
</table>

#### Possible Errors ####
<table>
	<thead>
    	<tr>
        	<th>Status Code</th>
            <th>Error</th>
        <tr>
    </thead>
    <tbody>
    	<tr>
        	<td>404 NOT FOUND</td>
        	<td>Account was not found.</td>
        </>
    	<tr>
        	<td>400 BAD REQUEST</td>
        	<td>The balance value must not be negative.</td>
        </>
    	<tr>
        	<td>400 BAD REQUEST</td>
        	<td>The transfer of money to the same account is not allowed.</td>
        </>
    	<tr>
        	<td>400 BAD REQUEST</td>
        	<td>Not enough money on balance.</td>
        </>
    	<tr>
        	<td>4**, 5**</td>
        	<td>Other unhandled errors.</td>
        </>
    </tbody>
</table>

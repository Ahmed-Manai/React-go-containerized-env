# React-go-containerized-env

## Minimal 3 tier web application 
- **React fontend:** Uses react qurey to load data from the two apis and display the result
- **Node JS and Golan APIs:** Both have `/` and `/ping` endpoints. `/` queries the Database for the current time, and `/ping` return ``
- **Postgres Database:** An empty PostgreSQL database with no tables or data.
Used to show how to set up conncectivity. The API applications execute `SELECT NOW() as now;` to determine the current time to return.

## Running the Application (Localy)
The `Makefile` contians the commands to start the applicatoin.

### Postgres
run postgress in a container :
`make run-postgres` will start postgres in a container and publish port 5432 from the container to your localhost.

### api-node
install dependencies `npm install` used dependency (node `v10.4.0`, npm `v9.2.0`)
run the api in development mode with nodemon for restarting the app when you make source code changes. `make run-api-node`

### api-golang
download and install dependencies `go mod dowload` (go `go1.19.1`)
buil and run the api `make run-api-golang`

### client-react 
install dependencies `npm install` 
run the react app in development mode `make run-clinet-react`
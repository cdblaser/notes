# Postgres Notes:
## How to set up express and react:

1. in project directory, create two folders - client and server
2. in server directory, type npm init to create package.json
3. in server directory's package.json, change "main" to "server.js" instead of "index.js" (personal preference)
4. create server.js file in server directory
5. in server directory, type npm install express pg cors (can also $npm install nodemon -D so server auto-refreshes)
   NOTE: cors: allows different domain applications to interact with each other, pg: connects db with server to run postgresql queries
6. in package.json, add scripts "start": "node server", and "dev": "nodemon server". can use $npm run dev to run dev script
7. in client directory, run $npx create-react-app . like usual. or however you wanna do it
8. in App.js, create functional component <App/>
9. in server.js, look at code to get a feel. note: have to have const app = require("express")(). use listen() as well
10. in client directory's package.json, add "proxy": "https://localhost:PORT" (make relative api requests and avoid any issues with cross-origin) (may not be necessary yet)
11. create empty database.sql file
12. install postgres. may need to add bin and lib to system PATH
13. in terminal, run $psql -U postgres (pw: password)
14. type $\l to see list of databases
15. type $\c {database name} to enter database
16. however, we don't have database yet. in database.sql file, type 'CREATE DATABASE pernstack;'
17. in terminal, copypaste $CREATE DATABASE pernstack; . type $\l and you should see it
18. look in database.sql file - he types more
19. pastes it back in terminal
20. type $\dt - lets you see table. can also type $SELECT \* FROM todo; to see table
21. makes db.js file. check contents
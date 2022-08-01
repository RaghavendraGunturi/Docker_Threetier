1docker network create todo-app
2docker run -d `
     --network todo-app --network-alias mysql `
     -v todo-mysql-data:/var/lib/mysql `
     -e MYSQL_ROOT_PASSWORD=secret `
     -e MYSQL_DATABASE=todos `
     mysql:5.7
3docker exec -it <mysql-container-id> mysql -u root -p
4SHOW DATABASES;
5ALTER USER 'root' IDENTIFIED WITH mysql_native_password BY 'secret';
6flush privileges;
7docker run -dp 3000:3000 `
   -w /app -v "$(pwd):/app" `
   --network todo-app `
   -e MYSQL_HOST=mysql `
   -e MYSQL_USER=root `
   -e MYSQL_PASSWORD=secret `
   -e MYSQL_DB=todos `
   node:12-alpine `
   sh -c "yarn install && yarn run dev"
8docker exec -it <mysql-container-id> mysql -p todos
9exit

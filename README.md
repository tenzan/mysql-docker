# Creating docker container for sample.sql in MySQL

1. Install docker for your OS.

2. `git clone https://github.com/tenzan/mysql-docker && cd mysql-docker`

3. Build a repository
`docker build -t mysql-repo`

4. Run `mysql-cont` by specifying a `root` password.
`docker run --name mysql-cont -e MYSQL_ROOT_PASSWORD=root -d mysql-repo`

5. Check if container `mysql-repo` is running with `docker ps`

6. Login into the `mysql-cont`: `docker exec -it mysql-cont bash`
 - This will mean you logged in another linux environment

7. Now you can login mysql by `mysql -uroot --proot` and confirm the existence of `catalog` database (as per `sample.sql`)

# Pushing to the DockerHub

8. Run `docker images` to check `IMAGE ID` for `mysql-repo`, which is `9a53308ac9dd` in my case.

9. `docker tag 9a53308ac9dd tenzan/mysql:20151214`
where `tenzan` is my user name in the `DockerHub` and `20151214` is a tag.

10. Login into the DockerHub `docker login`

11. Push to the DockerHub `docker push tenzan/mysql:20151214`

# Pulling from the DockerHub

12. Make sure you don't have same image you're going to pull from the DockerHub, as it won't pull identical images
You can check with `docker images`. If there's, you need to remove it by `docker rmi <image-id>` prior to pulling.

13. Pull and run container
`docker run --name mysql-cont -e MYSQL_ROOT_PASSWORD=root -d tenzan/mysql:20151214`

14. Create network with name `mysql-db`
`docker network create mysql-db`

15. Connect network `mysql-db` with container `mysql-cont`
`docker network connect mysql-db mysql-cont`

16. Login to a mysql shell
`docker run -it --net=mysql-db --rm tenzan/mysql:20151214 sh -c 'exec mysql -hmysql-cont -uroot -proot'`

# OFFICIAL REPOSITORY (based on Debian)
 - Dockerfile: https://goo.gl/1sWRXV
 - https://hub.docker.com/_/mysql/

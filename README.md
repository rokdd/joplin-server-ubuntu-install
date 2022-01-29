# Install joplin server on Ubuntu (20.04)

1. as root run `joplin-requirements.sh` to install the joplin requirements but please take this script with causion since it is not well tested in existing environments. Root privileges are needed packages are installed. You can run `sudo bash -x joplin-requirements.sh` to see what it's doing. 
    You could also create the requirements by yourself by:
    
    1. create user joplin with home `/home/joplin`
    2. install nodejs 16
    3. install packages `vim git build-essential python curl dirmngr apt-transport-https lsb-release ca-certificates`
    4. checkout your desired version from https://github.com/laurent22/joplin.git to /home/joplin
    
2. collect expected files and folders and build joplin by running `joplin-build.sh`.  This must run as the joplin user. That is you can run it with `sudo -u joplin bash -x joplin-build.sh` to see what it's doing.

3. create a new db `joplin` in postgres (running on the same or on another server) and adapt run.sh with your db credentials. You can use psql to do this, for example:

    ```sql
    sudo -u postgres psql
    CREATE USER joplin WITH PASSWORD 'your-secret-password-here';
    CREATE DATABASE joplin;
    ALTER DATABASE joplin OWNER TO joplin;
    GRANT ALL PRIVILEGES ON DATABASE joplin TO joplin;
    \q
    ```

4. create a new file `.joplinrc` containing the variables you want to overwrite from the `run.sh`'s defaults. e.g.
    ````
    POSTGRES_PASSWORD="your-secret-password-here"
    APP_BASE_URL="https://joplin.mydomain.org"
    ````
    
5. test run.sh

6. if run.sh works as expected you can use `joplin.service` to run joplin as a systemd service

8. in the nginx folder you find an configuration example how to access the joplin server from your reverse proxy

# Update

````
cd ~/joplin
git fetch --tags
````
then checkout the new version from the tag history e.g.
````
git checkout server-v2.6.9
````
then stop your joplin server and build latest version
````
cd ~/
~/joplin-build.sh
````

s. [Joplin Server pre-release is now available](https://discourse.joplinapp.org/t/joplin-server-pre-release-is-now-available/13605/176)

# <p style="text-align: center; font-family: 'Times New Roman', serif;"><font size="10">Crunchy-Postgres-Exporter</p>

</br>

**<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="6.5">1. Task requirement:</font>**

  <p style="text-align: left; font-family: 'Times New Roman', serif;">
  <font size="4"> To set up Postgres exporter which capture the slow query also.</font> 

<p style="text-align: left; font-family: 'Times New Roman', serif;"><font size="4">The crunchy-postgres-exporter container provides real time metrics about the PostgreSQL database via an API. These metrics are scraped and stored by a Prometheus time-series database and are then graphed and visualized through the open source data visualizer Grafana.</font>


**<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="6.5">2. Environment details:</font>**

* <p style="text-align: left; font-family: 'Times New Roman', serif;">
  <font size="4">Podman - Version 3 . </font></p>

**<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="6.5">3. List of tools and technologies: </font></p>**

* <p style="text-align: left; font-family: 'Times New Roman', serif;">
  <font size="4"> Podman Version-12 </font></p>


* <p style="text-align: left; font-family: 'Times New Roman', serif;">
  <font size="4"> Postgres version -12 </font></p>

  </br>

<div style="page-break-after: always;"></div>

**<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="6.5">a. Definition of Podman</font></p>**


<p style="text-align: left; font-family: 'Times New Roman', serif;">
  <font size="4">Podman (the POD manager) is an open source tool for developing, managing, and running containers on your Linux systems. Originally developed by Red Hat® engineers along with the open source community, Podman manages the entire container ecosystem using the libpod library.</font></p>

  </br>

**<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="6.5">b. Definition of PostgreSQL</font></p>**

<p style="text-align: left; font-family: 'Times New Roman', serif;">
  <font size="4">PostgreSQL, also known as Postgres, is a free and open-source relational database management system emphasizing extensibility and SQL compliance. It was originally named POSTGRES, referring to its origins as a successor to the Ingres database developed at the University of California, Berkeley.</font></p>

  </br>

**<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="6.5">4. Command for the setup or configuration: </font></p>**

  </br>

**<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="5">1. Create Pod: </font></p>** 

> <font size="4">podman pod create --name crunchy-postgres --publish 9090:9090 --publish 9187:9187 --publish 5432:5432 --publish 3000:3000 </font>


* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">podman pod create:**  This part of the command instructs Podman to create a new pod.</font>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">--name crunchy-postgres**: This option assigns the name "crunchy-postgres" to the newly created pod.</font>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">--publish 9090:9090**: This option maps port 9090 from the host to port 9090 within the pod. Any service running in the pod on port 9090 will be accessible from the host machine on port 9090.</font>

<div style="page-break-after: always;"></div>

 * **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">--publish 9187:9187** :  The 9187 option directs port 9187 from the host machine to port 9187 within the pod. This enables any service running on port 9187 within the pod to be accessed from the host machine using port 9187. This setup resembles the earlier port mapping options, where both the host port number and the pod's internal port number are set to 9187 to facilitate communication. between host and pod on the same port.</font>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">--publish 5432:5432**: This option maps port 5432 from the host to port 5432 within the pod. Port 5432 is used for PostgreSQL database connections.</font>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">--publish 3000:3000:** This option is used to map port 3000 from the host to port 3000 within the PostgreSQL container for Grafana service. This allows access to the Grafana web interface running on port 3000 within the container from the host machine using the same port number.</font> 

**<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="5">2. Create Postgres container:</font></p>** 

> <font size="4">podman run -d --pod crunchy-postgres --name postgres_crunchy -e "POSTGRES_DB=postgres" -e "POSTGRES_USER=postgres" -e "POSTGRES_PASSWORD=redhat" -v 
/home/yogendra/shiksha_portal/crunchy/postgres/data:/var/lib/postgresql/data docker.io/postgres:12</font>  

* **<p style="text-align: left; font-family: 'Times New Roman', serif;"> <font size="4">echo:** The echo command is used to display text or output to the terminal.</font>
* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">shared_preload_libraries = 'pg_stat_statements,auto_explain':** This is the text you want to append to the file. This is a configuration line that specifies the use of two PostgreSQL extensions: pg_stat_statements and auto_explain.</font>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">postgresql.conf:**  This part of the command uses the >> operator to append the text generated by the echo command to the postgresql.conf file. The postgresql.conf file is a configuration file for PostgreSQL that contains various settings and options for the database.</font>

<div style="page-break-after: always;"></div>

**<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="5">3. Do Changes in configuration file:</font></p>**

> <font size="4">echo "shared_preload_libraries = 'pg_stat_statements,auto_explain'" >> postgresql.conf</font>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4"> echo:** The echo command is used to display text or output to the terminal.</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4"> postgresql.conf:** This part of the command uses the >> operator to append the text generated by the echo command to the postgresql.conf file. The postgresql.conf file is a configuration file for PostgreSQL that contains various settings and options for the database.</font></p>

**<p style="text-align: left; font-family: 'Times New Roman', serif;"><font size="4">4. Pull crunchy-postgres-exporter image:</font></p>** 

> <font size="4">podman pull 
registry.connect.redhat.com/crunchydata/crunchy-postgres-exporter:latest</font> 

* **<p style="text-align: left; font-family: 'Times New Roman', serif;"><font size="4">podman pull:** This command is used to fetch or download a container image.</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;"><font size="4">registry.connect.redhat.com:** This is the hostname of the container registry. It indicates that the container image is hosted on the Red Hat container registry.</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;"><font size="4">crunchydata/crunchy-postgres-exporter:** This is the name of the image repository and the name of the image itself. It's structured as "repository/image-name." The image belongs to the "crunchydata" repository and is named "crunchy-postgres-exporter."</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;"><font size="4">latest:** This is the image tag. Tags are used to identify different versions or variations of a container image. The "latest" tag typically refers to the most recent version of the image.</font></p>

<div style="page-break-after: always;"></div>

**<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">5. Create a demo container of crunchy for setup.sql.</font></p>** 

> <font size="4">podman run -itd --pod crunchy-postgres --name crunchy -e 
EXPORTER_PG_PASSWORD=redhat 615904c619c5</font>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">podman run:**  This command is used to start a new container.</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">-itd:** These flags collectively specify that the container should be run in "interactive" mode (-i), attached to the terminal (-t), and in "detached" mode (-d), which means the container runs in the background.</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">--pod crunchy-postgres:** This option indicates that the container should be run within the existing pod named "crunchy-postgres."</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">--name crunchy:** This option assigns the name "crunchy" to the newly created container.</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">-e EXPORTER_PG_PASSWORD=redhat:** This environment variable (EXPORTER_PG_PASSWORD) is set to the value "redhat." It's configuring the password for the PostgreSQL Exporter.</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">615904c619c5:** This is the ID of the container image that the new container should be based on. It specifies which image to use as the template for the container.</font></p>

**<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">6. Get the setup.sql from /opt/cpm/conf/pgxx/setup.sql from the crunchy container according to your postgres version.</font></p>**

> <font size="4">podman cp crunchy:/opt/cpm/conf/pg12/setup.sql</font>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">podman cp:** This command is used to copy files or directories between a container and the host system.</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">crunchy:** This is the name of the container from which you want to copy the file.</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">/opt/cpm/conf/pg12/setup.sql:** This part specifies the path of the source file inside the container. The file you want to copy is named "setup.sql," and it's located in the directory "/opt/cpm/conf/pg12" inside the "crunchy" container.</font></p>

**<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">7. Remove the test container of crunchy:</font></p>** 

> <font size="4">podman rm -f crunchy </font>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">podman rm:** This command is used to remove one or more containers.

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">-f:** This flag stands for "force" and is used to forcefully stop and remove the container if it's currently running.

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">crunchy:** This is the name of the container that you want to remove.

**<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">8. Push setup.sql in postgres database:</font></p>** 

<font size="4">Get the setup.sql from /opt/cpm/conf/pg12/setup.sql from the exporter container.</font>

> <font size="4">psql -h 127.0.0.1 -U postgres -d template1 < setup.sql</font> 

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">psql:** This is the PostgreSQL command-line client used to interact with PostgreSQL databases.</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">-h 127.0.0.1:** This specifies the host to connect to. it's the local host with the IP address 127.0.0.1, indicating that the PostgreSQL database is running on the same machine.</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">-U postgres:** This specifies the username to use for the PostgreSQL connection. In this case, the username is "postgres."</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">-d template1:** This specifies the name of the database to connect to. it's the "template1" database, a default PostgreSQL system database often used for administrative tasks.</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;"> <font size="4">psql:** This is the PostgreSQL command-line client used to interact with PostgreSQL databases.</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">-h 127.0.0.1:** This specifies the host to connect to. it's the local host with the IP address 127.0.0.1, indicating that the PostgreSQL database is running on the same machine.</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">-U postgres:** This specifies the username to use for the PostgreSQL connection. In this case, the username is "postgres."</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">-d template1:** This specifies the name of the database to connect to. it's the "template1" database, a default PostgreSQL system database often used for administrative tasks.</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4"> setup.sql:** This part uses input redirection (<) to feed the contents of the "setup.sql" file as input to the psql command. The "setup.sql" file contains SQL commands that will be executed on the specified database.</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">sql:** This is the PostgreSQL command-line client used to interact with PostgreSQL databases.</font></p>



**<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="5">9. Create Extension:</font></p>** 

> <font size="4">psql -h 127.0.0.1 -U postgres -d template1 -c "CREATE EXTENSION pg_stat_statements;"</font>


* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">-h 127.0.0.1:** This specifies the host to connect to. it's the local host with the IP address 127.0.0.1, indicating that the PostgreSQL database is running on the same machine.</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">-U postgres:** This specifies the username to use for the PostgreSQL connection. In this case, the username is "postgres."</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">-d template1:** This specifies the name of the database to connect to. it's the "template1" database, a default PostgreSQL system database often used for administrative tasks.</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">-c:** This is used with the psql command to specify a single SQL command to be executed. It stands for "command" or "query."</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">CREATE EXTENSION:** This is a PostgreSQL SQL command used to create an extension in the current database. An extension is a module that provides additional functionality to the database.</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">pg_stat_statements:** This is the name of the PostgreSQL extension you're creating. The pg_stat_statements extension is used for collecting and analyzing query statistics for performance monitoring and tuning.</font></p>

**<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="5">10. Create password for user ccp_monitoring:</font></p>**

> <font size="4">postgres=# \password ccp_monitoring</font> 

> <font size="4">Enter new password for user "ccp_monitoring":</font> 

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">postgres=#:** This is the PostgreSQL command-line prompt, indicating that are interacting with the PostgreSQL database as the "postgres" user. The # symbol is the command prompt indicating that you can enter a command.</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">\password:** This meta-command is used to change a user's password.</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">ccp_monitoring:** This is the username for which you want to change the password.</font></p>

<div style="page-break-after: always;"></div>

<font size="4">Enter it again:</font>

> <font size="4">postgres=# create database yogendra;</font> 

> <font size="4">CREATE DATABASE </font>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">Enter it again:** his is a prompt that asks you to re-enter the same password for confirmation. This step is taken to ensure that the password is entered correctly without typos.</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">Create database yogendra:** This SQL command is used to create a new database named "yogendra."</font></p>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">CREATE DATABASE:** After executing the command, PostgreSQL responds with "CREATE DATABASE" to confirm that the database creation was successful.</font></p>

**<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="5">11. Now create crunchy-postgres-exporter container:</font></p>**

> <font size="4">podman run -itd --pod crunchy-postgres --name crunchy -e 
EXPORTER_PG_PASSWORD=redhat -e EXPORTER_PG_HOST=127.0.0.1 -e EXPORTER_PG_USER=ccp_monitoring -e 
DATA_SOURCE_NAME=postgresql://ccp_monitoring:redhat@127.0.0.1:5432/yo gendra?sslmode=disable 615904c619c5</font> 

**<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="5">12. Check metrics:</font></p>** 

> <font size="4">curl localhost:9187/metrics | grep query </font>
![Alt text](image-1.png)
* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">curl:** This is a command-line tool used to make HTTP requests to web servers.</font>
* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">localhost:9187/metrics:** This is the URL you're requesting data from. In this case, you're accessing the /metrics endpoint on the local machine at port 9187. This is typically where the Prometheus metrics are exposed by various applications, including PostgreSQL Exporter.</font>

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">( | ):** This is the pipe symbol, which is used to take the output from one command and use it as input for another command.

* **<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">grep query:** This command is used to search for lines containing the word "query." It filters the output of the curl command to only show lines that include the term "query."



**<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">13. Create prometheus container:<font></p>**

> <font size="4"> podman run -itd --pod crunchy-postgres --name prometheus_crunchy -v /home/yogendra/shiksha_portal/prometheus_crunchy/prometheus.yml:/etc/pro metheus/prometheus.yml docker.io/prom/prometheus</font>

> <font size="4">Set the target in prometheus.yml file to get metrics in prometheus and hit on browser: http://localhost:9090/</font>
![Alt text](webpreview_htm_4c71cad42acd9c89.png)

![Alt text](image.png)

![Alt text](webpreview_htm_1f7cafb3a385483a.png)

<div style="page-break-after: always;"></div>

**<p style="text-align: left; font-family: 'Times New Roman', serif;">  <font size="4">14. Create Grafana Container:</font></p>** 

> <font size="4">podman run -itd --pod crunchy-postgres --name grafana_crunchy
docker.io/grafana/grafana 


Hit on browser: 

> http://localhost:3000/ 
Select the prometheus as a datasource and import the dashboard.
9628

![Alt text](webpreview_htm_42122464ace378b3.png)
![Alt text](webpreview_htm_b09ef94be1f97924.png)


<p style="text-align: left; font-family: 'Times New Roman', serif;">
  <font size="4">Note : NA</font></p>


**<p style="text-align: left; font-family: 'Times New Roman', serif;">    <font size="5">Reference link</font></p>**

https://access.crunchydata.com/documentation/pgmonitor/2.2/exporter/index.html 


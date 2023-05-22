# Establishing the Grafana-mysql connection via Docker Containers

## Prerequisites

- Add a EC2 instance with the docker installed on it
-  `yum install docker`
-  `systemctl start docker`
-  `systemctl enable docker`

## Steps to be followed:

- 1: **Install Grafana**
   ```
   docker run -d --name=grafana -p 3001:3000 -v grafana_config:/etc/grafana -v grafana_data:/var/lib/grafana -v grafana_logs:/var/log/grafana grafana/grafana
   ```
   
   - Check the grafana Dashboard using the Browser
   ![Screenshot from 2023-05-22 12-36-52](https://github.com/adityadhopade/grafana-mysqldb/assets/48392204/7dc2b77a-930d-459a-b140-0b9dd53d6e4f)

    `
    Public IP of EC-2: Host Port Number(3306)
    ` 
    - Whne the Dashboard gets loaded ; then add the credentials using
    `
    Username: admin
    password: admin
    `
    
- 2: **Install mysql**

    ```
    docker run -d --name mysqldb -p 3306:3306 -v db_data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=password -d mysql:latest
    ```
    
    `using the MYSQL_ROOT_PASSWORD in env variable we are setting the password as "password" `
    
 - 3 Check if all containers are running in or not ?! and **Enter the mysql container**
 
    `docker container ls`
    `docker exec -it mysqldb bash`
    
    Enter all commands as **root**
    
    - After entering into the bash window of container 
     `mysql -u root -p password`
     
     
  - 4  Steps involved in the mysql
  
      - Create DB `create database team;`
      - Use team Database `use team;`
      - Create the table in team DB `create table person(person_id INT NOT NULL PRIMARY KEY, fname VARCHAR(40) NULL, lname VARCHAR(40) NULL, age INT NOT NULL, created TIMESTAMP);`
      - show tables `show tables;`
      - Then refer the scripts into the **scripts.sql** file linked above for adding the dump data
      - check for the content added in the table using `` select * from tables;
  
  
  - 5 **Addding the Datasource in the Grafana Dashboard**
    
      ![Screenshot from 2023-05-22 12-39-51](https://github.com/adityadhopade/grafana-mysqldb/assets/48392204/9dfe18b2-4c91-4526-b0a1-199e29cfb68d)
     - Grafana comes with built-in support for many data sources. If you need other data sources, you can also install one of the many data source plugins. If the plugin you need doesn’t exist, you can develop a custom plugin.

     - On the Dashboard page of grafana; click on the Add Data Source Tab --> Search for the mysql data source
     - Fill up the details in the respective fields as follows
     ```
     Host= EC-2 Public IP Address: Port on which the Mysql Hosted(3306)
     DB= team
     User= root
     pass = password (or whatever added in while creating the mysql container)
     ```
     ![Screenshot from 2023-05-22 14-18-23](https://github.com/adityadhopade/grafana-mysqldb/assets/48392204/ce016e79-f246-4440-a90d-1e639c0fbf47)

     
     - Click on save and test; if connection gets established it means we have successfully completed the integration with mysql container.
     - Go back to the Dashboard Page --> Click on the **Add a Panel**
     - Add Viusalization
     - Click on the Dropdown asking for Data Source --> Select mysql
     - Select the Code from the Builder | Code Buttons present below the recently selected Dropdwon
     - Check out the tbale contents in the code section so write `
      ```
      select * from table;
      ```
     
    
    



# Installation
```bash
https://github.com/Vlad-Misiukevich/m10_kafkabasics_sql_local.git
```
# Requirements
* Python 3.8
* Windows OS
* WSL2
* Docker version 1.11 or later
* Docker compose
# Description
1. Run WSL2
2. Change directory to m10_kafkabasics_sql_local:
```bash
cd m10_kafkabasics_sql_local/
```
3. Run command:
```bash
sudo sysctl -w vm.max_map_count=262144
```
![img.png](images/img.png)
4. Get the Jar files for kafka-connect-datagen and kafka-connect-elasticsearch:
```bash
docker run -v $PWD/confluent-hub-components:/share/confluent-hub-components confluentinc/ksqldb-server:0.8.0 confluent-hub install --no-prompt confluentinc/kafka-connect-datagen:0.4.0
```
![img_1.png](images/img_1.png)
```bash
docker run -v $PWD/confluent-hub-components:/share/confluent-hub-components confluentinc/ksqldb-server:0.8.0 confluent-hub install --no-prompt confluentinc/kafka-connect-elasticsearch:10.0.2
```
![img_2.png](images/img_2.png)
5. Run the tutorial in Docker:
```bash
docker-compose up -d
```
![img_3.png](images/img_3.png)
6. Run the status command to ensure that everything has started correctly:
```bash
docker-compose ps
```
![img_4.png](images/img_4.png)
7. Run the ksqlDB CLI:
```bash
docker-compose exec ksqldb-cli ksql http://ksqldb-server:8088
```
![img_5.png](images/img_5.png)
8. Ensure the ksqlDB server is ready to receive requests:
```bash
show topics;
```
![img_6.png](images/img_6.png)
9. Run the script:
```bash
RUN SCRIPT '/scripts/create-connectors.sql';
```
![img_7.png](images/img_7.png)
10. Sample the messages in the clickstream topic:
```bash
print clickstream limit 3;
```
![img_8.png](images/img_8.png)
11. Sample the messages in the clickstream_codes topic:
```bash
print clickstream_codes limit 3;
```
![img_9.png](images/img_9.png)
12. Sample the messages in the clickstream_users topic:
```bash
print clickstream_users limit 3;
```
![img_10.png](images/img_10.png)
13. Go to UI at http://localhost:9021 and view the three kafka-connect-datagen source connectors:
![img_11.png](images/img_11.png)
14. Load the file that runs the tutorial app.
```bash
RUN SCRIPT '/scripts/statements.sql';
```
![img_12.png](images/img_12.png)
15. Exit out of the ksqldb-cli with a CTRL+D command:
![img_13.png](images/img_13.png)
16. Go to UI and view the ksqlDB view Flow:
![img_14.png](images/img_14.png)
17. Verify that data is being streamed through various tables and streams. Query one of the streams CLICKSTREAM:
![img_15.png](images/img_15.png)
18. Set up the required Elasticsearch document mapping template:
```bash
docker-compose exec elasticsearch bash -c '/scripts/elastic-dynamic-template.sh'
```
![img_16.png](images/img_16.png)
19. Run this command to send the ksqlDB tables to Elasticsearch and Grafana:
```bash
docker-compose exec ksqldb-server bash -c '/scripts/ksql-tables-to-grafana.sh'
```
![img_17.png](images/img_17.png)
20. Load the dashboard into Grafana:
```bash
docker-compose exec grafana bash -c '/scripts/clickstream-analysis-dashboard.sh'
```
![img_18.png](images/img_18.png)
21. Grafana dashboard:
![img_19.png](images/img_19.png)
22. In the UI at http://localhost:9021 view the running connectors:
![img_20.png](images/img_20.png)
23. Generate the session data:
```bash
./sessionize-data.sh
```
![img_21.png](images/img_21.png)
24. Clickstream Analysis Dashboard:
![img_24.png](images/img_24.png)
### Metrics
1. General website analytics, such as hit count and visitors:
![img_22.png](images/img_22.png)
2. Mapping user-IP addresses to actual users and their location:
![img_23.png](images/img_23.png)
3. Error-code occurrence and enrichment:
![img_25.png](images/img_25.png)
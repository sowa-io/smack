1.Install mysql and create a db
        sudo apt-get update
        sudo apt-get install mysql-server
        mysql_secure_installation


        mysql -uroot -proot

        GRANT ALL PRIVILEGES ON *.* TO 'rmoff'@'localhost' IDENTIFIED BY 'pw';

        create database demo;

        use demo;


        create table foobar (c1 int, c2 varchar(255),create_ts timestamp DEFAULT CURRENT_TIMESTAMP , update_ts timestamp DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP );


        insert into foobar (c1,c2) values(1,'foo');



2. install java 1.8

        sudo add-apt-repository ppa:webupd8team/java
        sudo apt update
        sudo apt install oracle-java8-installer

        sudo apt install oracle-java8-set-default

        javac -version

3.Download Confluent and unzip
        wget https://packages.confluent.io/archive/5.0/confluent-oss-5.0.0-2.11.zip


4.download mysql driver
        cp mysql-connector-java-5.1.47-bin.jar ~/confluent-5.0.0/share/java/kafka-connect-jdbc

5.load mysql connector
        ./bin/confluent load jdbc_source_mysql_foobar_01 -d /kafka-connect-jdbc-source.json


6. check status
        ./bin/confluent status jdbc_source_mysql_foobar_01


7. Consumer a topic 
./bin/kafka-avro-console-consumer \
> --bootstrap-server localhost:9092 \
> --property schema.registry.url=http://localhost:8081 \
> --property print.key=true \
> --from-beginning \
> --topic mysql-foobar



8 . In another console 
  mysql -uroot -proot
  use demo;

  insert into foobar (c1,c2) values(2,'foo');
  insert into foobar (c1,c2) values(3,'foo');
  update foobar set c2='bar' where c1=1;

(console open in step 7 must show the topic with these new rows)






# Athena Federate Query

## Athena Jdbc Connector 
With Athena Jdbc Connector, the following databases are supported:
1. MySQL
2. PostgreSQL
3. Redshift

For detailed information about Amazon Athena Jdbc Connector, refer [here](https://github.com/awslabs/aws-athena-query-federation/tree/master/athena-jdbc).

---
In this sample, we will create a Aurora Serverless MySQL Database, with settings as follows: 
-   Master Username: awsuser
-   Master Password: awspassword
-   And then enable Data API
- ... and keep other setting as default.

> Copy the Security Group Id, Subnet Id, and endpoint for later use.

Using Query Editor to connect to Aurora Serverless MySQL Database; and then run SQL as follows:

1. Create database:
```CREATE DATABASE tpch;```

2. Create 4 table includes region, nation, customer, supplier
```
CREATE TABLE tpch.region (
  `r_regionkey` int(11) NOT NULL,
  `r_name` char(25) DEFAULT NULL,
  `r_comment` varchar(152) DEFAULT NULL,
  PRIMARY KEY (`r_regionkey`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE tpch.nation (
  `n_nationkey` int(11) NOT NULL,
  `n_name` char(25) DEFAULT NULL,
  `n_regionkey` int(11) DEFAULT NULL,
  `n_comment` varchar(152) DEFAULT NULL,
  PRIMARY KEY (`n_nationkey`),
  KEY `n_regionkey` (`n_regionkey`),
  CONSTRAINT `nation_ibfk_1` FOREIGN KEY (`n_regionkey`) REFERENCES `region` (`r_regionkey`) ON DELETE CASCADE ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE tpch.customer (
  `c_custkey` bigint(20) NOT NULL,
  `c_mktsegment` char(10) DEFAULT NULL,
  `c_nationkey` int(11) DEFAULT NULL,
  `c_name` varchar(25) DEFAULT NULL,
  `c_address` varchar(40) DEFAULT NULL,
  `c_phone` char(15) DEFAULT NULL,
  `c_acctbal` decimal(19,4) DEFAULT NULL,
  `c_comment` varchar(118) DEFAULT NULL,
  PRIMARY KEY (`c_custkey`),
  KEY `c_nationkey` (`c_nationkey`),
  CONSTRAINT `customer_ibfk_1` FOREIGN KEY (`c_nationkey`) REFERENCES `nation` (`n_nationkey`) ON DELETE CASCADE ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE tpch.supplier (
  `s_suppkey` int(11) NOT NULL,
  `s_nationkey` int(11) DEFAULT NULL,
  `s_comment` varchar(102) DEFAULT NULL,
  `s_name` char(25) DEFAULT NULL,
  `s_address` varchar(40) DEFAULT NULL,
  `s_phone` char(15) DEFAULT NULL,
  `s_acctbal` decimal(19,4) DEFAULT NULL,
  PRIMARY KEY (`s_suppkey`),
  KEY `supplier_ibfk_1` (`s_nationkey`),
  CONSTRAINT `supplier_ibfk_1` FOREIGN KEY (`s_nationkey`) REFERENCES `nation` (`n_nationkey`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

3. Insert serveral test data which generate from TPC-H dbgen
```
INSERT INTO tpch.region VALUES (0,"AFRICA","asymptotes sublate after the r"),
(1,"AMERICA","requests affix quickly final tithes. blithely even packages above the a"),
(2,"ASIA","accounts cajole carefully according to the carefully exp"),
(3,"EUROPE","slyly even theodolites are carefully ironic pinto beans. platelets above the unusual accounts aff"),
(4,"MIDDLE EAST","furiously express accounts wake sly");

INSERT INTO tpch.nation VALUES (0,"ALGERIA",0,"slyly express pinto beans cajole idly. deposits use blithely unusual packages? fluffily final accounts x-r"),
(1,"ARGENTINA",1,"instructions detect blithely stealthily pending packages"),
(2,"BRAZIL",1,"blithely unusual deposits are quickly--"),
(3,"CANADA",1,"carefully pending packages haggle blithely. blithely final pinto beans sleep quickly even accounts? depths aroun"),
(4,"EGYPT",0,"slyly express deposits haggle furiously. slyly final platelets nag c"),
(5,"ETHIOPIA",0,"quickly special pinto beans nag quickly blithely close instructions. furiously regular ideas against"),
(6,"FRANCE",3,"even idle instructions ha,ggle evenly carefully even theodolites. carefully unusual frays lose acros"),
(7,"GERMANY",3,"carefully regular dependencies was regular dependencies. even packages wa"),
(8,"INDIA",2,"slyly even theodolites maintain. even pending requests might cajole against th"),
(9,"INDONESIA",2,"enticingly silent platelets above the blithely sil"),
(10,"IRAN",4,"furiously ironic deposits sleep blithely against the furiously even pinto beans"),
(11,"IRAQ",4,"carefully furious packages are-- furiously ironic accounts caj"),
(12,"JAPAN",2,"furiously unusual attainments poach. pending accounts cajole. "),
(13,"JORDAN",4,"ironic dependencies wake carefully alongside of the realms? unusual req"),
(14,"KENYA",0,"carefully ironic dolphins should sleep slyly furiously special theod"),
(15,"MOROCCO",0,"even foxes wake quickly. furiously special ideas are above the special deposits"),
(16,"MOZAMBIQUE",0,"carefully pending notornis across the quickly even theodolites grow alongs"),
(17,"PERU",1,"slow deposits nag around the furiously final requests. quickly ironic excuses across the"),
(18,"CHINA",2,"quickly regular pinto beans boost slyly express decoys. furiously express excuses impress blithely regular foxes"),
(19,"ROMANIA",3,"slyly final pinto beans affix slyly regular even packages. carefully regular acco"),
(20,"SAUDI ARABIA",4,"blithely regular dolphins nag furiously above the slyly silent waters-- slyly fluffy pinto beans cajole bravely "),
(21,"VIETNAM",2,"quickly regular sauternes affix. blithely final excuses are blithely about the fluff"),
(22,"RUSSIA",3,"silent Tiresias thrash for the blithely"),
(23,"UNITED KINGDOM",3,"furiously regular requests are furiously special deposits. excuses boost silently express ideas. accounts according "),
(24,"UNITED STATES",1,"special asymptotes cajole carefully. slyly regular accounts according to the quickly i");

INSERT INTO tpch.customer VALUES (1,"BUILDING",8,"Customer#000000001","KwX3hMHjZ6","937-241-3198",3560.03,"ironic excuses detect slyly silent requests. requests according to the exc"),
(2,"MACHINERY",16,"Customer#000000002","ioUn,eqTTXOdo","906-965-7556",7550.21,"final express accounts mold slyly. ironic accounts cajole! quickly express a"),
(3,"FURNITURE",11,"Customer#000000003","YddJqmIdouNT9Yj","328-750-7603",-926.96,"carefully express foxes sleep carefully. pending platelets sleep thinly for t"),
(4,"FURNITURE",24,"Customer#000000004","iE7PADWuxr4pR5f9ewKqg","127-505-7633",-78.75,"silent packages sleep. even re"),
(5,"MACHINERY",4,"Customer#000000005","h3yhvBTVbF2IJPzTKLoUe4","322-864-6707",7741.9,"slyly special frays nag quietly bl"),
(6,"AUTOMOBILE",6,"Customer#000000006","Zhl0dFevhaCb0JBz5B3N519mB","906-987-4933",6016.22,"bold ideas cajole furiously furiously express accounts. deposits cajole furiously. furiously close accounts ar"),
(7,"BUILDING",1,"Customer#000000007","RoJOXUJTtJiPOeLOevu2XraKnK8gc","501-272-8293",4096.7,"pending theodolites above the ironic"),
(8,"MACHINERY",14,"Customer#000000008","eAtAkcVMh 7227UtcTi","457-420-9613",2888.93,"accounts boost closely. theodolites after the requests impress sometimes i"),
(9,"HOUSEHOLD",1,"Customer#000000009","zCEFPG5PMy9eliG4sif3wmn64VqrhqGOtOYiXgU","560-675-8060",9686.28,"furiously even Tiresias use across the boldly ironic sauternes-- carefully ironic pearls sleep. carefully regular i"),
(10,"HOUSEHOLD",9,"Customer#000000010","R2abOzd4DrMa2 6ELgG XcGMk6CzTCstA","563-218-3245",7160.6,"ironic even hockey players are carefully foxes. quickly sly packages wake furiously across the thinly express pac"),
(11,"HOUSEHOLD",21,"Customer#000000011","sKLrv0Aw2k0gBxOyJKN9jZkXmWM","791-259-9024",9674.51,"carefully even frays above the fur"),
(12,"BUILDING",13,"Customer#000000012","P8ypBhHRy5e","168-251-6200",6378.29,"even accounts use fluffily. bold ev"),
(13,"HOUSEHOLD",8,"Customer#000000013","GGiR2UUZI4LySqiQ7","773-606-1477",8582.84,"excuses wake furiously. slyly ironic ideas wake ironic somas. blithely final deposits s"),
(14,"BUILDING",12,"Customer#000000014","oVqyfM zwmHlAvbPARiTW1LCl,eWx","278-525-1783",8767.1,"closely regular attainments use carefully, foxes need to nod"),
(15,"HOUSEHOLD",10,"Customer#000000015","jfLX0HzKS1567KF5Df","224-177-8649",7953.84,"fluffily even accounts use above the even even platelets, blithely "),
(16,"HOUSEHOLD",24,"Customer#000000016","Ztsxm t028fX xdfUy2W0d2T2","616-577-8269",670.77,"special ironic packages serve about the pending epi"),
(17,"AUTOMOBILE",24,"Customer#000000017","WrKrmIHAvH79sCCwQUoq5","976-587-3399",8116.42,"pending ideas at the stealthily final foxes boost regular even requests. furiously ironic courts use careful"),
(18,"FURNITURE",1,"Customer#000000018","Ai705tZlCec0K2imYIn","307-852-5280",4136.16,"fluffily bold ideas wake above the unusual instructions, furiously unusual accounts cajole"),
(19,"AUTOMOBILE",11,"Customer#000000019","389swZc Rz7c8KoWMSYv 1oc6jr j","744-169-1627",2523.88,"slyly pending theodolites cajole carefully. carefully even accounts haggle furiously regular pinto beans"),
(20,"AUTOMOBILE",1,"Customer#000000020","YCJwOMmWLTi,IPo","121-778-2184",3165.2,"even asymptotes along the carefully special accounts detect fluffily bold do");


INSERT INTO tpch.supplier VALUES (1,13,"blithely final pearls are. instructions thra","Supplier#000000001",",wWs4pnykQOFl8mgVCU8EZMXqZs1w","800-807-9579",3082.86),
(2,5,"requests integrate fluffily. fluffily ironic deposits wake. bold","Supplier#000000002","WkXT6MSAJrp4qWq3W9N","348-617-6055",3009.73),
(3,22,"carefully express ideas shall have to unwin","Supplier#000000003","KjUqa42JEHaRDVQTHV6Yq2h","471-986-9888",9159.78),
(4,22,"quickly ironic instructions snooze? express deposits are furiously along the slyly","Supplier#000000004"," dxp8WejdtFKFPKa Q7Emf0RjnKx3gR3","893-133-4384",9846.01),
(5,9,"regular requests haggle. final deposits according to the","Supplier#000000005","W9VO4vl4dfoDYZ RhawP8xLoc","752-877-4449",-74.94),
(6,2,"furiously regular pinto beans detect. quickly final braids sleep carefully bold dolphins. slyly expre","Supplier#000000006"," UehnPL54frpEmHoH ","308-113-4748",7349.96),
(7,0,"carefully even packages affix furiously pending pinto beans.","Supplier#000000007","P5YDcLfpLaVkJcPnqeMv","400-485-7616",1680.44),
(8,15,"regular ironic theodolites wake blithely pending theodolites. furiously ironic deposits are carefully","Supplier#000000008","rPMo3 1Xbp","893-918-3335",-276.13),
(9,8,"carefully bold deposits haggle carefully against the blithely regular depo","Supplier#000000009","YUjB,vHmtrRQltUFBoTPLHZ","277-152-4591",5246.2),
(10,19,"express pinto beans nag daringly furiously bold warthogs. express special requests affix acc","Supplier#000000010","4qmhqqhjW7SYPwRI","454-730-5084",6824.98),
(11,18,"bold requests poach carefully furious requests. care","Supplier#000000011","CsPrqsunkt7YgKwh0u","488-820-1133",7866.58),
(12,1,"accounts breach along the fluffily regular foxes. special requests are. pending f","Supplier#000000012","kaik3OgMM5wUm7gB2Ezas,AuQ","814-239-7110",5222.05),
(13,2,"excuses among the requests are blithely about the unusual accounts. carefully final dependencies han","Supplier#000000013","MiaaIaqfDllL b68vBe","981-671-4833",5234.64),
(14,22,"slyly unusual ideas sleep according to the even accounts. slyly ","Supplier#000000014","WoeLCTPhTE","772-647-1392",4930.0),
(15,13,"ironic ideas sleep about the slyly ironic accounts. slyly regular packages sleep furio","Supplier#000000015","794RP7ebqdTYFOP","972-724-4712",4973.38),
(16,18,"ironic ironic pains cajole about the c","Supplier#000000016","BHEY8uFlvrLpsK e8Z6Is mIZ3rxmqSxh9QF3J","924-198-3387",5917.4),
(17,1,"blithely special ideas sleep. quickly even cour","Supplier#000000017","QuGRFF5yaMaDFhuxIa1NgohuefzNRhUq D2J6pbk","635-437-1512",8809.66),
(18,17,"carefully regular packages cajole unusual deposits-- carefully pending ideas are carefully a","Supplier#000000018","mY7or,fcLVt2xRk YjHgD,wJ5tXxIG3nOYeyl","826-385-7193",8914.76),
(19,21,"unusual ironic packages cajole ironic accounts. express cour","Supplier#000000019","98H M pNuJNG","514-988-5122",2143.15),
(20,3,"express final deposits wake sometimes regular pinto beans. express pains accordin","Supplier#000000020","iUPertyqE9ZqQ,ZKaj2,6btjBAlqLroEGbC26Q","256-531-4743",1199.56);

```

4. Create Aurora Connector
**Serverless Application Repository** -> **Available applications** -> Search **AthenaJdbcConnector** -> And create it with **Application Settings**:

-   **Application name**: Leave it as default name -  **AthenaJdbcConnector**
-   **SecretNamePrefix**: Put  **AthenaJdbcFederation**
-   **SpillBucket**: Put ***your s3 bucket name***
-   **DefaultConnectionString**: mysql://jdbc:mysql://***yourendpoint***:3306/mysql?user=awsuser&password=awspassword
-   **DisableSpillEncryption**: leave it as default value of  **false**
-   **LambdaFunctionName**: Put  **mysql**
-   **LambdaMemory**: leave it as default value of  **3008**
-   **LambdaTimeout**: leave it as default value of  **900**
-   **SecurityGroupIds**:  Put  **SecurityGroupIds**  value from Aurora Serverless
-   **SpillPrefix**: Put  **athena-spill/jdbc**
-   **SubnetIds**: Put  **Subnets**  value from Aurora Serverless

5. Run SQL in your Athena console:
```
select * from "lambda:mysql"."tpch"."region" limit 10;
select * from "lambda:mysql"."tpch"."nation" limit 10;
select * from "lambda:mysql"."tpch"."customer" limit 10;
select * from "lambda:mysql"."tpch"."supplier" limit 10;
```

## Athena Dynamodb Connector 
For detailed information about Amazon Athena DynamoDB Connector, refer [here](https://github.com/awslabs/aws-athena-query-federation/tree/master/athena-dynamodb).

1. Create ***part*** table in DynamoDB
**DynamoDB** -> **Table** -> **Create Table** :
- Table name: part
- Primary key: p_partkey

**Create item** in ***part*** table for test purpose
```
{
  "p_brand": "Brand#43",
  "p_comment": "blithely busy reque",
  "p_container": "LG BAG",
  "p_mfgr": "Manufacturer#4",
  "p_name": "burlywood plum powder puff mint",
  "p_partkey": 1,
  "p_retailprice": 901,
  "p_size": 31,
  "p_type": "LARGE PLATED TIN"
}
```
```
{
  "p_brand": "Brand#55",
  "p_comment": "even ironic requests s",
  "p_container": "LG CASE",
  "p_mfgr": "Manufacturer#5",
  "p_name": "hot spring dodger dim light",
  "p_partkey": 2,
  "p_retailprice": 902,
  "p_size": 4,
  "p_type": "LARGE POLISHED STEEL"
}
```
...

 2.  Create ***partsupp*** table in DynamoDB
 **DynamoDB** -> **Table** -> **Create Table** :
- Table name: partsupp
- Primary key: ps_partkey
- Sort key: ps_suppkey

**Create item** in ***partsupp*** table for test purpose
```
{
  "ps_availqty": 1111,
  "ps_comment": "carefully ironic deposits use against the carefully unusual accounts. slyly silent platelets nag quickly even",
  "ps_partkey": 1,
  "ps_suppkey": 2,
  "ps_supplycost": 400.75
}
```

3. **Serverless Application Repository** -> **Available applications** -> Search **AthenaDynamoDBConnector** -> And create it with **Application Settings**:

-   **Application name**: Leave it as default name -  **AthenaDynamoDBConnector**
-   **SpillBucket**: Put ***your s3 bucket name***
-   **AthenaCatalogName**: Put  **dynamo**
-   **DisableSpillEncryption**: leave it as default value of  **false**
-   **LambdaMemory**: leave it as default value of  **3008**
-   **LambdaTimeout**: leave it as default value of  **900**
-   **SpillPrefix**: Put  **athena-spill-dynamo**

4.  Run SQL in your Athena console:
```
select * from "lambda:dynamo"."default"."part" limit 10;
select * from "lambda:dynamo"."default"."partsupp" limit 10;
```

For detail information: , refer [here](https://athena-in-action.workshop.aws/40-federatedquery.html).

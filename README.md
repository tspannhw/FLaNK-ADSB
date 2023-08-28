# FLaNK-ADSB

ADSB-Y Flight Information


#### Hardware

https://flightaware.store/products/indoor-antenna-17cm-two-coil

https://flightaware.store/products/pro-stick-plus


#### Query to obtain airport info

````
https://opensky-network.org/api/flights/departure?airport=KEWR&begin=${now():toNumber():divide(1000):minus(604800)}&end=${now():toNumber():divide(1000)}

````

#### Example JSON Data

````
{
  "icao24" : "a46cc1",
  "firstSeen" : 1688869070,
  "estDepartureAirport" : "KEWR",
  "lastSeen" : 1688869079,
  "estArrivalAirport" : null,
  "callsign" : "UAL1317 ",
  "estDepartureAirportHorizDistance" : 645,
  "estDepartureAirportVertDistance" : 32,
  "estArrivalAirportHorizDistance" : null,
  "estArrivalAirportVertDistance" : null,
  "departureAirportCandidatesCount" : 325,
  "arrivalAirportCandidatesCount" : 0,
  "ts" : "1688869093501",
  "uuid" : "30682e35-e695-4524-8d1b-1abd0c7cffaf"
}
````

![data](https://github.com/tspannhw/FLaNK-ADSB/blob/main/images/nifidataopenskyairport.jpg?raw=true)

#### Kafka Data

![kafka1](https://github.com/tspannhw/FLaNK-ADSB/blob/main/images/openskyairportsmm2.jpg?raw=true)

![kafka2](https://github.com/tspannhw/FLaNK-ADSB/blob/main/images/smmopenskyairportdata.jpg?raw=true)

#### Flink SQL Table

````

CREATE TABLE `ssb`.`Meetups`.`openskyairport` (
  `icao24` VARCHAR(2147483647),
  `firstSeen` BIGINT,
  `estDepartureAirport` VARCHAR(2147483647),
  `lastSeen` BIGINT,
  `estArrivalAirport` VARCHAR(2147483647),
  `callsign` VARCHAR(2147483647),
  `estDepartureAirportHorizDistance` BIGINT,
  `estDepartureAirportVertDistance` BIGINT,
  `estArrivalAirportHorizDistance` VARCHAR(2147483647),
  `estArrivalAirportVertDistance` VARCHAR(2147483647),
  `departureAirportCandidatesCount` BIGINT,
  `arrivalAirportCandidatesCount` BIGINT,
  `ts` VARCHAR(2147483647),
  `uuid` VARCHAR(2147483647),
  `eventTimeStamp` TIMESTAMP(3) WITH LOCAL TIME ZONE METADATA FROM 'timestamp',
  WATERMARK FOR `eventTimeStamp` AS `eventTimeStamp` - INTERVAL '3' SECOND
) WITH (
  'scan.startup.mode' = 'group-offsets',
  'deserialization.failure.policy' = 'ignore_and_log',
  'properties.request.timeout.ms' = '120000',
  'properties.auto.offset.reset' = 'earliest',
  'format' = 'json',
  'properties.bootstrap.servers' = 'kafka:9092',
  'connector' = 'kafka',
  'properties.transaction.timeout.ms' = '900000',
  'topic' = 'openskyairport',
  'properties.group.id' = 'openskyairportflrdrgrp'
)

````
![ssbkafka](https://github.com/tspannhw/FLaNK-ADSB/blob/main/images/buildakafkatableairport.jpg?raw=true)


#### Flink SQL Query

````
select icao24, callsign, firstSeen, lastSeen, estDepartureAirport, arrivalAirportCandidatesCount,
      estDepartureAirportHorizDistance, estDepartureAirportVertDistance, estArrivalAirportHorizDistance, 
      estArrivalAirportVertDistance, departureAirportCandidatesCount
from openskyairport


select icao24, callsign, firstSeen, lastSeen, estDepartureAirport, estArrivalAirport, arrivalAirportCandidatesCount,
      estDepartureAirportHorizDistance, estDepartureAirportVertDistance, estArrivalAirportHorizDistance, 
      estArrivalAirportVertDistance, departureAirportCandidatesCount
from openskyairport
````

![flinksql](https://github.com/tspannhw/FLaNK-ADSB/blob/main/images/openskyairportresultsflinksql2.jpg?raw=true)

![flinksql2](https://github.com/tspannhw/FLaNK-ADSB/blob/main/images/ssbopenskyairportsqlresults.jpg?raw=true)



#### NiFI Flow to Acquire Data


![nififlow](https://github.com/tspannhw/FLaNK-ADSB/blob/main/images/flowall.jpg?raw=true)

![nifi0](https://github.com/tspannhw/FLaNK-ADSB/blob/main/images/airportstatusnifihead.jpg?raw=true)

![nifi1](https://github.com/tspannhw/FLaNK-ADSB/blob/main/images/airportflightsnifidemomode.jpg?raw=true)



![query](https://github.com/tspannhw/FLaNK-ADSB/blob/main/images/queryrecordairport.jpg?raw=true)

![nifi2](https://github.com/tspannhw/FLaNK-ADSB/blob/main/images/airportjsonwriter.jpg?raw=true)

![nifi5](https://github.com/tspannhw/FLaNK-ADSB/blob/main/images/updateRecordAirport.jpg?raw=true)

![nifi3](https://github.com/tspannhw/FLaNK-ADSB/blob/main/images/publishkafkarecordairport.jpg?raw=true)

![nifi4](https://github.com/tspannhw/FLaNK-ADSB/blob/main/images/updateParameterContextairport.jpg?raw=true)


# References

* https://github.com/tspannhw/pulsar-adsb-function
* https://flightaware.com/adsb/stats/user/TimothySpann
* https://www.linkedin.com/pulse/flight-monitor-cloudera-best-flow-contest-alexis-naranjo-escalona/
* https://github.com/tspannhw/raspberry-pi-adsb
* https://github.com/tspannhw/java-adsb
* https://opensky-network.org/
* https://github.com/tspannhw/FLiP-Py-ADS-B


# Data

* https://opensky-network.org/api/states/all?extended=1
* https://www.radarbox.com

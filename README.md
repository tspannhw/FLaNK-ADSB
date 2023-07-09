# FLaNK-ADSB
ADSB-Y Flights


#### Query to obtain airport info

````
https://opensky-network.org/api/flights/departure?airport=KEWR&begin=${now():toNumber():divide(1000):minus(604800)}&end=${now():toNumber():divide(1000)}

````

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

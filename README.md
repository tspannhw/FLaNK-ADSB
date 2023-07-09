# FLaNK-ADSB
ADSB-Y Flights


#### Query to obtain airport info

````
https://opensky-network.org/api/flights/departure?airport=KEWR&begin=${now():toNumber():divide(1000):minus(604800)}&end=${now():toNumber():divide(1000)}

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

## synology-monitoring
Simple shell script for capturing SNMP stats to [InfluxDB](https://docs.influxdata.com/influxdb/v1.7/) and monitoring health in [Grafana](https://grafana.com/).

![Grafana Dashboard](https://user-images.githubusercontent.com/25406580/56866768-f22a7d00-69a2-11e9-80e6-fd3145d619ac.png)


###### Requirements
* InfluxDB 
* Grafana
* Synology NAS with SNMP enabled


###### Setup
The script is best suited to run on the NAS itself and highly recommended. It can be ran on another device but the device would need to be able to run shell scripts, able to make SNMP calls such as using snmpwalk and has the [Synology SNMP MIB's](https://global.download.synology.com/download/Document/MIBGuide/Synology_DiskStation_MIB_Guide.pdf) . With minimal configuration, the Synology NAS should be more than capable of capturing this itself.

1. Save the script to a known location on the NAS. For example, /volume1/Local/Scripts/synology_snmp.sh 
2. Input the InfluxDB information such as URL/IP, ports, database name, username and password. 
3. Modify any other configuration settings, each should have an explanation.
4. On the Synology NAS, Select Control Panel > Terminal & SNMP > SNMP and enable SNMP V1, V2c service. <br> Ensure that you set the community to 'public' in the Synology settings as the `snmpwalk` command is utilizing the `-c public` flag 
5. On the Synology NAS, Select Control Panel > Task Scheduler > Create >> Scheduled Task >> User-defined Script.
6. Give the Task a recognizable name.
7. On the Task Settings, set the Run command to *bash /path/to/synology_snmp.sh*
8. On the Schedule:
   * Run on the following days: **Daily**
   * First run time: **00:00**
   * Frequency: **Every minute** *The frequency of times it runs within that minute is defined in the script itself*
   * Last run time: **23:59**
9. Import the Synology_dashboard.json file to your Grafana instance, Create > Import > Import .json file.

###### Setup InfluxDB

Setup InfluxDB per [this script](https://gist.github.com/viper25/f89cc61eb3feabe437ab1f2e0149cb44)
Configure the DB as below:

```sql
CREATE DATABASE "telegraf";
CREATE USER "srvc_synology" WITH PASSWORD 'xxxxxx';
GRANT WRITE ON telegraf TO srvc_synology;
SHOW USERS;
```

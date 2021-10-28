# check_raspi_temperature
nagios check for Raspberry Pi onboard CPU/GPU temperature sensors

The following external sensors will be reported if they are detected.  0c45:7402 Microdia TEMPerHUM Temperature & Humidity Sensor
```
DigiTemp DS9097U 1-wire USB-attached temperature sensor
0c45:7401 Microdia TEMPer Temperature Sensor
0c45:7402 Microdia TEMPerHUM Temperature & Humidity Sensor
```

# Requirements
ssh on nagios server, perl and ssh on monitored Raspberry Pi

#  Configuration

This script is executed remotely on a monitored system by the NRPE or check_by_ssh methods available in nagios.

If you are using the check_by_ssh method, you will need to add a section similar to the following to the services.cfg file on the nagios server.
This assumes you already have ssh key pairs configured.
```
define service {
       use                             generic-service
       hostgroup_name                  all_raspi
       service_description             temperature
       check_command                   check_by_ssh!/usr/local/nagios/libexec/check_raspi_temperature
       }
 ```
 
 Alternatively, if you are using the check_nrpe method, you will need to add a section similar to the following to the services.cfg file on the nagios server.
This assumes you already have ssh key pairs configured.
```
   define service{
      use                             generic-service
      hostgroup_name                  all_raspi
      service_description             temperature
      check_command                   check_nrpe!check_raspi_temperature -t 30
      }
  ```

If you are using the NRPE method, you will also need a command definition similar to the following on each monitored host in the /usr/local/nagios/nrpe/nrpe.cfg file:
```
    command[check_raspi_temperature]=/usr/local/nagios/libexec/check_raspi_temperature
```
 
# Output

Output will look similar to the following.  This example has a 1-wire USB-attached sensor with two temperature probes.
```
temperature OK -  CPU temperature: 44.0 C. GPU temperature: 44.0 C. External sensor 0 (wine cellar ambient) temperature: 23.75 C.  External sensor 1 (inside keezer) temperature: 2.75 C. |  cpu_temp=44.0;5:75;10:70 gpu_temp=44.0;5:75;10:70 ambient_temp=23.75;10:30;15:25;; inside_keezer_temp=2.75;1:14;2:7;;
```

# check_raspi_temperature
nagios check for Raspberry Pi onboard CPU/GPU temperature sensors, as well as 1-wire USB-attached temperature sensors.

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
 

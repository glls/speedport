# speedport

## python script

copy `check_speedport` file to `/usr/local/nagios/libexec/`

## nagios config


```
###############################################################################
#
# HOST DEFINITIONS
#
###############################################################################

# Define the switch that we'll be monitoring

define host {

    use                     generic-switch                      ; Inherit default values from a template
    host_name               speedport                           ; The name we're giving to this switch
    alias                   SpeedPort Plus                      ; A longer name associated with the switch
    address                 192.168.3.1                         ; IP address of the switch
    hostgroups              switches                            ; Host groups this switch is associated with
}



###############################################################################
#
# HOST GROUP DEFINITIONS
#
###############################################################################

# Create a new hostgroup for switches

define hostgroup {

    hostgroup_name          switches                            ; The name of the hostgroup
    alias                   Network Switches                    ; Long name of the group
}



###############################################################################
#
# COMMAND DEFINITION
#
###############################################################################

define command {
    command_name            check_speedport
    command_line            $USER1$/check_speedport
}



###############################################################################
#
# SERVICE DEFINITIONS
#
###############################################################################

# Create a service to PING to switch

define service {

    use                     generic-service                     ; Inherit values from a template
    host_name               speedport                           ; The name of the host the service is associated with
    service_description     PING                                ; The service description
    check_command           check_ping!200.0,20%!600.0,60%      ; The command used to monitor the service
    check_interval          5                                   ; Check the service every 5 minutes under normal conditions
    retry_interval          1                                   ; Re-check the service every minute until its final/hard state is determined
}

define service {

    use                     generic-service                     ; Inherit values from a template
    host_name               speedport                           ; The name of the host the service is associated with
    service_description     vDSL                                ; The service description
    check_command           check_speedport                     ; The command used to monitor the service
    check_interval          5                                   ; Check the service every 5 minutes under normal conditions
    retry_interval          1                                   ; Re-check the service every minute until its final/hard state is determined
}
```


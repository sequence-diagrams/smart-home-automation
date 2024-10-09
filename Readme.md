# Overview
A sequence diagram is a type of UML diagram that shows how entities operate with one another and in what order. A sequence diagram shows object interactions arranged in time sequence, which is crucial for understanding the dynamic behavior of the system.

# Instructions and Requirements
In this assignment, you will create a sequence diagram for a Smart Home Automation scenario.

For the sequence placeholders, the sequences will need to feature the following:

- alternate pathing (if/else)
- parallellism (asynchronous sequences)
- looping
- self call (self referencing)

# Scenario
Smart Home Automation System
Entities:

- Homeowner
- Smart Home App
- Voice Assistant
- Light Control System
- Thermostat System
- Security System
- Door Lock System
- Surveillance Camera System
- Home Automation Hub
- Appliance Control System
- Placeholder for additional systems
- Placeholder for additional systems
- Placeholder for additional systems

## Sequence 1: 

Morning Routine Automation
- **Triggering Automation**: _Homeowner_ schedules a morning routine through the _Smart Home App_, and the _App_ communicates with the _Home Automation Hub_ to initiate the routine.
- **Light and Temperature Adjustments**: The _Home Automation Hub_ sends a command to the _Light Control System_ to gradually increase brightness, and the _Thermostat System_ adjusts to the preferred morning temperature.
- **Appliance Activation**: The _Appliance Control System_ turns on the coffee maker and other appliances as scheduled.
- **Security System Monitoring**: _Surveillance Camera System_ records outdoor movement while the Door Lock System remains secure.
- **Status Updates**: The _Homeowner_ receives a routine completion notification from the _App_.

## Sequence Diagram - Morning Routine Automation
```mermaid
sequenceDiagram
    participant ho as Homeowner
    participant a as App
    participant hah as Home Automation Hub
    participant lcs as Light Control System
    participant ts as Thermostat System
    participant acs as Appliance Control System
    participant scs as Surveillance Camera System

    par Add new routine
        ho->>a:Select new routine
        a-->>ho:Show the list of possible routines
        ho->>a:Confirm the specific routine
        a->>hah:Record the new routine
    and Trigger routine
        loop Home Automation Hub
            hah->>hah: Check for scheduled routines
            Note right of hah: Read every single stablished routine
            par Light Control System Routine
                hah-->>lcs: Apply settings
                lcs->>lcs: Adjust brightness
                lcs-->>hah: Send back status routine completion
                hah-->>a: Send back notification
            and Thermostat System Routine
                hah->>ts: Apply settings
                ts->>ts: Adjust temperature
                ts-->>hah:Send back status routine completion
                hah-->>a: Send back notification
            and Appliance Control System Routine
                hah->>acs: Apply settings
                loop Active any appliance in the list
                    acs->>acs: Check current time
                        alt Is it time to activate ? 
                            acs->>acs: Turn on inactive appliance
                            acs->>hah: Notify appliance status
                        end
                end 
                acs-->>hah:Send back status routine completion
                hah-->>a: Send back notification
            and Surveillance Camera System Routine
                hah->>scs: Apply settings
                loop Checking door movement
                    alt Is there any movement ?
                        scs-->>hah:Send back status door movement
                    end
                end
                scs-->>hah:Send back status routine completion
                hah-->>a:Send back notification 
            end
        end
    end
```
## Sequence 6:  Light Control Shutdown

This simple case illustrates how a homeowner can easily turn off all lights in a smart home using voice commands

**Initiate Shutdown Command**: The _Homeowner_ issue a command "Turn off all lights" through his or her voice.

**Voice Activation**: The _Voice Assistance_ is listening and processing every sound that it catches.

**Command Processing**: The _Voice Assistant_ processes the command and sends a shutdown request to the _Home Automation Hub_.

**Relay Command to Light Control System**:

The _Home Automation Hub_ receives the request and relays the command to the _Light Control System_.

**Execute Shutdown**: The _Light Control System_ turns off all connected lights.

**Status Update**: The _Light Control System_ sends a confirmation back to the Home Automation Hub that all lights have been turned off.

**Notification**: The _Home Automation Hub_ can notify the _Smart Home App_ or the Voice Assistant of the successful shutdown, confirming to the Homeowner that all lights are off.


## Sequence Diagram - Light Control Shutdown
```mermaid
sequenceDiagram
    participant ho as Homeowner
    participant a as App
    participant va as Voice Assistant
    participant hah as Home Automation Hub
    participant lcs as Light Control System
    

    par Issue a command
        ho->>va:Issue a command

    and Voice Assitant
        va->>va: Proccess any sound and extract any command
        alt Was a command detected
            va->>hah: Send a shutdown request of lights
            hah->>lcs: Send "turn off" command to all connected lights
            lcs->>lcs: Turn off any light
            lcs-->>hah: Update status
            hah-->>a: Send the notification
        end
        
    end
```


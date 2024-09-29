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
- **Triggering Automation**: <u>Homeowner</u> schedules a morning routine through the <u>Smart Home App</u>, and the <u>App</u> communicates with the <u>Home Automation Hub</u> to initiate the routine.
- **Light and Temperature Adjustments**: The <u>Home Automation Hub</u> sends a command to the <u>Light Control System</u> to gradually increase brightness, and the <u>Thermostat System</u> adjusts to the preferred morning temperature.
- **Appliance Activation**: The <u>Appliance Control System</u> turns on the coffee maker and other appliances as scheduled.
- **Security System Monitoring**: <u>Surveillance Camera System</u> records outdoor movement while the Door Lock System remains secure.
- **Status Updates**: The <u>Homeowner</u> receives a routine completion notification from the <u>App</u>.

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
        a->>ho:Show the list of possible routines
        ho->>a:Confirm the specific routine
        a->>hah:Record the new routine
    and Trigger routine
        loop Home Automation Hub
            hah->>hah: Check for scheduled routines
            Note right of hah: Read every single stablished routine
            alt Is routine Light Control System ? 
                hah-->>lcs: Apply settings
                hah-->>lcs: Activate
                par Adjust brightness
                    lcs->>lcs: Read current ambient light
                    lcs->>lcs: Increase or decrease ambient light based in settings
                    lcs-->>hah:Send back status routine completion
                end
                
            else Other type
                alt Is routine Thermostat System ? 
                    hah-->>ts: Apply settings
                    hah-->>ts: Activate
                    par Adjust temperature
                        ts->>ts: Read current ambient temperature
                        ts->>ts: Increase or decrease temperature
                        ts->>ts: Apply changes based in settings
                        ts-->>hah:Send back status routine completion
                    end
                else Other type
                    alt Is routine Appliance Control System ?
                        hah-->>acs: Apply settings
                        hah-->>acs: Activate
                        par Turn on appliances
                            loop Inactive appliances
                                acs->>acs: Check current time
                                    alt Is it time to activate ? 
                                        acs->>acs: Turn on inactive appliance
                                        acs->>hah: Notify appliance status
                                    end
                                acs-->>hah:Send back status routine completion
                            end 
                        end
                    else Surveillance Camera System
                        hah-->>scs: Apply settings
                        hah-->>scs: Activate
                        par Checking door movement
                            loop Checking door movement
                                alt Is there any movement ?
                                    scs-->>hah:Send back status door movement
                                end
                            end
                            scs-->>hah:Send back status routine completion
                        end
                    end
                end
            end
        end
        hah-->>a:Send back notification 
    end
```
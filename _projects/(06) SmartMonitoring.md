---
name:  Smart Monitoring and Control System for Transporting Question Papers
type: coursework
tools: [Arduino Mega, Sim808, Electric Door Lock, Keypad, Django]
image: ../assets/img/smartmonitoring.png 
description:
external_url:  
---
### **{{page.name}}**
<br/>


{% include elements/video.html id="Hny4cRbhWTY" %}
<br/>

This was a sessional project of course CSE 404 (Digital System Design). 

<a class="github-button" href="https://github.com/ajoydas/SmartMonitoring" data-size="large" aria-label="View ajoydas/SmartMonitoring on GitHub">View Source Code on Github</a>
<br/>
**Description:**
Question papers of public examinations are transported using trunks. 
These trunks needs security so that no one can open them without authorization.
Moreover, tracking their way to the destination is also necessary so that any unwanted situation can be checked in the way.

**Our approach:**
Our system uses GPS trackers to track real time position of the trunks.
Each trunk will have an electronic lock. It can be unlocked by inserting pins that will be validated by a global server.

### Hardwares
- Arduino Mega
- Sim808 module
- DC 12v Solenoid Electric Door Lock
- 12v Door Lock Driver
- 4x4 Keypad
- LCD Module Advanced
- Battery

### Softwares
- Arduino IDE
- Django
- Public Hosting Server


# Building a Full LTE Network using srsRAN + Open5GS with over WSL (No SDR Required)

## Introduction
This project documents the complete implementation of a fully functional LTE network running entirely on a single laptop without any SDR hardware. The lab combines srsRAN 4G for the radio access network and Open5GS for the EPC core, interconnected through ZeroMQ virtual RF, and deployed inside WSL (Windows Subsystem for Linux).

What makes this lab particularly valuable is that it does not stop at a basic attach demonstration. Instead, it recreates a modern EPC architecture using CUPS (Control and User Plane Separation), the same design principle used in real operator networks today.

This setup allowed you to validate the below:
* S1AP signaling between eNB and MME
* NAS Attach procedure
* Subscriber authentication via HSS
* Session establishment via PGW-C(SMF).
* GTP-U tunnel creation via PGW-U(UPF).
* IP address allocation from EPC.
* Linux TUN/TAP integration.
* Deep troubleshooting of control plane vs user plane behavior.

All of these will be achieved without SDR hardware.

<br>

## Architectural Concept Applying CUPS in the Lab

In legacy EPC, the PGW handled both control and user plane. In modern networks, this is separated:

|Function|Open5GS Component|Plane|
|----|----|----|
|Mobility Management|MME|Control|
|Subscriber Database|HSS|Control|
|Session Management (SGW-C)|SGWC|Control|
|User Data Serving Gateway(SGW-U)|SGWU|User|
|Session Management (PGW-C)|SMF|Control|
|User Data Forwarding (PGW-U)|UPF|User|
|Radio Access|srsENB|Access|
|UE|srsUE|Access|
||||



In 5G Core the function of PGW-C resident in SMF and PGW-U function resident in UPF in order to deploy a real CUPS EPC:


## Topology on a Single Host
Because all components run on the same machine, we will use the loopback addressing in order to separate network elements logically.

**Open5GS** 
|Node|IP Address|
|--|--|
|MME	|127.0.0.2|
|SGWC|127.0.0.3|
|SGWU|127.0.0.6|
|PGW-U (UPF)|	127.0.0.7|
|PGW-C (SMF)	|127.0.0.4|
|HSS|127.0.0.8|
|PCRF|127.0.0.9|
|||

Open5GS internally implements this separation exactly as defined by 3GPP and this enables independent processes to communicate exactly as if they are on different servers.


Please refer to the diagram from Open5gs [website](https://open5gs.org/open5gs/docs/guide/01-quickstart/)

![Open5gs CUPS diagram](../assets/images/Open5GS_CUPS-01.jpg)



To install Open5GS please refer to [Quick start](https://open5gs.org/open5gs/docs/guide/01-quickstart/) or [Building Open5GS from Sources](https://open5gs.org/open5gs/docs/guide/02-building-open5gs-from-sources/)





**MiniCPS** is a framework for security research on Cyber Physical System
(CPS) under development by the SCy-Phy group at Singapore University of Technology and
Design (SUTD).

![SWaT testbed](images/testbed.png "swat-testbed")

There are at least 2 motivations behind the development of MiniCPS: the first
one are CPS themselves and the second one is the Secure Water Treatment Testbed (SWaT).

CPS are complex systems composed by different devices, such as Programmable
Logic Controllers (PLCs), SCADA, Network Switches, Firewalls, Human-Machine
Interfaces (HMI). A CPS is able to manage autonomously or semi-autonomously a
critical process, such as water purification and electrical power distribution. 
CPS devices are interconnected and sometimes connected to the Internet, indeed
they are exposed both to *physical* attack and *cyber* attacks. 
Additionally, CPS information may be difficult to obtain, prove
and share because of many reasons such as system diversity, industrial competition
and government issues. Moreover, CPS research requires different expertises
and it is quite hard to build a research team in the same University sitting
in the same room able to speak the same technical language.

The second motivation behind MiniCPS is more practical. SUTD is one of the
privileged University to have full access on a industrial scale water
treatment testbed, known as the *Secure Water Treatment Testbed (SWaT)*. SWaT
is composed by six *sequential* sub-processes such as chemical
treatments, ultra-filtration, reverse osmosis and Ultra-Violet light exposure.
Each sub-process is managed by a separate couple of PLC in redundant configuration. 
The whole system must guarantee a set of requirements on the amount of
pure filtered and recovered water and on the quality of the pure water.
In the picture below you can see a schematic view of the system that is the
same image displayed in real-time by the HMI.

![SWaT Process](images/swat-process.jpg "swat-process")

The SWaT network is a typical CPS layered network. The link are both wired
and wireless. Each sub-process implements a Device Level Ring (DLR) network
and the PLCs are connected in a star topology. The spoken industrial protocol
is *Ethernet/IP (ENIP)* that is a Real Time Ethernet (RTE) field bus based on
TCP/IP with a custom application layer designed to meet typical industrial 
communications requirements like *real-time response* and *packet order determinism*.
Technically ENIP is an Ethernet-based implementation of the Common Industrial Protocol
(CIP). In the picture below you can see a simplified schematic view of the
network architecture.

![SWaT Network Block Diagram](images/swat-network.png "swat-network")

Our long-term vision for MiniCPS is to create a research environment that is
*reproducible* in other research labs without the need to physically visit the
target testbed. We'd like to provide an *extensible* framework that encourages
*sharing* of information and results. MiniCPS is targeted to CPS network,
indeed it must deal with networked devices, control logic, physical
process and interactions among devices and the physical process itself. We
understand that the task is not trivial and we extensively did research about
other relevant emulation and simulation tools to build the framework. We
opted for a *stand on the shoulder of the giants* approach where it was
possible.

![MiniCPS diagram](images/minicps-diagram.png "minicps-diagram")

The picture above shows MiniCPS block diagram.  

For the *Physical* part we are using a simple
physical process simulator script, an abstraction of the system state and an
Physical layer API to access it.

For the *System* part we are using simple  control logic simulators. 
Usually a physical process simulation script and  some control logic simulation
scripts are able to update the abstracted system state using the Physical
Layer API.

For the *Cyber* part we are using *Mininet* network emulator.
Mininet was developed by some Stanford University researcher. Mininet is
a well established project and it was and it is used to reproduce even
very complex network topologies producing realistic traffic using
realistic protocol stacks.
Mininet is extremely lightweight and because is based on Linux
container virtualization it is able to emulate thousands of node on a single Linux
kernel. Mininet offers a big advantage over fully-virtualized solutions and
it is an ideal component for a research platform that aims to be easy to
configure, share and reproduce even on a single laptop.
Furthermore Mininet easily allows *Software-Defined Networking*
development, using *OpenFlow* as the control protocol.

The MiniCPS Physical Layer API is the *glue* of the platform.
CPS are composed by multiple devices *sharing* some resources and
*concurrently* access them. We decided to use a database to represent the
actual physical state and to manage concurrency problems. We raise the level
of abstraction implementing intuitive semantic functions such as *get* and
*set*. The aim is to develop a compatibility layer that support multiple
programming languages, because we understand that researcher with different
background may program at different level of abstraction using different
environments, E.g.: GNU/Octave for the pysical layer simulation and Python
for the control logic simulation. Furthermore we'd like to support multiple
database backends (currently we are working with SQLite) such as Redis
(NoSQL) and we'd like to add support to multiple XML graph formats to specify
the network topologies.

<!---
Fig: MiniCPS swat example: description

We *evaluated* MiniCPS reproducing the first stage of the SWaT testbed.
The water coming from the *grid* is collected in a Raw water tank
and it is eventually pumped to a chemical treatment stage and the result is
*collected* in a second tank. The water level is measured by level tank
indicator and by flow meter sensors. Motorized valves allows or block the
water flow and pumps push the water to the next stage.
PLC1 takes the decision but it has to
collaborate with PLC2 and PLC3. The physical process simulation update the
state of the system computing new level and flow values. The network
emulation uses realistic *addresses* such as IP and MAC and realistic
*services* are using realistic port numbers such as ENIP servers. The
scenario can be initialized with an optional external *attacker* node and an
*SDN controller*. 

Fig: MiniCPS swat example: network

We used this setting to reproduce the same attack that we performed on our
testbed evaluation. Those attack are based on ARP poisoning and include
passive and active man-in-the-middle-attacks. As you can see from the figure
the attacker is man-in-the-middleing between the HMI and PLC1. In our testbed
HMI is able both to monitor and to control the system and in this picture it
is sending a control signal to PLC1. the smart attacker is able to intercept
the traffic and change the relevant payload on the fly, fooling PLC1 that
will send the incorrect command to the actuator.
-->

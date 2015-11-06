**MiniCPS** is a framework for security research on Cyber Physical Systems
(CPS) developed by SCy-Phy team at Singapore University of Technology and
Design (SUTD).

There are at least 3 motivations behind the development of MiniCPS:

1. CPS itself
2. Secure Water Treatment Testbed (SWaT)
3. CPS researchers

CPS are complex systems composed by different devices, such as Programmable
Logic Controllers (PLCs), SCADA, Network Switches, Firewalls, Human-Machine
Interfaces (HMI). A CPS is able to manage autonomously or semi-autonomously a
critical process, such as water purification and electrical power distribution. 
CPS devices are interconnected and sometimes connected to Internet, indeed
they are exposed to both physical attack and network (cyber) attacks. 
the Internet. Additionally, CPS information may be difficult to obtain, prove
and share because of many reasons such as system diversity, industrial competition
and government issues. Moreover, CPS research requires different expertise
and it is quite hard to build a research team in the same University sitting
in the same room able to speak the same technical language.

The second motivation behind MiniCPS is more practical. SUTD is one of the
privileged University to have full access on a industrial scale water
treatment testbed, known as the *Secure Water Treatment Testbed (SWAT)*. SWAT
is composed by six *sequential* sub-processes that includes chemical
treatments, ultra-filtration, reverse osmosis and Ultra-Violet light exposure.
Each sub-process is managed by a separate couple of PLC in redundant configuration. 
The whole system must guarantee a set of requirements on the amount of
pure filtered and recovered water and on the quality of the pure water.
In Fig. TODO you can see a schematic representation of the water flows.

Fig: Swat real pic

Fig: Swat scheme

Fig: Swat network scheme

The SWAT network is a typical CPS layered network. The link are both wired
and wireless. Each sub-process implements a Device Level Ring (DLR) network
and the PLCs are connected in a star topology. The spoken industrial protocol
is *Ethernet/IP (ENIP)* that is a Real Time Ethernet (RTE) field bus based on
TCP/IP with a custom application layer designed to meet typical industrial 
communications requirements like *real-time response* and packet *determinism*.
Technically ENIP is an Ethernet-based implementation of the Common Industrial Protocol
(CIP).

CPS and SWAT testbed are the two main motivations behind the development of
MiniCPS, our long-term vision is to create a research environment that is
*reproducible* in other research labs without the need to physically visit the
testbed. We want an *extensible* framework that encourages
*sharing* of information and results. MiniCPS is targeted to CPS network,
indeed it must deal with Networked devices, control logic, physical
process and interactions among devices and the physical process itself. We
understand that the task is not trivial and we extensively did research about
other relevant emulation and simulation tool to build the framework. We
opted for a *stand on the shoulder of the giants* approach where it was
possible.

Fig: MiniCPS block diagram

Fig TODO  shows MiniCPS block diagram.  For the *Physical* part we
use a physical process simulator script, an abstraction of the system state and an
Physical layer API to access it.
For the *System* part we use control logic simulators. 
In summary physical process simulation script and control logic simulation
scripts modify the abstracted system state using an API and simulated devices
are able to communicate using a realistic network emulator.

For the *Cyber* part we decided to use *Mininet* emulator.
Mininet is a network emulator developed at Stanford university. The tool is
well established and it is used by network researcher to reproduce complex
topologies and generate and process real packets 
The emulator is extremely lightweight and because is based on Linux
container virtualization it is able to emulate thousands of node on a single Linux
kernel. Mininet offers a big advantage over fully-virtualized solutions and
it is an ideal component for a research platform that aims to be easy to
configure, share and reproduce even on a single laptop.
Furthermore mininet easily allows *Software-Defined Networking*
development using *OpenFlow* as the control protocol.

The MiniCPS physical layer API is the *glue* of the platform.
CPS are composed by multiple devices *sharing* some resources and
*concurrently* access them. We decided to use a database to represent the
actual physical state and to manage concurrency problems. We raise the level
of abstraction implementing intuitive semantic functions such as *get* and
*set*. The aim is to develop a compatibility layer that support multiple
programming languages such that two researcher can develop different scripts
in different programming languages and use our API to perform high level
function call,
and that is compatible with different storage back ends,

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

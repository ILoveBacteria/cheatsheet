# Term Cheatsheet

- [Term Cheatsheet](#term-cheatsheet)
  - [Database](#database)
  - [Operating System](#operating-system)
  - [Computer Network](#computer-network)

## Database

| id | name | page | description |
|-----|-----|-----|-----|
| 92 | correlation name | 110 | An identifier, such as T and S, used to rename a relation is referred to as a correlation name in the SQL standard, but it is also commonly referred to as a table alias, a correlation variable, or a tuple variable. |
| 91 | multiset relational algebra | 109 | To model this behavior of SQL, a version of relational algebra, called the multiset relational algebra, is defined to work on multisets: sets that may contain duplicates |
| 86 | compatible relations | 83 | We must ensure that the input relations to the union operation have the same number of attributes and types |
| 85 | project | 78 | The project operation allows us to produce this relation |
| 84 | select | 78 | The select operation selects tuples that satisfy a given predicate |
| 30 | primary key | 73 | To denote a candidate key |
| 29 | candidate keys | 73 | Minimal superkeys |
| 28 | superkey | 72 | A superkey is a set of one or more attributes that, taken collectively, allow us to identify uniquely a tuple in the relation |
| 27 | database instance | 70 | Snapshot of the data in the database at a given instant in time |
| 26 | atomic | 69 | A domain is atomic if elements of the domain are considered to be indivisible units |
| 25 | domain | 68 | Set of permitted values for a attribute |
| 24 | relation instance | 68 | Refer to a specific instance |
| 23 | attribute | 68 | Refer to column |
| 22 | relation | 68 | Refer to table |
| 21 | tuple | 68 | List of values |
| 20 | storage manager | 48 | The interface between the low-level data (file manager) stored in the database and the application programs and queries submitted to the system |
| 19 | query processor | 47 | simplify and facilitate access to data and improve performance |
| 18 | conceptual-design | 46 | Detailed overview |
| 17 | embedded SQL queries | 45 |  |
| 16 | metadata | 43 |  |
| 15 | data dictionary | 43 | DDL output |
| 14 | data-manipulation language (DML) | 42 | To express database queries and updates |
| 13 | data-definition language (DDL) | 42 | To specify the database schema |
| 12 | schema | 41 | The overall design of the database |
| 11 | instance | 41 | The collection of information stored in the database at a particular moment |
| 10 | Logical level | 38 | The next-higher level of abstraction describes what data are stored in the database and what relationships |
| 9 | Physical level | 38 | The lowest level of abstraction describes how the data are actually stored |
| 8 | Object-Based Data Model | 38 |  |
| 7 | Semi-structured Data Model | 37 |  |
| 6 | Entity-Relationship Model | 37 |  |
| 5 | Relational Model | 37 |  |
| 4 | data model | 37 | A collection of conceptual tools for describing data, data relationships, data semantics, and consistency constraints |
| 3 | consistency constraints | 35 | The data values stored in the database must satisfy certain types |
| 1 | data inconsistency | 35 | The various copies of the same data may no longer agree |


## Operating System

| id | name | page | description |
|-----|-----|-----|-----|
| 112 | bitmap | 67 | A bitmap is a string of n binary digits that can be used to represent the status of n items |
| 111 | Distributed Systems | 63 | A distributed system is a collection of physically separate, possibly heteronumerous computer systems that are networked to provide users with access to the various resources that the system maintains |
| 110 | Emulation | 62 | which involves simulating computer hardware in software, is typically used when the source CPU type is different from the target CPU type. |
| 109 | Virtualization | 62 | is a technology that allows us to abstract the hardware of a single computer |
| 108 | security | 61 | inappropriate access |
| 107 | Protection | 61 | any mechanism for controlling the access of processes or users to the resources defined by a computer system. |
| 106 | timer | 54 | A timer can be set to interrupt the computer after a specified period. The period may be fixed |
| 105 | protection ring | 53 | Intel processors have four separate protection rings, where ring 0 is kernel mode and ring 3 is user mode |
| 104 | privileged instructions | 53 |  |
| 103 | logical memory | 52 |  |
| 102 | physical memory | 52 |  |
| 83 | Multitasking | 51 | A logical extension of multiprogramming. In multitasking systems, the CPU executes multiple processes by switching among them, but the switches occur frequently, providing the user with a fast response time |
| 82 | Multiprogramming | 51 | by organizing programs so that the CPU always has one to execute. In a multiprogrammed system, a program in execution is termed a process. |
| 81 | trap (exception) | 50 | A software-generated interrupt caused either by an error or by a specific request from a user program that an operating-system service be performed by executing a special operation called a system call |
| 80 | system daemons | 50 | Some services are provided outside of the kernel by system programs that are loaded into memory at boot time |
| 79 | non-uniform memory access | 46 |  |
| 78 | shared system interconnect | 46 | The CPUs are connected by a shared system interconnect, so all CPUs share one physical address space. This approach known as non-uniform memory access, or NUMA |
| 77 | multicore | 44 | multiple computing cores reside on a single chip |
| 76 | symmetric multiprocessing (SMP) | 44 | in which each peer CPU processor performs all tasks, including operating-system functions and user processes |
| 75 | multiprocessor systems | 44 | such systemshave two (or more) processors, each with a single-core CPU |


## Computer Network

| id | name | page | description |
|-----|-----|-----|-----|
| 101 | IP spoofing | 68 |  |
| 100 | packet sniffer | 68 |  |
| 99 | encapsulation | 64 |  |
| 98 | frames | 63 | we'll refer to the link-layer packets as frames |
| 97 | datagrams | 62 | The Internet's network layer is responsible for moving network-layer packets known as datagrams from one host to another |
| 96 | segment | 62 | we'll refer to a transport-layer packet as a segment. |
| 95 | message | 61 | We'll refer to this packet of information at the application layer as a message |
| 94 | protocol stack | 61 | the protocols of the various layers are called the protocol stack |
| 93 | service model | 60 | each layer provides its service by (1) performing certain actions within that layer and by (2) using the services of the layer directly below it |
| 90 | bottleneck link | 56 | this simple two-link network, the throughput is min{Rc, Rs}, that is, it is the transmission rate of the bottleneck link. |
| 89 | average throughput | 55 |  |
| 88 | instantaneous throughput | 54 | The instantaneous throughput at any instant of time is the rate (in bits/sec) at which Host B is receiving the file. |
| 87 | traffic intensity | 50 | The ratio La/R |
| 74 | content-provider networks | 45 | By creating its own network, a content provider not only reduces its payments to upper-tier ISPs, but also has greater control of how its services are ultimately delivered to end users. |
| 73 | Internet Exchange Point (IXP) | 44 | which is a meeting point where multiple ISPs can peer together. |
| 72 | peer | 44 | they can directly connect their networks together so that all the traffic between them passes over the direct connection rather than through upstream intermediaries. When two ISPs peer, it is typically settlement-free, that is, neither ISP pays the other. |
| 71 | multi-home | 44 | Any ISP (except for tier-1 ISPs) may choose to multi-home, that is, to connect to two or more provider ISPs |
| 70 | points of presence (PoP) | 44 | A PoP is simply a group of one or more routers (at the same location) in the provider's network where customer ISPs can connect into the provider ISP |
| 69 | tier-1 ISPs | 43 |  |
| 68 | regional ISP | 43 |  |
| 67 | bandwidth | 39 | the link dedicates a frequency band to each connection for the duration of the connection |
| 66 | time-division multiplexing (TDM) | 39 |  |
| 65 | frequency-division multiplexing (FDM) | 39 |  |
| 64 | end-to-end connection | 38 | In circuit switching, when two hosts want to communicate, the network establishes a dedicated end-to-end connection |
| 63 | circuit switching | 38 | the resources needed along a path (buffers, link transmission rate) to provide for communication between the end systems are reserved for the duration of the communication session between the end systems |
| 62 | routing protocols | 37 | that are used to automatically set the forwarding tables. A routing protocol may, for example, determine the shortest path from each router to each destination and use the shortest path results to configure the forwarding tables in the routers. |
| 61 | forwarding table | 37 | maps destination addresses (or portions of the destination addresses) to that router's outbound links |
| 60 | packet loss | 36 | an arriving packet may find that the buffer is completely full with other packets waiting for transmission |
| 59 | queuing delays | 35 | If an arriving packet needs to be transmitted onto a link but finds the link busy with the transmission of another packet, the arriving packet must wait in the output buffer |
| 58 | output buffer (output queue) | 35 | which stores packets that the router is about to send into that link |
| 57 | store-and-forward transmission | 34 | Store-and-forward transmission means that the packet switch must receive the entire packet before it can begin to transmit the first bit of the packet onto the outbound link. |
| 56 | unguided media | 30 | wireless LAN or a digital satellite |
| 55 | guided media | 30 | such as a fiber-optic cable, a twisted-pair copper wire, or a coaxial cable |
| 54 | physical medium | 30 | Examples of physical media include twisted-pair copper wire, coaxial cable, multimode fiber-optic cable, terrestrial radio spectrum, and satellite radio spectrum |
| 53 | optical line terminator (OLT) | 26 |  |
| 52 | optical network terminator (ONT) | 27 |  |
| 51 | fiber to the home (FTTH) | 26 | provide an optical fiber path from the CO directly to the home |
| 50 | Cable modems | 26 | Cable modems divide the HFC network into two channels: downstream and upstream. As with DSL, access is typically asymmetric, with the downstream channel typically allocated a higher transmission rate than the upstream channel |
| 49 | hybrid fiber coax (HFC) | 26 | Because both fiber and coaxial cable are employed in this system, it is often referred to as hybrid fiber coax (HFC) |
| 48 | asymmetric | 25 | In DSL the downstream and upstream rates are different, and the access is said to be asymmetric |
| 47 | DSLAM | 24 | the analog signals from many such houses are translated back into digital format at the DSLAM |
| 46 | digital subscriber line (DSL) | 24 |  |
| 45 | Access Network | 23 | the network that physically connects an end system to the first router |
| 44 | Network Edge | 20 | they sit at the edge of the Internet |
| 43 | protocol | 20 | A protocol defines the format and the order of messages exchanged between two or more communicating entities |
| 42 | distributed applications | 16 | they involve multiple end systems that exchange data with each other |
| 41 | requests for comments (RFCs) | 16 | Internet standards |
| 40 | Internet Engineering Task Force (IETF) | 16 | Internet standards |
| 39 | Internet Service Providers (ISPs) | 15 |  |
| 38 | path or route | 15 | The sequence of communication links and packet switches traversed by a packet from the sending end system to the receiving end system |
| 37 | link-layer switches | 15 | Packet switches come in many shapes and flavors, but the two most prominent types in today's Internet are routers and link-layer switches. Typically used in access networks |
| 36 | routers | 15 | Packet switches come in many shapes and flavors, but the two most prominent types in today's Internet are routers and link-layer switches. Typically used in the network core |
| 35 | packets | 15 | the sending-end system segments the data and adds header bytes to each segment. |
| 34 | transmission rate | 15 | Different links can transmit data at different rates, with the transmission rate of a link measured in bits/second. |
| 33 | communication links | 15 |  |
| 32 | packet switches | 34 | packet switches (for which there are two predominant types, routers and link-layer switches) |
| 31 | end system | 13 |  |

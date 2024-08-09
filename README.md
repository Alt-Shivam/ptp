# ptp
A repo to ease the process of configuring ptp on linux.

## Things to check before proceeding to set linuxptp on the system.

### 1. Check if your nic support timestamping or not.

#### Timestamping Capabilities
- `hardware-timestamping`: This indicates whether the NIC supports hardware timestamping. For PTP, hardware timestamping is usually necessary to achieve accurate time synchronization.
- `tx-hw-ts`: Shows if the NIC supports hardware timestamping for transmitted packets.
- `rx-hw-ts`: Indicates if the NIC supports hardware timestamping for received packets.


```
sudo ethtool -T ens2f0
```
A ptp supported nic should be like this:
```
Time stamping parameters for ens2f0:
Capabilities:
        hardware-transmit
        software-transmit
        hardware-receive
        software-receive
        software-system-clock
        hardware-raw-clock
PTP Hardware Clock: 1
Hardware Transmit Timestamp Modes:
        off
        on
Hardware Receive Filter Modes:
        none
        ptpv1-l4-sync
        ptpv1-l4-delay-req
        ptpv2-l4-event
        ptpv2-l4-sync
        ptpv2-l4-delay-req
        ptpv2-l2-event
        ptpv2-l2-sync
        ptpv2-l2-delay-req
        ptpv2-event
        ptpv2-sync
        ptpv2-delay-req
```
### 2. Check if ptp packets are coming from the interface connected to Grandmaster.
```
sudo tcpdump -i ens2f0
```
You'll see packets of type `ptpv2`
```
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on ens2f0, link-type EN10MB (Ethernet), snapshot length 262144 bytes
19:25:03.009501 PTPv2, v1 compat : no, msg type : sync msg, length : 44, domain : 24, reserved1 : 0, Flags [none], NS correction : 0, sub NS correction : 22358016, reserved2 : 0, clock identity : 0x580fffe083765, port id : 14, seq id : 18082, control : 0 (sync msg), log message interval : 252, originTimeStamp : 1723211740 seconds, 9505416 nanoseconds
19:25:03.064489 PTPv2, v1 compat : no, msg type : announce msg, length : 64, domain : 24, reserved1 : 0, Flags [utc reasonable, timescale, time tracable, frequency tracable], NS correction : 0, sub NS correction : 0, reserved2 : 0, clock identity : 0x580fffe083765, port id : 14, seq id : 17284, control : 5 (Other), log message interval : 253, originTimeStamp : 1723211740 seconds 64386201 nanoseconds, origin cur utc :37, rsvd : 0, gm priority_1 : 128, gm clock class : 6, gm clock accuracy : 33, gm clock variance : 20061, gm priority_2 : 128, gm clock id : 0x580fffe083765, steps removed : 0, time source : 0x20
```

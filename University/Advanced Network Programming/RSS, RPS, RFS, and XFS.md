RSS, RPS, RFS, and XFS are techniques used in networking, particularly in the context of multi-core processors and network interface cards (NICs), to improve the performance of packet processing. Here's a brief overview of each:

1. **RSS (Receive Side Scaling)**:
   - **Purpose**: Distributes incoming network traffic across multiple CPU cores to improve processing efficiency and reduce bottlenecks.
   - **Stateful or Stateless**: Stateless. It does not maintain any session information; it simply hashes packets to determine which core should handle them.
   - **Overhead**: Helps with per-packet overhead by allowing multiple cores to process packets simultaneously.

2. **RPS (Receive Packet Steering)**:
   - **Purpose**: Similar to RSS, RPS directs incoming packets to specific CPU cores based on a hash of the packet's source and destination addresses. It is often used in conjunction with RSS.
   - **Stateful or Stateless**: Stateless. Like RSS, it does not maintain session state.
   - **Overhead**: Primarily helps with per-packet overhead by balancing the load across CPU cores.

3. **RFS (Receive Flow Steering)**:
   - **Purpose**: Extends RPS by steering packets of the same flow to the same CPU core, which can improve cache locality and reduce processing overhead for flows that are handled by the same core.
   - **Stateful or Stateless**: Stateless. It does not maintain state but uses flow information to direct packets.
   - **Overhead**: Helps with per-packet overhead by optimizing flow processing.

4. **XFS (Transmit Flow Steering)**:
   - **Purpose**: Similar to RFS but for outgoing packets. It ensures that packets belonging to the same flow are sent out through the same CPU core, which can help maintain performance and reduce context switching.
   - **Stateful or Stateless**: Stateless. It does not maintain session state but uses flow information for steering.
   - **Overhead**: Helps with per-packet overhead by optimizing the transmission of packets.

### Summary of Differences:
- **RSS** and **RPS** focus on distributing incoming packets across multiple cores, while **RFS** and **XFS** focus on maintaining flow consistency for incoming and outgoing packets, respectively.
- All four techniques are stateless, meaning they do not maintain session information.
- They primarily help with per-packet overhead by improving the efficiency of packet processing across multiple CPU cores. 

In summary, these techniques are designed to enhance the performance of network processing in multi-core environments by efficiently distributing and managing packet flows.
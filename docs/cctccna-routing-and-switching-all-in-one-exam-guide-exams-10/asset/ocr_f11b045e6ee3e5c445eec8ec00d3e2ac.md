information sent by the source.

# Ready/Not Ready Signals

When the destination receives more traffic than it can handle, it can send a *not ready* signal to the source, indicating that the source should stop transmitting data. When the destination has a chance to catch up and process the source’s data, the destination responds back to the source with a *ready* signal. Upon receiving the ready signal, the source can resume sending data.

Two problems are associated with the use of ready/not ready signals to implement flow control. First, the destination may respond to the source with a not ready signal when its buffer fills up. While this message is on its way to the source, the source is *still sending* information to the destination, which the destination will probably have to drop because its buffer space is full. The second problem with the use of these signals is that once the destination is ready to receive more information, it must first send a ready signal to the source, which must receive it before more information can be sent. This causes a delay in the transfer of information. Because of these two inefficiencies with ready/not ready signals, they are not commonly used to implement flow control. Sometimes this process is referred to as *stop/start*, where you stop transmitting for a period and then start retransmitting.

# Windowing

*Windowing* is a much more sophisticated method of flow control than using ready/not ready signals. With windowing, a window size is defined that specifies how much data (segments) can be sent before the source has to wait for an acknowledgment (ACK) from the destination. Once the ACK is received, the source can send the next batch of data (up to the maximum defined in the window size).

Windowing accomplishes two things: First, flow control is enforced, based on the window size. In many protocol implementations, the window size is dynamically negotiated up front and can be renegotiated during the lifetime of the connection. This ensures that the most optimal window size is used to send data without having the destination drop anything. Second, through the windowing process, the destination tells the source what was
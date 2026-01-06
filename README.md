<p align="center">Binary Protocol Documentation Standard</p>
<p style='text-align:center'>1.0</p>

# Rational
When working with binary protocols the format and details of the documentation have been inconstant and often hard to follow.  The binary protocol documentation standard (BPDS) tries to fix this, using a standard format that is human readable, intuitive, and text based.  The hope is that will improve the readability of the documentation for byte based protocols.

The format is well defined and designed so that a machine can parse a BPDS definition and be able to decode what bytes in a protocol are for what.  It can not interpret the meaning of the byte, but would be able to extract the structure of the packet, knowing how to recognize the start and end of the packet and label the parts.

The BPDS does not try to represent possible states, message responses, the flow of a protocol, handshaking, or how the protocol should be used.  It just tries to explain the structure of a packet of data, not how to work with it.

# Example
We start with an example to give an idea of how this works.

```
<Header=0xFF><Version><Cmd><Len:2><Data:Len><Footer=0x77>
```

This is a generic description of a made up protocol.  You can see that it starts with a header set to the value 0xFF followed by the version and the command, because there is no size provided then these are 1 byte each.  The length is next and the :2 tells us that it's 2 bytes.  The length is followed by the data and we can see the size is a field 'Len' so there will be 'Len' bytes of data (that we can only determine when we read a packet).  The data will be followed by a footer byte that is always set to 0x77. 

So you would expect to see a data stream something like:

```
0xFF 0x01 0x01 0x00 0x08 0x64 0x64 0x10 0x10 0x00 0xFF 0x00 0x00 0x77
```
Documented in Autodoc format (as a C comment):

```
/*******************************************************************************
 * NAME:
 *    Command Protocol
 *
 * SYNOPSIS:
 *    <Header=0xFF><Version><Prop><Cmd><Len:2><Data:Len><Footer=0x77>
 *
 * PARAMETERS:
 *    Head -- Marker used for resyncing.  Always 0xFF.
 *    Version -- What version of the protocol we are using.  Currently only
 *               version 1 is defined.
 *    Prop -- What optional properties are included in this packet.  This is a bit
 *            field where:
 *               0x01 -- Transparency
 *               0x02 -- Fill style
 *               0x04 -- Border
 *    Cmd -- What command we are going to be executing.  See details below
 *           for a description of available commands.
 *    Len -- The length of the 'Data'.  This does not include the 'Footer',
 *           only the data it's self.  This is in big endian.
 *    Data -- The data for this command.  The size will depend on the command.
 *    Footer -- Marker used for resyncing.  Always 0x77.
 *
 * FUNCTION:
 *    This is general packet format for sending commands to the graphic
 *    processor system.
 *
 * REPLY:
 *    Replies will be an acknowledge packet.
 *
 * SEE ALSO:
 *    
 ******************************************************************************/
```

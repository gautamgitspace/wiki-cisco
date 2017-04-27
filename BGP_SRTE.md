# BGP SRTE
- SRGB - Segment Routing Global Block (SRGB) is the range of labels reserved for segment routing. SRGB is local property of an segment routing node.
In MPLS, architecture, SRGB is the set of local labels reserved for global segments. In segment routing, each node can be configured with a different SRGB value and hence the absolute SID value associated to an IGP Prefix Segment can change from node to node.
- Prefix SID - A segment ID that contains an IP address prefix calculated by an IGP in the service provider core network. Prefix SIDs are globally unique.
A node SID is a special form of prefix SID that contains the loopback address of the node as the prefix. It is advertised as an index into the node specific SR Global Block or SRGB.
- Adjacency SID - A segment ID that contains an advertising router’s adjacency to a neighbor. An adjacency SID is a link between two routers. Since the adjacency SID is relative to a specific router, it is locally unique.
The Adjacency Segment Identifier (adj-SID) is a local label that points to a specific interface and a next hop out of that interface. No specific configuration is required to enable adj-SIDs.
Once segment routing is enabled over IS-IS for an address-family, for any interface that IS-IS runs over, the address-family automatically allocates an adj-SID towards every neighbor out of that interface.

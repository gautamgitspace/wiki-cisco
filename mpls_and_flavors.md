# LDP LSP and RSVP-TE LSP

- If you are new to the world of MPLS, these flavors of LSP might be confounding to you. You might also be seeking a difference between the two rather just knowing what they are. Going with the basic stuff, LSP is simply a Label Switched Path. LDP and RSVP-TE are two different ways of telling the neighboring routers that speak MPLS, what label to use.
- RSVP-TE uses its signaling element and sets up an LSP end-to-end (ingress-to-egress). So label distribution is coordinated among all the LSRs along a path. LDP, on the other hand, has no signaling element. It sets up LSPs hop-by-hop, and labels can be distributed between neighbors independently of what other LSRs along the path are doing. Because it has no signaling element, LDP depends on the network's IGP to determine the path an LSP must take, whereas RSVP-TE can set up paths independently of what the IGP determines to be the optimal path to a destination - hence the Traffic Engineering part of the protocol.

# Signaling and Label Distribution explained:
- Through signaling, LSRs along a path know that they are a part of a given LSP.
- Label Distribution is the means by which an LSR tells an upstream LSR what label value to use for a particular LSP. Label Distribution cab be performed by 4 different protocols:
1. LDP
2. RSVP-TE
3. Constraint-Based Routed LDP(CR-LDP - CR-LDP is an extension of LDP that adds signaling capabilities almost identical to those of RSVP-TE. CR-LDP is not much used. This isn't because it has something wrong with its implementation but RSVP-TE is darling of the industry.
4. MPBGP - Multi-protocol BGP supports label distribution through the IPv4 and IPv6 labeled unicast address families. This is a specialized function in support of Layer 3 MPLS VPNs

# RSVP-TE from here on:
- In its working, RSVP-TE can verify that the path is good to go and that the resources it wants to use from the network are available before the LSP is established. It can also reserver the resources it needs.
- How the signaling works in RSVP-TE?

RSVP-TE uses two messages to accomplish this signaling: PATH messages and RESV messages. Suppose R1, the ingress LSR, wants to set up an LSP to R4, the egress (remember that LSPs are unidirectional). It sends a PATH message requesting the LSP setup hop-by-hop along the path to R4, and each LSR along the path sets aside the resources requested. If the requested resources are not available at any LSR, that router sends a message back to the ingress; the ingress LSR must then either find another path or fail to establish the LSP. Assuming the path is good and the PATH message reaches the egress LSR, that router responds by sending a RESV message back to the ingress. This message performs the label distribution function as described in the previous post.

Another way to look at all this is that RSVP-TE signaling flows downstream from the ingress to egress; label distribution flows upstream from egress to ingress.

- There are quite a few resources that can be reserved by means of the Path message. The most easily understood is bandwidth: Each interface along the path can be asked to set aside some amount or percentage of its available bandwidth to be used exclusively by the signaled LSP. Other resources that can be reserved are functions, such as Fast Reroute (FRR), or capabilities such as the ability of the LSP to take resources away from another LSP, or the ability to resist having its resources taken away by another LSP.

PATH and RESV messages carry information about the requested reservations, distributed labels, and so on by means of Objects within the messages. One of the most essential RSVP-TE Objects, enabling RSVP to set up an LSP over some path other than what the local IGP determines to be the best path, is the Explicit Route Object (ERO). The ERO is a list of LSRs – specified by IP addresses – through which the path must pass.

# Explicit Route Objects

- Explicit Route Objects (EROs) limit LSP routing to a specified list of LSRs. By default, RSVP messages follow a path that is determined by the network IGP's shortest path. However, in the presence of a configured ERO, the RSVP messages follow the path specified.
- EROs consist of two types of instructions: loose hops and strict hops. When a loose hop is configured, it identifies one or more transit LSRs through which the LSP must be routed. The network IGP determines the exact route from the inbound router to the first loose hop, or from one loose hop to the next. The loose hop specifies only that a particular LSR be included in the LSP.
- When a strict hop is configured, it identifies an exact path through which the LSP must be routed. Strict-hop EROs specify the exact order of the routers through which the RSVP messages are sent. You can configure loose-hop and strict-hop EROs simultaneously. In this case, the IGP determines the route between loose hops, and the strict-hop configuration specifies the exact path for particular LSP path segments.
In the topology shown in [Figure](http://gitlab.cisco.com/abhigaut/Wiki/blob/master/img/ERO.png), traffic is routed from Host C1 to Host C2. The LSP can pass through Routers R4 or Router R7. To force the LSP to use R4, you can set up either a loose-hop or strict-hop ERO that specifies R4 as a hop in the LSP. To force a specific path through Router R4, R3, and R6, configure a strict-hop ERO through the three LSRs.
- More about [RSVP](http://www.juniper.net/techpubs/en_US/junos/topics/concept/mpls-security-rsvp-signaling-protocol-understanding.html)

# Stake Capital's Factom Networking Archive

### May 13, 2019 - Testnet Networking Implementation

There is currently no way to dial out from one public IP and then establish a separate TCP connection to a second IP for return traffic via the Factom daemon. This means that, for example, an Authority Server cannot send outbound traffic via a NAT Gateway (with one public IP) and receive inbound traffic via a load balancer on a separate public IP. 

Since this feature is not currently supported at the application layer, an intuitive workaround approach to a similar result might be to rewrite the source IP of outbound packets using `iptables` (to set the load balancer’s public IP address as the source IP). However, the preconfigured NAT Gateway from AWS (where we were running our test infrastructure) automatically overwrites the source IP of all packets passing through to be that of the NAT Gateway itself. Of course, our load balancer also does not allow backend-generated outbound traffic to pass through either. Both of these rendered the `iptables` / source IP rewriting approach unviable.

In the mean time, we’ve embarked on implementing a combined forward and reverse proxy solution, whereby we can continue using our existing load balancer.

As we discussed in the [Factom #technical-chat](https://discordapp.com/channels/419201548372017163/441597446636830721), this feature (setting the "source" IP seen by the server's peers) should be available at the daemon level. It is, for example, available in Tendermint, via the “external_addr” config setting (which tells peers to send requests back to a specific IP address). [Who](https://factomize.com/forums/members/192/) seemed receptive to this feature, and said on Discord, "I'll see what I can do to include it on my medium-term roadmap." So hopefully we will see this feature available in the Factom daemon soon.


this is useful for the condition if the server where miss-blocks-checker is installed gets hacked, the attacker will not receive logs from the server with validator.
This type of alert does not collect personal data

If machine logs are not collected, then how does alerting get information about validators?

gRPC node address to get signing info and validators info from, defaults to localhost:9090. By default, nodes are installed with an open gRPC port, so it will most likely be possible to prescribe the seeds and peers commands or any node from the addrbook, since if the node is intended to work on a public network, then why would its owner disable gRPC.
Tendermint RPC node to get block info from. Defaults to http://localhost:26657. In the same way, you can write the seeds and peers of the command or any node from the addrbook project.
🤔 Where is the best place to install this alerting software?

As usual, the best option is a separate server.

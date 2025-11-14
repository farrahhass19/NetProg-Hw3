TO-DO:
    fixed segfaults, code works.

Done:
    Split Horizon and Poison Reverse in send_dv()
    Broadcast DV updates to all alive neighbors in broadcast_dv()
    Distance Vector updates (Bellman-Ford) in dv_update()
    Packet forwarding using Longest Prefix Match (LPM)

Log demonstrating DV convergence:

[R1] ROUTES (init):
  network         mask            next_hop        cost
  192.168.10.0    255.255.255.0   0.0.0.0         0
[R1] ROUTES (dv_update):
  network         mask            next_hop        cost
  192.168.10.0    255.255.255.0   0.0.0.0         0
  10.0.20.0       255.255.255.0   127.0.1.2       1
[R1] ROUTES (dv_update):
  network         mask            next_hop        cost
  192.168.10.0    255.255.255.0   0.0.0.0         0
  10.0.20.0       255.255.255.0   127.0.1.2       1
  10.0.30.0       255.255.255.0   127.0.1.3       3
[R1] ROUTES (dv_update):
  network         mask            next_hop        cost
  192.168.10.0    255.255.255.0   0.0.0.0         0
  10.0.20.0       255.255.255.0   127.0.1.2       1
  10.0.30.0       255.255.255.0   127.0.1.2       2

Log demonstrating LPM forwarding:

[R1] FWD dst=127.0.1.2 via=127.0.1.2 cost=2 ttl=7
[R2] FWD dst=127.0.1.3 via=127.0.1.3 cost=1 ttl=6
[R3] DELIVER src=192.168.10.10 ttl=5 payload="hello LPM world"


Log demonstrating Neighbor timeout behavior: 

[R1] ROUTES (neighor-dead):
  network         mask            next_hop        cost
  192.168.10.0    255.255.255.0   0.0.0.0         0
  10.0.20.0       255.255.255.0   127.0.1.2       65535
  10.0.30.0       255.255.255.0   127.0.1.2       65535
[R1] ROUTES (dv_update):
  network         mask            next_hop        cost
  192.168.10.0    255.255.255.0   0.0.0.0         0
  10.0.20.0       255.255.255.0   127.0.1.2       65535
  10.0.30.0       255.255.255.0   127.0.1.3       3

What did you observe after removing the Split Horizon/Poison Reverse logic?

Because of the infinite bouncing, the router still believes that there's a path to the dead router. Thus, the router displays:

[R3] ROUTES (neighbor-dead):
  network         mask            next_hop        cost
  10.0.30.0       255.255.255.0   0.0.0.0         0
  192.168.10.0    255.255.255.0   127.0.1.2       2
  10.0.20.0       255.255.255.0   127.0.1.2       1
[R3] ROUTES (dv_update):
  network         mask            next_hop        cost
  10.0.30.0       255.255.255.0   0.0.0.0         0
  192.168.10.0    255.255.255.0   127.0.1.2       65535
  10.0.20.0       255.255.255.0   127.0.1.2       1
[R3] ROUTES (dv_update):
  network         mask            next_hop        cost
  10.0.30.0       255.255.255.0   0.0.0.0         0
  192.168.10.0    255.255.255.0   127.0.1.2       4
  10.0.20.0       255.255.255.0   127.0.1.2       1

  This is incorrect.
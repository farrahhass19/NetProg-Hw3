TO-DO:
    fixed segfaults, code works.

Done:
    Split Horizon and Poison Reverse in send_dv()
    Broadcast DV updates to all alive neighbors in broadcast_dv()
    Distance Vector updates (Bellman-Ford) in dv_update()
    Packet forwarding using Longest Prefix Match (LPM)

Log demonstrating DV convergence:

Log demonstrating LPM forwarding:

Log demonstrating Neighbor timeout behavior: 

What did you observe after removing the Split Horizon/Poison Reverse logic?
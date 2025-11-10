TO-DO:
    Packet forwarding using Longest Prefix Match (LPM)
    Neighbor timeout handling
    Handle data packets and call forward_data

Review:
    Neighbor timeout detection (use neighbor_t's last_heard)
    Handle control (DV) messages and call dv_update

Done:
    Split Horizon and Poison Reverse in send_dv()
    Broadcast DV updates to all alive neighbors in broadcast_dv()
    Distance Vector updates (Bellman-Ford) in dv_update()

Log demonstrating DV convergence:


Log demonstrating LPM forwarding:

Log demonstrating Neighbor timeout behavior: 

What did you observe after removing the Split Horizon/Poison Reverse logic?
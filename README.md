# Hearbeat in Disturbuted system
Have you ever thought how, in a disturbuted system ,one the fundamental challege is detecting whether a node or server is alive or not? . unlike in monolithic setupewhere every thing is running on single machine and health checks are straightforward , but in a disturbuted architecture , you have  multiple server , machine ,data center running across different locations. This is where the heartbeat mechanism comes into the picture.

Imagine a bunch of server is working together to process millions of request per day and suddenly one of server silently crash. How quickly can the system detect that failure and respond to it?

This is where the heartbeat mechanism comes into the picture.
A heartbeat is a lightweight signal that nodes send to each other at regular intervals to confirm that they are still alive and functioning. It sounds simple, but it forms the backbone of failure detection in distributed systems.

In a heartbeat system, there are two core components:
1Sender: the node that periodically sends a small “I’m alive” signal.
           


    class Node:
      def __init__(self, node_id, interval=2):
          self.node_id = node_id
          self.interval = interval   # heartbeat interval in seconds
          self.is_alive = True
  
      def send_heartbeat(self):
          if self.is_alive:
              print(f"[Heartbeat] Node {self.node_id} sending heartbeat...")
              return True
          else:
              print(f"[Heartbeat] Node {self.node_id} is DOWN. No heartbeat.")
              return False
          
  2) Receiver :the node or monitoring service that listens for these signals and keeps track of when each one arrives.       

    class Receiver:
        def __init__(self, timeout=5):
            self.timeout = timeout
            self.last_heartbeat = {}

      def receive(self, node_id):
          self.last_heartbeat[node_id] = time.time()
          print(f"[Receiver] Heartbeat received from Node {node_id}")
  
      def check_node_status(self, node_id):
          if node_id not in self.last_heartbeat:
              return f"Node {node_id} → No heartbeat received yet"
  
          elapsed = time.time() - self.last_heartbeat[node_id]
          if elapsed > self.timeout:
              return f"Node {node_id} → FAILED (no heartbeat for {elapsed:.2f}s)"
          else:
              return f"Node {node_id} → ALIVE (last heartbeat {elapsed:.2f}s ago)"



The mechansim work through  two role keys : sender and receiver .The sender commits to broadcasting its heartbeat at regular intervals, say every x seconds. The receiver monitors these incoming heartbeats and maintains a record of when the last heartbeat was received. If the receiver does not hear from the sender within an expected timeframe, it can reasonably assume something has gone wrong.    
When a node crashes, stops responding, or becomes isolated due to network partitions, the heartbeats stop arriving. The monitoring system can then take appropriate action, such as removing the failed node from a load balancer pool, redirecting traffic to healthy nodes, or triggering failover procedures.

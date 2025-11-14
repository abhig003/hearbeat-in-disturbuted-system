# Hearbeat in Disturbuted system
Have you ever thought how, in a disturbuted system ,one the fundamental challege is detecting whether a node or server is alive or not? . unlike in monolithic setupewhere every thing is running on single machine and health checks are straightforward , but in a disturbuted architecture , you have  multiple server , machine ,data center running across different locations. This is where the heartbeat mechanism comes into the picture.

Imagine a bunch of server is working together to process millions of request per day and suddenly one of server silently crash. How quickly can the system detect that failure and respond to it?

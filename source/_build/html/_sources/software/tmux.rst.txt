####
TMUX
####

tmux_ is available on Katana. It can be used to manage multiple sessions, including keeping then alive despite a terminal losing connectivity or being shutdown.

When you login to Katana using the terminal, it is a "live" session - if you close the terminal, the session will also close. If you shut your laptop or turn off the network, you will also kill the session.

This is fine, except when you have a long running program - say you are downloading a large data set - and you need to leave campus.

In these situations, you can use :code:`tmux` to create an **interruptible session**. :code:`tmux` has other powerful features - multiple sessions and split screens being two of many features.

To start tmux, type :code:`tmux` at the terminal. A new session will start and there will be a green information band at the bottom of the screen. 

Anything you start in this session will keep running even if you are disconnected from that session regardless of the reason. Except when the server itself is rebooted. That - and I hope this is obvious - will kill all sessions.

If you do get detached, you can re-attach by logging into the same server and using the command :code:`tmux a`

tmux is similar to another program called :code:`screen` which is also available. 

.. _tmux: https://github.com/tmux/tmux/wiki

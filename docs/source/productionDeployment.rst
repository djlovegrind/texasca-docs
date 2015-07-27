Deploying to Production
===================================================

The Hermes Software Platform is hosted using something called Docker Containers. 

Docker wraps up our software on a complete filesystem, that way it can run just like it does on your local computer. These are the so-called "containers." 

To deploy updated code so that it runs on our production environment you need to follow the simple instructions listed in these docs. **Always proceed with caution when interacting with live production processes.** Problems in the production environment can be delicate to fix, and if things go down our customers will very likely be affected!

So, on to our how-to for deploying to production.

1: Register your SSH Key with Digital Ocean
-------------------------------------------------

If you have already been added as a registered user for our Docker container environment, you can proceed directly to Step 2. Otherwise, contact Zachary Cook to have your ssh key added as a user for our hosting solution.

You will need to share your ssh key, and be added as a user.


2. Gain Access to the Docker Container
--------------------------------------------

Once you are ready, navigate to the directory location where your ssh key is held. Typically this is in your root directory --> .ssh. So from root you would navigate to it with ``cd .ssh``.

When there, type the command ``ssh core@IP_ADDRESS_FROM_DIGITAL_OCEAN -i your_ssh_key``. 

Replacing, of course, ``IP_ADDRESS_FROM_DIGITAL_OCEAN`` with the IP address you find from your secure Digital Ocean account and ``your_ssh_key`` with the name of your SSH key, usually ``id_rsa``. 

If this is sucessful, you should see your command line prompt change to ``core@texasca2 ~ $`` indicating that you are now in the Docker container environment. 

If unsuccessful, check that the IP address is correct from Digital Ocean or, if that is correct, contact Zachary Cook about making sure that your SSH key is an authorized user for the Docker container. 

3. Startup Admin Client
------------------------

4. Move into Directory Being Updated and Pull Updated Code
------------------------------------------------------------

5. Build from Source
----------------------

6. Docker Commit new Admin Image
---------------------------------

7. Remove Old Container so the New Container can Spawn
--------------------------------------------------------

8. Exit SSH Session
------------------------
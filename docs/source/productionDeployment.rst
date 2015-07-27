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

When there, type the command ``ssh core@IP_ADDRESS_FROM_DIGITAL_OCEAN -i your_ssh_key``. This will start your SSH session with the IP Address where the Hermes Platform source code runs. 

Replace, of course, ``IP_ADDRESS_FROM_DIGITAL_OCEAN`` with the IP address you find from your secure Digital Ocean account and ``your_ssh_key`` with the name of your SSH key filename, usually ``id_rsa``. 

If this is sucessful, you should see your command line prompt change to ``core@texasca2 ~ $`` indicating that you are now in the Docker container environment. 

In the container, a helpful command you will need to use during the following steps is ``docker ps``. This will simply show you the active containers running on our server right now. This lets you know what is currently running on Digital Ocean and give you useful information such as ``CONTAINER_ID``, ``IMAGE`` name, ``CREATED`` timestamp, and container ``NAMES``.

If unsuccessful, check that the IP address is correct from Digital Ocean or, if that is correct, contact Zachary Cook about making sure that your SSH key is an authorized user for the Docker container. 

3. Run Script to Create "Administrative" Container
------------------------------------------------------

Our updating process relies on creating an "admin" container, which is essentially a copy of the actual container. In this container, we can complete administrative tasks like updating code to its latest version. Creating these admin containers is a tricky process, so we have written scripts to help us do this step and save some time/complexity. 

There are two images you might need to update during your deployment to production. One is the ``texasca/client`` (which holds the front-end code, such as ``texasca-store`` & ``texasca-dashboard``) and the other is ``texasca/api``. This corresponds the type of administrative container you will need to make. So for the ``texasca/api`` you run script ``admin_api.sh`` and for the ``texasca/client`` run script ``admin_client.sh``. 

Run the corresponding script by typing ``./script_name.sh``. 

If successful, the administrative container will be created, and a command prompt will immediately open in the corresponding container. Perfect!

If unsuccessful, you might get an error: ``Error response from daemon: Conflict. the name "texasca-client-admin" is already in use by container ...``. If this happens, it is because the administrative container created the last time someone made an update was not deleted. **It is not the container actually running on production**, so you can safely remove it using ``docker rm -f CONTAINER_ID``. You can easily find the container ID information using ``docker ps`` commmand. 

Then try the command ``./admin_api.sh`` or ``./admin_client.sh`` again to successfully create your administrative container. 

4. Move into Directory Being Updated and Pull Updated Code
------------------------------------------------------------

We are now getting to a part that should be pretty familiar for you and part of the everyday coding process. You just need to navigate to the correct repository for the code you need to update to its newest version. Once there, we are going to ``git pull origin master`` to pull the newest code from master branch hosted on the company's private Github accounts. 

**Before you do this** make sure to see a ``git diff`` to make sure no unintended changes are going to be updated to our production environment code. If everything looks in order, go ahead and ``git pull origin master``.

5. Build from Source
----------------------

Before you are ready to have the updated code run on production, run ``grunt build`` from the command line to build the updated application from source. You should be done now doing work from inside of the container. Hit ``Ctrl`` + ``P`` + ``Q`` to get back outside of the specific container. 

6. Docker Commit new Admin Image
---------------------------------

Now commit the new administrative container you have made and updated using command ``docker commit CONTAINER_ID texasca/client`` or ``docker commit CONTAINER_ID texasca/api`` replacing ``CONTAINER_ID`` with the id of the administrative container you have just created. 


7. Remove Old Container so the New Container can Spawn
--------------------------------------------------------

You can now remove the old container your administrative one is going to replace. This **is** the container actually running live on production, but it will be automatically replaced by the administrative container you have just created after you remove it. Run ``docker rm -f CONTAINER_ID`` to remove it.

You should now have the updated code running on production! Run ``docker ps`` to checkout your handywork and confirm that a container was just created running the new code. Please remember to ``docker rm`` the now useless administrative container, which should clearly have been created longer ago than the actual production container that should have just spawned. 

8. Exit SSH Session
------------------------

That should complete your successful deployment of updated code to our production environment. Contact Zachary Cook at zach_cook@texasca.com if you have any questions. 

Run ``exit`` to end the SSH session. 
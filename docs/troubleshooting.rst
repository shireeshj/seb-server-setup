Troubleshooting
===============

This page's purpose is to collect frequently ask questions, problems and workarounds that are reported and known. 
There shall be at least a problem description, an optional explanation if needed, a solution description and the SEB Server version and domain information. For Example:

--------------------------------

- **Version**     : 1.0.0
- **Domain**      : production / bundled / basic
- **Problem**     : This is an example problem description.
- **Explanation** : An explanation is not mandatory but shall be provided if you want to give some background information to the problem.
- **Solution**    : A description of the solution, workaround or upcoming fixes.

------------------------------

- **Version** : 1.0.0
- **Domain**  : general bundled setups
- **Problem** : SEB Server is not able to connect internally to MariaDB SEB Server is not able to connect internally to MariaDB within the docker network. You see a connection error on the seb-server log files in SEB Server startup. 

.. note::
   Use "docker logs seb-server" to quickly show the SEB Server application logs.
   
- **Explanation** : It seems that this error happens sometimes when the services are starting slowly, and either the MariaDB service or the network is not up and running while the seb-sever service tries to connect. It also fails on following automated service restarts for some unknown reason. We are working on a better solution for this.
- **Solution** : to stop and restart the seb-server service manually. This usually works if the described explanation is the reason of the connection error.

    - Stop the service with:

        .. code-block:: bash
 
            $ docker stop seb-server
   
    - And restart it again with:

        .. code-block:: bash
 
            $ docker restart seb-server
   
    - See the logs if this solved the problem:

        .. code-block:: bash
 
            $ docker logs seb-server
   
-------------------------------------
   
- **Version** : 1.0.0
- **Domain**  : general bundled setups
- **Problem** : The initial password file setup was not correctly done for the first time or you need or want to reset the password.
- **Solution** : You have to remove the configuration volume form the docker engine and create the password file again. Then restarting the service.

.. note::
   Do this with care! If there is already data stored that was encrypted for security reasons with a password that was originally set on the
   initial setup, this must not be changed. Otherwise the SEB Server service will not be able to decrypt the old secured data anymore. 
   In the initial setup you are able to change the password but from the time one you have living data on the service you never should change
   this initial password. 

 - Stop the whole service with:

  .. code-block:: bash
 
    $ docker-compose down
   
 - Go into the config directory and create the "secret" file with the password as described in the setup process:

  .. code-block:: bash
 
    $ cd config
    $ vi secret
    $ cd ..
   
 - Start the whole service again with:

  .. code-block:: bash
 
    $ docker-compose up -d
   
------------------------------------------------


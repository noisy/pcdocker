Docker Remote Debugging
=======================

To connect to python remote interpreter, ssh connection has to be established. You may have heard about article |docker-ssh-considered-evil|_

.. _docker-ssh-considered-evil: https://jpetazzo.github.io/2014/06/23/docker-ssh-considered-evil/
.. |docker-ssh-considered-evil| replace:: *If you run SSHD in your Docker containers, you're doing it wrong!*


There is a better tool to login into container if you want to access to shell, i.e. `docker exec`_. However, connecting to remote interpreter is a different thing, much more difficult. You can read more about `attempts on StackOverflow`_.

.. _docker exec: https://docs.docker.com/reference/commandline/exec/
.. _attempts on StackOverflow: http://stackoverflow.com/a/28675525/338581

To avoid putting sshd into production-ready docker image, we create another docker image on top of main ``redditclone_django`` image from `compose/pycharm/Dockerfile <../compose/pycharm/Dockerfile>`_. That's why you have to first build main `Dockerfile <../Dockerfile>`_, and you can do that by::

    $ docker-compose -f dev.yml build

After that, you can build and run debug container::    

    $ docker-compose -f debug.yml build
    $ docker-compose -f debug.yml up

Container should be ready, when::

    ...
    debug_1    | Starting OpenBSD Secure Shell server: sshd
    ...

will be displayed in docker-compose logs.

You can test ssh conection using password *docker*, by::

    ssh docker_redditclone@localhost -p 2222
    
or you can also use ssh-key::

    ssh -i compose/pycharm/.ssh_keys_to_docker/id_rsa docker_redditclone@localhost -p 2222


PyCharm
^^^^^^^

This repository comes with already prepared "Run/Debug Configurations" for docker.

Even if you tested ssh connection manually, you **have to** do that one more time, through PyCharm, to active "Run/Debug Configurations" for Docker. To do that, please go to *Settings > Build, Execution, Deployment > Deployment > docker_redditclone* and click *Test SFTP connection*:

You should see:

**Important note:** if in the future you will somehow lose ability to login to docker container through PyCharm, always start with *Test SFTP Connection*. Very often this solves all issues.

Configure Remote Python Interpreter based on deployment settings
----------------------------------------------------------------

...

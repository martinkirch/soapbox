Installing Showergel
====================

Install the program by the running ``pip install showergel``.

Run the interactive installer by calling ``showergel_install``.
It will explain on the terminal what is happening and what to do from here.
If you stick to defaults, the installer will:

* create a database (``showergel.db``)
  and a configuration file (``showergel.ini``) in the current directory,
* create a systemd user service called ``showergel`` ;
  in other words you can ``systemctl --user status|start|stop|restart showergel``.
* enable the service and systemd's lingering_ so Showergel will start automatically at boot time.
* after installation Showergel will be available at http://localhost:2345/.

The installer's questions allow you to:

* change the name of the systemd service and the DB/configuration files' names.
  This is useful if you want to run multiple instances of showergel because you're
  broadcasting multiple programs from the same machine.
  For example, responding ``radio`` will create ``radio.db``, ``radio.ini`` and a ``radio`` service.
* skip the service creation, if you prefer launching Showergel yourself.
* create another systemd user service for your Liquidsoap script,
  so systemd will automatically launch everything - this is recommanded.
  In that case, the installer creates two systemd services with the
  same basename: for example,
  ``radio_gel`` (showergel service associated to ``radio``)
  and ``radio_soap`` (the Liquidsoap script you provided for ``radio``).


Configuration
-------------

See comments in the ``.ini`` file created by the installer.

Install for back-end development
--------------------------------

Depencencies, installation and packing is done by Poetry_.
Once Poetry is installed,
create a Python3 environment,
activate it, and run ``poetry install`` from a clone of this repository.

When developping, your Liquidsoap script and Showergel should be launched manually.
Run ``showergel_install --dev`` to create an empty database (``showergel.db``)
and a basic configuration file (``showergel.ini``)
in the current folder.
Read (and edit, maybe) ``showergel.ini``,
launch Liqudisoap, then run ``showergel showergel.ini``.
You'll likely want to enable the detailed log by setting ``level=DEBUG``
in the ``logger_root`` section of the ini file.

Test with ``pytest``.

Install for front-end development
---------------------------------

The front-end is written in JavaScript packed with Yarn_,
with VueJS_'s `single-file components <https://v3.vuejs.org/guide/single-file-component.html>`_.
We use the Bulma_ CSS Framework.

To modify the front-end, you must beforehand install Yarn and Vue_CLI_,
then run ``yarn install`` from the repository root.
Start the live-building server with ``yarn serve``.
If you don't have time to install the whole back-end,
you can call the demo app by creating a ``front/.env`` file that contains::

    VUE_APP_BACKEND_URL=https://arcane-retreat-54560.herokuapp.com

Similarly, a fully-working HTML/JS/CSS build is included in this repository,
so one doesn't have to install ``yarn`` and Vue while working on the back-end.
When you're done, run ``yarn build`` and commit modifications in the ``/showergel/www/`` folder.

See also `this VSCode configuration hint <https://code.visualstudio.com/docs/setup/linux#_visual-studio-code-is-unable-to-watch-for-file-changes-in-this-large-workspace-error-enospc>`_.


Deploy to Heroku in demo mode
-----------------------------

In demo mode, the application starts by putting fake data in the database.
It's enabled by putting ``demo = True`` in the configuration file's ``[listen]`` section.

Source repository includes such a configuration,
so you can create and push the app right after cloning:

.. code-block:: bash

    heroku create --region eu
    git push heroku main
    heroku logs --tail

We might need to update ``requirements.txt`` from time to time:

.. code-block:: bash

    poetry export --dev --without-hashes -f requirements.txt --output requirements.txt

``--dev`` is here because ``requirements.txt`` is also used by ReadTheDocs
to compile the present documentation, which requires a Sphinx extension.


.. _Poetry: https://python-poetry.org/
.. _lingering: https://www.freedesktop.org/software/systemd/man/loginctl.html
.. _Yarn: https://yarnpkg.com/
.. _VueJS: https://vuejs.org/
.. _Bulma: https://bulma.io/
.. _Vue_CLI: https://cli.vuejs.org/

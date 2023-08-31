..  _configuration:

Configuration
=============

.. _configuration_run_instance:

Running a Tarantool instance
----------------------------

Command:

..  code-block:: console

    $ tarantool [OPTION ...]

Tarantool is started by entering either of the following command:

*   3.0 and higher:

    ..  code-block:: console

        $ tarantool --name INSTANCE_NAME --config CONFIG_FILE_PATH [OPTION ...]

*   2.11 and earlier:

    ..  code-block:: console

        $ tarantool INITIALIZATION_FILE_PATH [OPTION ...]

See also: :ref:`Command-line options <configuration_command_options>`.



..  _box-cfg-params-prior:

Configuration approaches
------------------------

Tarantool configuration parameters can be specified in different ways.
The priority of parameter sources is the following, from higher to lower:

*   configuration file
*   (Legacy) ``box.cfg`` options (see :ref:`Programmatic configuration <configuration_code>`)
*   :ref:`environment variables <box-cfg-params-env>`
*   :ref:`tt configuration <tt-config>`


..  _configuration_file:

Configuration in a file
-----------------------

Levels:

-   global
-   group
-   replicaset
-   instance



.. _configuration_single_instance:

Single instance
~~~~~~~~~~~~~~~

Single instance (``config.yaml``):

..  literalinclude:: /code_snippets/test/config/single.yaml
    :language: yaml
    :dedent:

To run:

.. code-block:: console

    tarantool --name instance-001 --config config.yaml


.. _configuration_multiple_instances:

Multiple instances
~~~~~~~~~~~~~~~~~~

Basic overview.
Links to Replication and Sharding tutorials.
Mention failover approaches.

.. _configuration_remote:

Remote configuration (etcd)
~~~~~~~~~~~~~~~~~~~~~~~~~~~

..  admonition:: Enterprise Edition
    :class: fact

    Remote configuration is supported by the `Enterprise Edition <https://www.tarantool.io/compare/>`_ only.

Local config:

..  literalinclude:: /code_snippets/test/config/etcd.yaml
    :language: yaml
    :dedent:

Put a remote config:

.. code-block:: console

    etcdctl put /example/config/all.yaml < config.yaml

Run:

.. code-block:: console

    tarantool -n instance-001 -c etcd.yaml
    tarantool -n instance-002 -c etcd.yaml
    tarantool -n instance-003 -c etcd.yaml


Describe how to reload configuration.



..  _configuration_basic_options:

Basic configuration options
---------------------------

.. _configuration_basic_options_memory:

Memory
~~~~~~

``memtx_memory``


.. _configuration_basic_options_directories:

Directories
~~~~~~~~~~~


.. _configuration_basic_options_listen:

Listen URI
~~~~~~~~~~

.. _configuration_basic_options_database:

Database
~~~~~~~~

Mode, isolation.


.. _configuration_basic_options_credentials:

Credentials
~~~~~~~~~~~

.. _configuration_basic_options_application:

Application
~~~~~~~~~~~

See :ref:`Launching a binary program <app_server-launching_app_binary>`.



..  _box-cfg-params-env:

Environment variables
---------------------

Starting from version :doc:`2.8.1 </release/2.8.1>`, you can specify configuration parameters via special environment variables.
The name of a variable should have the following pattern: ``TT_<NAME>``,
where ``<NAME>`` is the uppercase name of the corresponding :ref:`box.cfg parameter <box-cfg-params-ref>`.

For example:

* ``TT_LISTEN`` -- corresponds to the ``box.cfg.listen`` option.
* ``TT_MEMTX_DIR`` -- corresponds to the ``box.cfg.memtx_dir`` option.

In case of an array value, separate the array elements by comma without space:

..  code-block:: console

    export TT_REPLICATION="localhost:3301,localhost:3302"

If you need to pass :ref:`additional parameters for URI <index-uri-several-params>`, use the ``?`` and ``&`` delimiters:

..  code-block:: console

    export TT_LISTEN="localhost:3301?param1=value1&param2=value2"

An empty variable (``TT_LISTEN=``) has the same effect as an unset one meaning that the corresponding configuration parameter won't be set when calling ``box.cfg{}``.


.. _configuration_command_options:

Command-line options
--------------------

Options that can be passed when :ref:`running a Tarantool instance <configuration_run_instance>`:

..  option:: -h, --help

    Print an annotated list of all available options and exit.

.. _index-tarantool_version:

..  option:: -v, -V, --version

    Print the product name and version.

    **Example**

    ..  code-block:: console

        % tarantool --version
        Tarantool 3.0.0-entrypoint-746-g36ef3fb43
        Target: Darwin-arm64-Release
        ...

    In this example:

    *   ``2.11.1`` is a Tarantool version.
        Tarantool follows semantic versioning, which is described in the :ref:`Tarantool release policy <release-policy>` section.

    *   ``Target`` is the platform Tarantool is built on.
        Platform-specific details may follow this line.


..  option:: -e EXPR

    Execute the 'EXPR' string. See also: `lua man page <https://www.lua.org/manual/5.3/lua.html>`_.

    **Example**

    ..  code-block:: console

        % tarantool -e "print('Hello, world!')"
        Hello, world!

..  option:: -l NAME

    Require the 'NAME' library. See also: `lua man page <https://www.lua.org/manual/5.3/lua.html>`_.

    **Example**

    ..  code-block:: console

        % tarantool -l luatest.coverage script.lua

..  option:: -j cmd

    Perform a LuaJIT control command. See also: `Command Line Options <https://luajit.org/running.html>`_.

    **Example**

    ..  code-block:: console

        % tarantool -j off app.lua

..  option:: -b ...

    Save or list bytecode. See also: `Command Line Options <https://luajit.org/running.html>`_.

    **Example**

    ..  code-block:: console

        % tarantool -b test.lua test.out

..  option:: -d SCRIPT

    Activate a debugging session for 'SCRIPT'. See also: `luadebug.lua <https://github.com/tarantool/tarantool/blob/master/third_party/lua/README-luadebug.md>`_.

    **Example**

    ..  code-block:: console

        % tarantool -d app.lua


..  option:: -i [SCRIPT]

    Enter an :ref:`interactive mode <interactive_console>` after executing 'SCRIPT'.

    **Example**

    ..  code-block:: console

        % tarantool -i


..  option:: --

    Stop handling options. See also: `lua man page <https://www.lua.org/manual/5.3/lua.html>`_.


..  option:: -

    Stop handling options and execute the standard input as a file. See also: `lua man page <https://www.lua.org/manual/5.3/lua.html>`_.




..  toctree::
    :hidden:

    configuration/configuration_code


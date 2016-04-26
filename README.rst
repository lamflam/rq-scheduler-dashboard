Introduction
============

``rq-scheduler-dashboard`` is a copy of rq-scheduler-dashboard for rq-scheduler jobs,
based on the code from this PR: https://github.com/ducu/rq-dashboard/pull/95


Installing
----------

.. code:: console

    $ pip install rq-scheduler-dashboard


Running the dashboard
---------------------

Run the dashboard standalone, like this:

.. code:: console

    $ rq-scheduler-dashboard
    * Running on http://127.0.0.1:9181/
    ...


.. code:: console

    $ rq-scheduler-dashboard --help
    Usage: rq-scheduler-dashboard [OPTIONS]

      Run the RQ Scheduler Dashboard Flask server.

      All configuration can be set on the command line or through environment
      variables of the form RQ_SCHEDULER_DASHBOARD_*. For example RQ_SCHEDULER_DASHBOARD_USERNAME.

      A subset of the configuration (the configuration parameters used by the
      underlying flask blueprint) can also be provided in a Python module
      referenced using --config, or with a .cfg file referenced by the
      RQ_SCHEDULER_DASHBOARD_SETTINGS environment variable.

    Options:
      -b, --bind TEXT               IP or hostname on which to bind HTTP server
      -p, --port INTEGER            Port on which to bind HTTP server
      --url-prefix TEXT             URL prefix e.g. for use behind a reverse proxy
      --username TEXT               HTTP Basic Auth username (not used if not set)
      --password TEXT               HTTP Basic Auth password
      -c, --config TEXT             Configuration file (Python module on search
                                    path)
      -H, --redis-host TEXT         IP address or hostname of Redis server
      -P, --redis-port INTEGER      Port of Redis server
      --redis-password TEXT         Password for Redis server
      -D, --redis-database INTEGER  Database of Redis server
      -u, --redis-url TEXT          Redis URL connection (overrides other
                                    individual settings)
      --interval INTEGER            Refresh interval in ms
      --help                        Show this message and exit.


Integrating the dashboard in your Flask app
-------------------------------------------

The dashboard can be integrated in to your own `Flask`_ app by accessing the
blueprint directly in the normal way, e.g.:

.. code:: python

    from flask import Flask
    import RQ_SCHEDULER_DASHBOARD

    app = Flask(__name__)
    app.config.from_object(RQ_SCHEDULER_DASHBOARD.default_settings)
    app.register_blueprint(RQ_SCHEDULER_DASHBOARD.blueprint)

    @app.route("/")
    def hello():
        return "Hello World!"

    if __name__ == "__main__":
        app.run()


The ``cli.py:main`` entry point provides a simple working example.


Developing
----------

We use piptools_ to keep our development dependencies up to date

::

    $ pip install --upgrade pip
    $ pip install git+https://github.com/nvie/pip-tools.git@future

Now make changes to the ``requirements.in`` file, and resolve all the
2nd-level dependencies into ``requirements.txt`` like so:

::

    $ pip-compile --annotate requirements.in


Develop in a virtualenv and make sure you have all the necessary build time (and
run time) dependencies with

::

    $ pip install -r requirements.txt


Develop in the normal way with

::

    $ python setup.py develop


Then use Fabric to perform various development tasks. For example, to tag, build
and upload to testpypi

::

    $ git tag 0.3.5   # no 'v' prefix or anything
    $ fab build
    $ fab upload

This requires write access to both the GitHub repo and to the PyPI test site.

See ``fab -l`` for more options and ``fab -d <subcommand>`` for details.


Maturity notes
--------------

The RQ Scheduler Dashboard is currently being developed and is in beta stage.


.. _piptools: https://github.com/nvie/pip-tools
.. _Flask: http://flask.pocoo.org/
.. _RQ: http://python-rq.org/

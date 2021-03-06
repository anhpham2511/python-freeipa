Welcome to FreeIPA client's documentation!
===========================================

Installation
------------

  .. code-block:: bash

    pip install python-freeipa

Example usage
-------------
Client using username and password to connect to specific IPA server.

.. code-block:: python

    from python_freeipa import Client
    client = Client('ipa.demo1.freeipa.org', version='2.215')
    client.login('admin', 'Secret123')
    user = client.user_add('test3', 'John', 'Doe', 'John Doe', preferred_language='EN')
    print(user)

Client using DNS service discovery.
By default, we will try to find IPA servers using the FQDN
of the host trying to connect to an IPA server.
Alternatively you can also manually specify a domain here.

For DNS service discovery, you need to have the `srvlookup` module installed.

.. code-block:: python

    from python_freeipa import Client
    client = Client(version='2.215', dns_discovery=True)
    client.login('admin', 'Secret123')
    user = client.user_add('test3', 'John', 'Doe', 'John Doe', preferred_language='EN')
    print(user)

Breaking changes in 1.0 release
-------------------------------
Previously, Python FreeIPA client covered only small fraction of FreeIPA API calls.
By introducing code generator we cover all FreeIPA API calls.
By default autogenerated client is used. It has different API signatures.
Therefore if you want to preserve old behaviour you should just use `ClientLegacy` instead of `Client`.
For example:

.. code-block:: python

    from python_freeipa import ClientLegacy
    client = ClientLegacy('ipa.demo1.freeipa.org', version='2.215')
    client.login('admin', 'Secret123')


Contributing
------------

The only dependency is Python Requests library (http://docs.python-requests.org/)

See also API documentation: https://ipa.demo1.freeipa.org/ipa/ui/#/p/apibrowser/

Install python-freeipa in development mode along with dependencies:

  .. code-block:: bash

    pip install -e .[tests]

Run tests suite:

  .. code-block:: bash

    python setup.py test

Recreation of MetaClient
------------------------

It is possible to manually recreate the "ClientMeta" class.
This might be needed if the IPA/IdM Server you are using is not matching
the on that has been used to build the packaged version.

Here is what you need to do:

.. code-block:: bash

  # fetch code, create virtual environment, and install required packages
  git clone git@github.com:opennode/python-freeipa.git
  cd python-freeipa
  python3 -m venv venv
  source venv/bin/activate
  pip install requests-kerberos python-freeipa
  # recreate the ClientMeta class
  contrib/py_ipa_api_recreate --source-url ipa.demo1.freeipa.org --source-url-user admin --source-url-pass Secret123
  # move the file where it belongs
  mv meta_api.py src/python_freeipa/client_meta.py
  # build the python package
  python setup.py sdist

This will give you a python package in dist/, which you can install using "pip install"

Base client module
------------------

.. automodule:: python_freeipa.client
    :members:
    :undoc-members:

Autogenerated client module
---------------------------

.. automodule:: python_freeipa.client_meta
    :members:
    :undoc-members:

Exceptions module
-----------------

.. automodule:: python_freeipa.exceptions
    :members:

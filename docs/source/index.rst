GraphQL API Gateway
===================

GraphQL API gateway for interacting with microservices.

Dependencies
============
Install the appropriate software:

1. [Docker Desktop](https://www.docker.com).
2. [Git](https://github.com/git-guides/install-git).
3. [PyCharm](https://www.jetbrains.com/ru-ru/pycharm/download) (optional).


Installing
==========
Clone the repository to your computer:
    .. code-block::console
    git clone https://github.com/mnv/python-course-graphql-gateway

1. Copy `.env.sample` into the `.env` file to customize the application:
    .. code-block::console
    cp .env.sample .env

This file contains environment variables whose values will be shared by the whole application.
The sample file (`.env.sample`) contains a set of variables with default values.
Therefore, it can be customized depending on the environment.

2. Build the container using Docker Compose:
    .. code-block::console
    docker compose build

This command must be run from the root directory where `Dockerfile` is located.
You also need to rebuild the docker container in case you have updated the `requirements.txt`.

3. To run a project inside the Docker container:
    .. code-block::console
    docker compose up

4. Perform migrations inside the database:

   Perform database migrations of favorite locations:
    .. code-block::console
    docker compose run favorite-places-app alembic upgrade head
   Perform migrations of the country directory database:
    .. code-block::console
    docker compose run countries-informer-app bash

   Run inside a container:
    .. code-block::console
    ./manage.py

When the containers are up, the server runs at [http://0.0.0.0:8000/graphql](http://0.0.0.0:8000/graphql). You can open it in your browser.


Using
=====
An example query to request a list of favorite places with pagination:
.. code-block::graphql
query {
  places (page: 1, size: 2) {
    latitude
    longitude
    description
    city
    locality
  }
}

An example query to get a list of favorite places with country information:
.. code-block::graphql
query {
  places {
    latitude
    longitude
    description
    city
    locality
    country {
      name
      capital
      alpha2code
      alpha3code
      capital
      region
      subregion
      population
      latitude
      longitude
      demonym
      area
      numericCode
      flag
      currencies
      languages
    }
  }
}

Example query to get a specific favorite place:
.. code-block::graphql
{
  place(placeId:1) {
    id
    latitude
    longitude
    description
    city
    locality
  }
}


Example of a query to create a new favorite place:
.. code-block::graphql
mutation {
  createPlace (
    latitude: 25.20485,
    longitude: 55.27078,
    description: "Nice food."
  ) {
    place {
      id
      latitude
      longitude
      description
      city
      locality
    }
    result
  }
}

Example of a query to remove a particular favorite locality:
.. code-block::graphql
mutation {
  deletePlace(placeId: 1) {
    result
  }
}

Example query to update a favorite place:
.. code-block::graphql
mutation {
  updatePlace (
  	placeId: 1
    latitude: 25.20485,
    longitude: 55.27078,
    description: "Very nice food!"
  ) {
    place {
      id
      latitude
      longitude
      description
      city
      locality
    }
    result
  }
}

Automation
==========
The project contains a special `Makefile` which provides shortcuts to a set of commands:

1. Create a Docker container:
    .. code-block::console
    make build

2. Generate the Sphinx documentation:
    .. code-block::console
    make docs-html

3. auto-formatting the source code:
    .. code-block::console
    make format

4. Static analysis (linters):
    .. code-block::console
    make lint

5. Autotests:
    .. code-block::console
    make test

6. Run autoformat, linters and tests with one command:
    .. code-block::console
    make all


Documentation
=============

The project is integrated with the [Sphinx] documentation engine (https://www.sphinx-doc.org/en/master/).
It allows you to create documentation from source code.
So the source code must contain documentation in [reStructuredText](https://docutils.sourceforge.io/rst.html) format.

To create HTML documentation, run this command from the source directory where `Makefile` is located:
    .. code-block::console
    make docs-html


Once generated, the documentation can be opened from the `docs/build/html/index.html` file.

Models
------

.. automodule:: models.places
    :members:

.. automodule:: models.countries
    :members:

Services
-------

.. automodule:: services.places
    :members:

.. automodule:: services.countries
    :members:

Clients
------
.. automodule:: clients.places
    :members:

.. automodule:: clients.countries
    :members:

Scheme
-----

.. automodule:: schema
    :members:

Context
-------

.. automodule:: context
    :members:

Data loaders
------------

.. automodule:: dataloaders
    :members:

Settings
---------

.. automodule:: settings
    :members:

Translated with www.DeepL.com/Translator (free version)
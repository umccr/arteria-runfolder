Arteria-Runfolder
=================

A self contained (Tornado) REST service for managing runfolders.

The service will watch a directory structure containing Illumina runfolders,
and try to determine the state it is in by looking for the `RTAComplete.txt` file
which pops up once the sequencing is finished.

Currently supported states are:

    none      -> No RTAComplete.txt file available
    pending   -> An action is pending
    ready     -> Has found and RTAComplete.txt file
    started   -> Some type of processing is going on of this runfolder
    done      -> Processing has finished
    error     -> Some problem has been detected
    cancelled -> The process has been cancelled

All states can be set via posting to the API. E.g:

    curl -X POST --data '{"state": "started"}' http://localhost:9999/api/1.0/runfolders/path/</path/to/my_runfolder>

This means that the client (e.g. a workflow) is responsible for updating the state, and determining how to handle it.

**Try it out:**

Install using pip:

    pip install -r requirements/dev . # possible add -e if you're in development mode.

Open up the `config/app.config` and specify the root directories that you want to monitor for runfolders. Then run:

    runfolder-ws --port 9999 --configroot config/

This will star the runfolder service on port 9999, and the api dock will be available under `localhost:9999/api`.
Try curl-ing to see what you can do with it:

    curl localhost:9999/api

**Running the tests**
After install you could run the integration tests to see if everything works as expected:
    ./runfolder_tests/run_integration_tests.py

This will by default start a local server, run the integration tests on it and then shut the server down.

Alternatively, you can run the same script against a remote server, specifying the URL and the runfolder directory:
    ./runfolder_tests/run_integration_tests.py http://testarteria1:10800/api/1.0 /data/testartera1/runfolders

Unit tests can be run with
    nosetests ./runfolder_tests/unit

**Install in production**
One way to install this as a daemon in a production environment
can be seen at https://github.com/arteria-project/arteria-provisioning

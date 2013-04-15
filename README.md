Running StrongLoop Node on a PaaS
=================================
This sample application is a quickstart to get StrongLoop Node running
on a PaaS. The current PaaS environments, this quickstart deploys to are
SalesForce's Heroku and Red Hat's OpenShift.
VMWare's CloudFoundry is on its way - stay tuned!

Selecting a StrongLoop Node version to use/install
--------------------------------------------------

To select the version of Node.js that you want to run, just edit or add
a version to the strongloop/VERSION file.

    Example: To install StrongLoop Node version 1.0.0-0.2.beta
    echo -e "1.0.0-0.2.beta\n" >> strongloop/VERSION

The platform specific installers in the StrongLoop buildpack will use that
VERSION file to download and extract the specific StrongLoop Node version
if it is available and automatically setup the paths to use the node/npm
binaries from the specific install directory.

Okay, now onto how you can get a StrongLoop supported Node.js version
running on different PaaS environments.


Selecting the application's Deployment Mode
-------------------------------------------
To select the deployment mode, just edit or add the environment to your
application in a file named strongloop/NODE_ENV

    Example: To run in production mode
    echo -e "production\n" >> strongloop/NODE_ENV

The platform specific installers in the StrongLoop buildpack will use that
NODE_ENV file and set the environment for npm to install and run
your application.

Okay, now onto how you can get a StrongLoop supported Node.js version
running on the different PaaS environments.


Deploying on Heroku:
--------------------
On Heroku, the simplest method is to first optionally edit the config
files as shown above, then create an app using the StrongLoop buildpack
and finally push to your Heroku app. That's the tl;dr version - detailed
steps below.

Create an account on http://heroku.com/

Install the Heroku toolbelt

    See: https://toolbelt.heroku.com/ for instructions

Login into the Heroku:

    heroku login

Clone this sample application

    git clone git://github.com/ramr/strongloop-paas-quickstart.git dynode

Change directory to your application and optionally edit the StrongLoop
configuration if you wish to and commit those changes.

    cd dynode
    # vi/edit strongloop/*
    git add strongloop
    git commit . -m 'Added StrongLoop config files'

Install the required packages and optionally lock 'em down:

    npm install
    npm shrinkwrap

You can optionally run the application locally by just running:

    npm start

And when you are satisfied that all's ok, just push your app to Heroku:

    #  Create an app with this buildpack and push to it.
    heroku apps:create -b git://github.com/ramr/strongloop-buildpack.git
    git push heroku master

This will now download and configure StrongLoop Node on Heroku, install
the dependencies as specified in the sample application's package.json
file (or npm-shrinkwrap.json if one exists).

That's it, you can now checkout your StrongLoop Node application at the
app url/domain returned from the heroku apps:create command.

    Example:  http://whispering-coast-1234.herokuapp.com/


Deploying on OpenShift:
-----------------------

Create an account at http://openshift.redhat.com/

Create a namespace, if you haven't already do so

    rhc domain create <yournamespace>

Create a nodejs-0.6 application (you can name it anything via -a)

    rhc app create -a slnode -t nodejs-0.6

Add this `github strongloop-paas-quickstart` repository

    cd slnode
    git remote add upstream -m master git@github.com:ramr/strongloop-paas-quickstart.git
    git pull -s recursive -X theirs upstream master

Then push the repo to OpenShift

    git push

That's it, you can now checkout your application at:

    http://slnode-$yournamespace.rhcloud.com


*work-in-progress*

Deploying on CloudFoundry:
--------------------------

Create an account on http://cloudfoundry.com/

Install the vmc command line tools

     See: http://docs.cloudfoundry.com/tools/vmc/installing-vmc.html

Target the CloudFoundry PaaS:

     vmc target https://api.cloudfoundry.com

Login into the CloudFoundry PaaS:

     vmc login

Clone this quickstart:

    git clone git://github.com/ramr/strongloop-paas-quickstart.git dynode

Install the required packages and optionally lock 'em down:

    npm install
    npm shrinkwrap

You can optionally run the application locally by just running:

    npm start

And when you are satisfied or then since this is a PaaS specific
quickstart, just push to CloudFoundry:

    vmc push

Note:  The first time you run vmc push, this will create a new application
       at CloudFoundry and you will need to specify the app name
       `example: slnode-app`, number of instances `example: 1`,
       framework `node`, runtime `node08` (yeah bit klunky as the PaaS
       environments have older versions), memory `example: 256M`,
       domain `example: slnode-app.cloudfoundry.com`,
       any other services `example: n`.
       And save the configuration `example: y`.

This will now download and configure StrongLoop Node on CloudFoundry,
install the dependencies as specified in the sample application's
package.json file (or npm-shrinkwrap.json if one exists).

That's it, you can now checkout your StrongLoop Node application at the
app url/domain you set for your CloudFoundry app.

    Example:  http://slnode-app.cloudfoundry.com/



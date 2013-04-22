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

Install the required packages and optionally lock 'em down (set 'in-stone'
the versions of the dependent packages you want to use):

    npm install
    npm shrinkwrap

You can also run the application locally via:

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

Change directory to your application and optionally edit the StrongLoop
configuration if you wish to and commit those changes.

    cd dynode
    # vi/edit strongloop/*
    git add strongloop
    git commit . -m 'Added StrongLoop config files'

Then push the repo to OpenShift

    git push

That's it, you can now checkout your application at:

    http://slnode-$yournamespace.rhcloud.com


Deploying on CloudFoundry:
--------------------------

Create an account on http://cloudfoundry.com/

For now, you will also need to register for the test/beta env @

    http://console.a1.cf-app.com/register 


Install the cf command line tools

     sudo gem install cf --pre

     #  If you run into issues w/ dependencies if you have the older vmc
     #  tools installed, then try uninstalling the vmc tools + other
     #  gems and run something like:
     #  sudo gem uninstall vmc cf
     #  sudo gem uninstall tunnel-cf-plugin cf-uaa-lib manifests-cf-plugin
     #  sudo gem uninstall caldecott-client cfoundry
     #
     #  sudo gem install cfoundry
     #  sudo gem install cf --pre
     #  sudo gem install cfoundry --pre
     #  sudo gem install manifests-cf-plugin --pre

     See: http://docs.cloudfoundry.com/tools/vmc/installing-vmc.html

Login into the CloudFoundry PaaS:

     cf login

Target the CloudFoundry PaaS:

     #  Normally, this would just be:
     #  cf target https://api.cloudfoundry.com

     #  But for the buildpack support, you will need to target the
     #  test/beta environment instead.
     cf target api.a1.cf-app.com

Clone this quickstart:

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

And when you are satisfied that all's ok, push your app to CloudFoundry.

    cf push dynode
      --buildpack=git://github.com/ramr/strongloop-buildpack.git
      --no-create-services --instances 1 --memory 128M

Note:  The first time you run vmc push, you will need to specify all the
       parameters - this will create a new application at CloudFoundry.
       domain `example: slnode-app.cloudfoundry.com`.
       And save the configuration `example: y`.

Subsequent pushes just need the `cf push` command - you only need to
specify all those options the first time you create the app (push).

This will now download and configure StrongLoop Node on CloudFoundry,
install the dependencies as specified in the sample application's
package.json file (or npm-shrinkwrap.json if one exists).

That's it, you can now checkout your StrongLoop Node application at the
app url/domain you set for your CloudFoundry app.

    Example:  http://dynode.cloudfoundry.com/



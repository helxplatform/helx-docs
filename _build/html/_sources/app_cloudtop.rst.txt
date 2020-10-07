########
CloudTop
########

On this page you will find guides for testing the CloudTop Docker, the
CloudTop OHIF Docker, and the CloudTop ImageJ/Napari Docker.

===========================
Testing the CloudTop Docker
===========================

Begin by starting the App as described in the section Creating an
Application_. Select the CloudTop Viewer application.

.. _Application: https://helx-10.readthedocs.io/en/latest/app_create.html?highlight=create%20an%20application

**Step 1:** Run the CloudTop Docker
::

    docker run -p8080:8080 -e USER_NAME=howard -e VNC_PW=test heliumdatastage/cloudtop:latest 

``USER_NAME`` and ``VNC_PW`` can be whatever you want: those are the
authentication info you will need to log in for Step 2. Change the tag
to whichever tag you want to test.

**Step 2:** Connect to the running docker 

-  Browse to ``localhost:8080``
-  Enter the ``USERNAME`` and ``VNC_PW`` you specified when starting the
``Dockerfile`` 
-  Press the Login button 
-  Wait for Guacamole to respond

**Step 3:** Make sure the home directory is OK

-  Start a terminal emulator
from the applications menu. In the resultant shell, type

::

    echo $HOME

-  You should see ``/home/USER_NAME`` where ``USER_NAME`` is the user
   name specified in
-  Note the presence of the Firefox browser icon

At this point the basic CloudTop functionality is working.

Testing the CloudTop OHIF Docker
--------------------------------

**Step 1:** Run the CloudTop OHIF docker

::

    docker run -p3000:3000 -p8080:8080 -e
    "CLIENT_ID=OUR_CLIENT_ID.apps.googleusercontent.com" -e
    USER_NAME=howard -e VNC_PW=test heliumdatastage/cloudtop-
    ohif:latest

where ``OUR_CLIENT_ID`` is found in the file.

``google_health.env`` file in the renci\_data\_stage directory of our
keybase account. ``USER_NAME`` and ``VNC_PW`` can be whatever you want:
those are the authentication info you will need to log in for Step 2.
Change the tag to whichever tag you want to test.

**Step 2:** Connect to the running docker 

-  Browse to ``localhost:8080``
-  Enter the ``USERNAME`` and ``VNC_PW`` you specified when starting the
Dockerfile 
-  Press the Login button - Wait for Guacamole to respond

**Step 3:** Make sure the home directory is OK 

-  Start a terminal
emulator from the applications menu. In the resultant shell, type:

::

    echo $HOME

-  You should see ``/home/USER_NAME`` where ``USER_NAME`` is the user
   name specified in Step 1
-  Note the presence of the Firefox browser icon

At this point the basic CloudTop functionality is working.

**Step 4:** Test the OHIF functionality 

-  Exit the terminal emulator by typing ``exit`` 
-  Click the Firefox icon and browse to ``localhost:3000`` 
-  At this point you will be prompted for your Google user ID. 
-  Click Next. 
-  Google may prompt you to choose the account you wish to proceed with. If prompted, pick your G Suite account. 
-  Click Next. 
-  Enter your password. The browser will ask if you want to save the password. It doesn’t matter if you do or not 
-  Respond to the 2 step authentication. If you haven't used it before, you may be prompted to set up the 2 step authentication. 
-  You should now see the basic OHIF screen with a large selection of projects.

**Step 5:** Browse to Your Data Set 

-  Select helx-dev 
-  Select the northamerica- northeast1 region 
-  Select the DicomTestData dataset 
-  Select the TestData Dicom Store 
-  You should now see our test datasets.
Chose your test data set and have fun!

Testing the CloudTop ImageJ/Napari Docker
-----------------------------------------

**Step 1:** Start the Docker 

-  Start the docker with the following
command:

::

    docker run -p8080:8080 -e USER_NAME=howard -e VNC_PW=test
    heliumdatastage/cloudtop-image-napari:latest

where ``USER_NAME`` and ``VNC_PW`` can be whatever you want: those are
the authentication info you will need to log in for Step 2. Change the
tag to whichever tag you want to test.

**Step 2:** Connect to the running docker 

-  Browse to ``localhost:8080``
-  Enter the ``USERNAME`` and ``VNC_PW`` you specified when starting the
Dockerfile 
-  Press the Login button 
-  Wait for Guacamole to respond

**Step 3:** Make sure the home directory is OK

-  Start a terminal
emulator from the applications menu. In the resultant shell, type:

::

    echo $HOME

-  You should see ``/home/USER_NAME`` where ``USER_NAME`` is the user
   name specified in Step 1
-  Note the presence of the ImageJ, Napari and Firefox browser icon.
   **If any are missing the test fails.**
-  At this point the basic CloudTop functionality is working. Next we
   will want to verify that ImageJ and Napari are working

**Step 4:** Make sure the ImageJ application launcher works correctly

-  Exit the terminal application and click the ImageJ icon. There is no ImageJ test data included in the docker. 
-  Exit ImageJ and make sure the Napari application launcher works correctly. 
-  The docker does not contain any test data. The docker test is now complete. 
-  Exit Napari and stop the docker.

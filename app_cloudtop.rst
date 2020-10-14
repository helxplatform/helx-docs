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

1. Browse to ``localhost:8080``
2. Enter the ``USERNAME`` and ``VNC_PW`` you specified when starting the
``Dockerfile`` 
3. Press the Login button 
4. Wait for Guacamole to respond

**Step 3:** Make sure the home directory is OK

1. Start a terminal emulator
from the applications menu. In the resultant shell, type

::

    echo $HOME

1. You should see ``/home/USER_NAME`` where ``USER_NAME`` is the user
   name specified in
2. Note the presence of the Firefox browser icon

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

1. Browse to ``localhost:8080``
2. Enter the ``USERNAME`` and ``VNC_PW`` you specified when starting the
Dockerfile 
3. Press the Login button - Wait for Guacamole to respond

**Step 3:** Make sure the home directory is OK 

1. Start a terminal
emulator from the applications menu. In the resultant shell, type:

::

    echo $HOME

2. You should see ``/home/USER_NAME`` where ``USER_NAME`` is the user
   name specified in Step 1
3. Note the presence of the Firefox browser icon

At this point the basic CloudTop functionality is working.

**Step 4:** Test the OHIF functionality 

1. Exit the terminal emulator by typing ``exit`` 
2. Click the Firefox icon and browse to ``localhost:3000`` 
3. At this point you will be prompted for your Google user ID. 
4. Click Next. 
5. Google may prompt you to choose the account you wish to proceed with. If prompted, pick your G Suite account. 
6. Click Next. 
7. Enter your password. The browser will ask if you want to save the password. It doesn’t matter if you do or not 
8. Respond to the 2 step authentication. If you haven't used it before, you may be prompted to set up the 2 step authentication. 
9. You should now see the basic OHIF screen with a large selection of projects.

**Step 5:** Browse to Your Data Set 

1. Select helx-dev 
2. Select the northamerica- northeast1 region 
3. Select the DicomTestData dataset 
4. Select the TestData Dicom Store 
5. You should now see our test datasets.

Chose your test data set and have fun!

Testing the CloudTop ImageJ/Napari Docker
-----------------------------------------

**Step 1:** Start the Docker 

1. Start the docker with the following
command:

::

    docker run -p8080:8080 -e USER_NAME=howard -e VNC_PW=test
    heliumdatastage/cloudtop-image-napari:latest

where ``USER_NAME`` and ``VNC_PW`` can be whatever you want: those are
the authentication info you will need to log in for Step 2. Change the
tag to whichever tag you want to test.

**Step 2:** Connect to the running docker 

1. Browse to ``localhost:8080``
2. Enter the ``USERNAME`` and ``VNC_PW`` you specified when starting the
Dockerfile 
3. Press the Login button 
4. Wait for Guacamole to respond

**Step 3:** Make sure the home directory is OK

1. Start a terminal
emulator from the applications menu. In the resultant shell, type:

::

    echo $HOME

2. You should see ``/home/USER_NAME`` where ``USER_NAME`` is the user
   name specified in Step 1
3. Note the presence of the ImageJ, Napari and Firefox browser icon.
   **If any are missing the test fails.**
4. At this point the basic CloudTop functionality is working. Next we
   will want to verify that ImageJ and Napari are working

**Step 4:** Make sure the ImageJ application launcher works correctly

1. Exit the terminal application and click the ImageJ icon. There is no ImageJ test data included in the docker. 
2. Exit ImageJ and make sure the Napari application launcher works correctly. 
3. The docker does not contain any test data. The docker test is now complete. 
4. Exit Napari and stop the docker.

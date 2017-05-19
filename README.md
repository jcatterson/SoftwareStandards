# Sofie Bio Software Standards

## Intro to Software Development Process

This section outlines the life-cycle of the software development process. What is the SOP for receiving/addressing these request changes?

### Change Request

#### Customer Request
TBD

#### Internal Request
Teamwork Projects will be used to create and track SW/UI issues. The following steps will allow the user to create a new
Task and provide the necessary details/images to explain the issue.

##### Reporting In Teamwork Project
1. Login to the SOFIE Teamwork Projects website.
2. Go to the “SW/UI” project within the ELIXYS program.
3. Go to the Task Lists within the "SW/UI" project.
4. Create a Task for the bug/issue in the Task List named “SW/UI Issue Reporting”.
   1. Who and When tab
       1. What needs to be done?
          * Create a brief description
       2. Who should do this?
          * Assign the appropriate person (Alex C. or Justin)
              1. This defines the Owner of the task.
          * Make sure the checkbox for “Notify by Email” is checked and green in color.
          * Start Date and Due Date – Do not fill this out.
   2. Description tab
      1. Write a detailed description of the issue.
   3. Files Tab
      1. Attach any useful logs, data, or images
   4. Priority
      1. Select a priority level based on the severity of the bug/issue.
   5. Followers
      1. Select your name if you wish to follow the progress on this issue.
      
#### Reviewing and assigning priority
1. The Owner of the Task should receive an email alert when a bug/issue task is created.
2. The Owner should review all the pertinent information input into the Task.
    1. Review the Description and any attached Files.
    2. Review the Priority level.
    3. Communicate with the team to make sure all information is complete and the Priority level is appropriate.
3. Update the Priority level and define a Due Date and create a SFDC Case #.
    1. Based on an assessment of the issue, the Owner should edit the Task and assign the appropriate Priority level.
    2. The Owner should enter a Start date and target Due Date for the task.
    3. The Owner should create a SFDC Case # for this task.
4. The Owner should move the Task (issue) into the appropriate Task List.

#### Bug/Feature vs Milestone
##### It's not quite clear to me how you will take a Change Request (from Teamworks or Customer) and decide what Github Milestone it will live under
What's the process to assess a request?

Weekly stand-up will decide

During weekly stand-ups each new Feature/Bug shall answer the following questions:
 1. Without the code change, does the system still run as designed?
     1. Is there a way to remedy the situation without any changes?
 2. Why are we doing this?  Does the change make sense vs the risk/cost of development?
 3. What else will be effected by making this change? [Does UI change, are there multiple places where change will be necessary?]
 4. Will other customers agree the change is necessary and desired?
     1. How can we please all of our customer base?  Perhaps this is a configuration to turn on/off?  Some choice via the UI?
 5. Will older sequences still work and run the same? 
 6. How complex is the change? [Low, Medium, High]
 7. What is the criteria to pass?


#### Github Milestone SOP
Milestones will be directly related to Elixys releases.  Think if a milestone exists; it is either an up-comming release or a release that has been already made.
It is expected Elixys will release new revisions of the software quarterly; however one-off bug fixes may also be made.  Titles shall be named in the format of 'pyelixys_vx.y.z'.  The title will need to match the release title [See Deployment SOP] in order to tie all changes to a specific Software Version.

Over-arching Large Projects shall increment the 'x' digit from the version number.  This will indicate a very large software change.
Quarterly Revisions shall increment the 'y' digit from the version number and '0' our the 'z' digit
Bug Fixes shall increment the 'z' digit.

To Create a Milestone
1. Select the Issue Tab from your project
2. Select Milestone from within the Issue Tab
3. Select New Milestone
    1.  Fill in the title based on the requirements stated above
    2.  Ignore the Description field
    3.  Select the due date

Immediatly; branch from master.  This shall indicate the 'next_release' that is defined in the section 'Working on Completing the task'
```
git checkout master
git pull
git checkout -b pyelixys_vx.y.z
# Edit src/pyelixys_config.txt, src/pyelixys/hal/templates/hwconf_simulator.ini, src/pyelixys/hal/templates/hwconf_hardware.ini
# Increment the order, and Version in the files
git push --set-upstream origin ticket_name
git commit -m"Creating branch for the next version of Elixys"
git push
```

#### Creating Github Ticket
Once it has been determined that this change requires a code change; a Github ticket shall be created.  All tickets shall be tied to a milestone.
1. Visit the Issue tab of the Project
2. Select [New Issue](https://github.com/SofieBiosciences/Elixys/issues/new)
    1. Select Assignees
    2. Select Milestone
        1.  If this change will be part of our quarterly schedule select from our Quarterly Milestone
        2.  If this change is immediately required [bug fix]; create a new milestone, follow the Github Milestone creation SOP.
    3. Fill in an appropriate title [Reference the teamwork task id found in the URL https://sofiebiosciences.teamwork.com/#tasks/**4753296**] **Title Description - 4753296**
    4. Copy the comment from the teamwork task. Reference the teamwork URL in comment
    5. Submit new Issue
    6. Once submitted, the developer shall note all sections that will be affected due to the request change.  It shall be noted in the comment section of github and brought up during Field Service meetings.

#### Working on Completing the task
1.  Create a branch from master on github.  Mention the git ticket name in the bug fix
```
git checkout master
git pull
git checkout -b ticket_name
git push --set-upstream origin ticket_name
git commit -m"Working on ticket #xyz"
git push
```
2. Follow procedure for the type of ticket (C++, python, javascript, css/scss) [See Categorizing Issues & Ticket Types]
3. Once the change has been made, close the github ticket
```
git add all_files_changed
git commit -m"This closes #34, closes #23"
git push
```
4. Validate change has satisfied the creator of the ticket.
    1. Visit the teamwork task id https://sofiebiosciences.teamwork.com/#tasks/**4753296**
    2. Comment to the task creator the fix is ready.  Have the creator validate it fixes their issue.
5. After validating the fix; update src/pyelixys_config.txt
6. Once validated and all tests pass, merge change back to the next release branch
7. Post a final comment on the issue stating how the fix was validated
```
git checkout next_release
git pull
git merge ticket_name
git commit -m"Merging #xyz into the next release"
git push
```
8.  Follow the deployment SOP, note this is a pre-release.

#### Deploying the next version
Deploying changes to production will depend on the project.  This document outlines the deployment process for Elixys, as it is our main product.  For SPN deployment procedures, follow Heroku Deployments section in the [SPN README](https://github.com/SofieBiosciences/SPN/blob/master/README.md)

SPN will host the most stable version of Elixys.  To view the version list go to https://www.sofienetwork.com/github/releases/.  Internal users may use: https://www.staging-spn.com/github/releases/ to look at all the pre-releases.
You must have a username and password to SPN in order to access this portion of site.  Without SPN credentials, you may still download the installer and get the latest version of Elixys.

Assuming you are on a Windows OS
1. http://sofienetwork.com/github/latests_installer_exe?os=windows
2. Once downloaded, open the exe.
3. From the toolbar select Elixys->Get Latest
4. Close the installer
5. Connect to the Elixys WIFI
6. Re-Open the installer
7. Wait for Installer to connect to your Elixys.  See the Tower PC Icon
8. Once connected, select Upload
9. Select elixys.zip
10. Confirm upload version
11. Once completed, restart the control box
12. Validate the new version of Elixys is running


#### Deployment SOP
To make a version available for download on SPN
1. git checkout next_release # or branch
2. git pull
3. git checkout master # or next_release
#Modify src/pyelixys_config.txt, src/pyelixys/hal/templates/hwconf_*.ini
4. git merge branch_to_deploy
5. git commit -m""
6. re-run all tests
7. ./osx_installer.sh [run from the target controlbox]
8. Draft a new release https://github.com/SofieBiosciences/Elixys/releases/new
    1. Tag Version: vx.y.z @ target: master or next_release
    2. Release Version: pyelixys_vx.y.z
    3. Copy src/pyelixys_config.txt Change_Logs section into the release notes section
    4. Validate src/pyelixys_config.txt, src/pyelixys/hal/templates/hwconf_simulator.ini, src/pyelixys/hal/templates/hwconf_hardware.ini.  All have the appropriate version number tied to the milestone being deployed!
    5. Drag & Drop this zip file created from ./osx_installer.sh into the binary section [Should look like linux_pyelixys_vx.y.z.zip]
    6. Publish Release [If this is a pre-release [target @ next_release], mark it so]
    7. Validate https://www.sofienetwork.com/github/releases/ displays the option for your new version.  Validate selecting that version prompts a selection of LINUX
    8. Close the Milestone tied to this release.  If issues are still open, tied to this milestone; validate they are still open issues, and move them to the next milestone to be released
    
Once completed, execute the following script to automatically email all users
```
# To be dev'd
```
**To Be Developed**
It shall contain
1. the version number recently released
2. The release notes
3. Blurb stating only the latests version is supported; so we suggest keeping the version up-to-date
4. Instructions on how to get the latest version

    
### Releasing new software versions to all users
1. How should users be notified of a SW release?
    1. All SPN users shall be notified after a release.  Please see the Deployment SOP
2. All software releases should include a short description of the issue fixes and changes.
    1. Please see the Deployment SOP for release notes
3. Will users have access to previous versions of the software?
    1. Yes, via SPN. Please see the Deployment SOP for more information

### Categorizing Issues & Ticket Types

#### Python
Python is the center-point of Elixys.  It controls how each sequence operation functions [timing, turning on/off valves, record creation],and it is the layer that does all communication between the User Interface and the Firmware.  If a code-change is around a business rule; think this is the layer that will need to change.

Pyelixs changes will be written in a TDD format [Test Driven Development].
This means, prior to source code being developed; a test must be written to validate it is working against the simulator.  This means, immediatly after writing the test, we would expect failure.  Develop the source code afterwards to make a passing test.  For more details on developing Pyelixys tests visit the [README](https://github.com/SofieBiosciences/Elixys/blob/master/src/test/README.md)


#### C++
C++ contains all the low-level controls for the hardware.  This layer should very seldomly be changed once hardware revisions has been finalized.


The firmware will be validated by executing all of the commands in benchmark directory of Elixys

#### Javascript
Javascript contains the workflow of the User Interface.  This contains rules such as, what the user will be able to control and what will be displayed next to the user.

#### CSS/SCSS HTML
CSS/SCSS and HTML is used to visually display the controls and data to the user.


### Retrieving Deployed Software Revision Changes/History

All tickets created for Elixys are accessable via the URL
www.sofienetwork.com/github/issue_history/.  This will export the a CSV containing when/if the issue was closed and what milestone/release the issue was fixed in.

Users shall be able to select on the downloadable options and see the release notes after selecting a specified version of Elixys.

A database (e.g. – spreadsheet) will be maintained which documents each issue’s history, from request to rectification.
Case numbers should be line items in the database
This document should track things like:
1. Case #
    1. Teamworks case number
    2. Github number
2. Date issue discovered/reported
3. Date issue resolved
4. Description of issue
5. How issue was fixed
    1. Validation method and result
6. What revision this was fixed in
7. Once we decide that it’s time to release a new version of the software, these issues get frozen and we generate a document showing all changes that were implemented in version X to version Y.




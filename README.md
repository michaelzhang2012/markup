Yeti User Manual
================

What is Yeti
------------
Yeti stands for "Yet Extraordinary Test Infrastructure".
<br>It is rspec-based basic validation test
<br>*Under development*

This repository contains tests for [vcap](https://github.com/cloudfoundry/vcap).

## Dependencies
RVM (Ruby Version Manager) is used to manage different ruby version on unix-based box
<br>please follow the guideline, https://rvm.io/ to install RVM on your box.

## _Supported Operation System_
1. Mac OS X 64bit, 10.6 and above
2. Ubuntu 10.04 LTS 64bit

How to run it
-------------
1. ```gerrit-clone ssh://<YOUR-NAME>@reviews.cloudfoundry.org:29418/vcap-yeti```
2. ```cd vcap-yeti```
3. ```./update.sh      ## if run admin cases, this step can be skipped```
4. ```bundle exec rake tests```
5. At first time, yeti will ask you several questions about
    - target
    - test user/test passwd
    - admin user/admin passwd
   <br>on Terminal interactively.
   <br>And save those information into ~/.bvt/config.yml file.
   <br>Therefore, when running again, yeit will never ask those questions again.

Note:
-----
1. And to be compatible with BVT, environment variables are still available in yeti.
```
||Environment Variables    ||Function                  ||Example                                ||
|VCAP_BVT_TARGET           |Declare target environment |VCAP_BVT_TARGET=cloudfoundry.com         |
|VCAP_BVT_USER             |Declare test user          |VCAP_BVT_USER=pxie@vmware.com            |
|VCAP_BVT_USER_PASSWD      |Declare test user password |VCAP_BVT_USER_PASSWD=<MY-PASSWORD>       |
|VCAP_BVT_ADMIN_USER       |Declare admin user         |VCAP_BVT_ADMIN_USER=admin@admin.com      |
|VCAP_BVT_ADMIN_USER_PASSWD|Declare admin user password|VCAP_BVT_ADMIN_USER_PASSWD=<ADMIN-PASSWD>|
```

2. In order to support administrative test case, yeti will ask admin user/admin passwd information.
   <br>However, yeti will not abuse administrative privilege for every operation,
   <br>just list users, create normal user, delete normal user which created in test script.
3. Currently yeti run in serial, you could ```tail -f ~/.bvt/bvt.log``` to get what is going on

FAQ:
----
1. what does "pending" mean and what is the correct number of pending cases that i should see?
   <br>A: "pending" means your target environment misses some preconditions,
      usually a service, framework or runtime.
      <br>The number of pending cases denpends on your target environment and environment variables.
      - For example, postgreSQL service is not available on dev_setup environment. Therefore,
      user run yeti against dev_setup environment, there should be some pending cases related
      to PostgreSQL service, and prompt message like "postgresql service is not available on
      target environment, #{url}"
      - On the other hand, mysql service should be available on dev_setup environment. When user
      run yeti against dev_setup, and get some pending message like "mysql service is not
      available on target environment, #{url}". That should be a problem.

2. What is done in "./update.sh"?
   <br>A: Yeti has uploaded all precompiled java apps onto one blobs server. Therefore,
      <br>Yeti users do not need maven, and build java-based apps locally, just need to sync
      precompiled JAR/WAR files from blobs.cloudfoundry.com.
      <br>This action has been done by ./update.sh script automatically, so Yeti users need to
      run ```./update.sh``` script before running Yeti tests

3. What is example?
   <br>A: Example is the conception in RSpec. It is entire test unit, and it will has one result,
      Pass/Failure/Pending.

4. Where are binary assets stored?
   <br>A: Binary assets are stored in blobs.cloudfoundry.com, which is simple Ruby/Sinatra application
      with Mongodb service

5. How do I submit binary assets?
   <br>A: There are two roles in Yeti project, one is Yeti User, the other is Yeti DEV.
      - Yeti user just run yeti scripts against any target environment.
      - Yeti DEV develop yeti scripts
      <br>Currently only Yeti DEV can submit binary assets

6. Where is the log file stored?
   <br>A: There two level logs, runtime logs and error logs
      - Runtime log is stored in <YOUR-USER-HOME>/.bvt/bvt.log
      - Error log is stored in <YOUR-USER-HOME>/.bvt/error.log
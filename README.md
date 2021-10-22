# jaws
Jira Automated Web Scripter

./jaws -j example.yml

example.yml is an ansible-playbook inspired yaml job file. Jaws will read and run, in turn, each of the defined tasks.

## Job Sessions

The script creates a session (authenticated or not) in order to run the tasks evaluated therein.

A Job must have:
* `stub` which serves as the base to which task `uri` are joined with a '/'.
* `tasks` which is a list of tasks to be run in the session.

A Job _may_ have:
* `authentication`
  * `username`
  * `password`
* `verbose` [ True | False ]

If the `authentiation` key is present in the job you will potentially be prompted for a username and password.

## Tasks

a task must have:
* `uri`
* `name`
* `req`. 

if any of there are not present, the script will bail.

a task _may_ have:
* `stub_override`  // a URL that will override the job stub ad hoc.
* `data`
* `output`
  * `type` [ print | store ]
  * `dest` // store output in this file
  * `show` [ content | status_code | headers ] // print the content or status_code of the current task

## Documentation

* https://docs.atlassian.com/software/jira/docs/api/REST/8.13.12/

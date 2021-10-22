# jaws
Jira Automated Web Scripter

./jaws -j job.yml

job.yml is an ansible-playbook styled yaml job file which will run, in turn, each of the defined tasks.

a task must have a `uri`, `name`, and `req`. if not the script will bail.
a task _may_ have `data` and an `action`.

The action for a task may be `print` or `store`.

## Documentation
* https://docs.atlassian.com/software/jira/docs/api/REST/8.13.12/

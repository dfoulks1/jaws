# warp
Warwalrux' Automated Rest Parser

./warp -j example.yml

example.yml is an ansible-playbook inspired yaml job file. Jaws will read and run, in turn, each of the defined tasks.

## Job Sessions

The script creates a session (authenticated or not) in order to run the tasks evaluated therein.

A Job **must** have:
* `stub` which serves as the base to which task `uri` are joined with a '/'.
* `tasks` which is a list of tasks to be run in the session.

A Job _may_ have:
* `authentication`
  * `username`
  * `password`
* `verbose` [ True | False ]
* `vars`
* `headers`

### Authentication

If the `authentiation` key is present in the job but no `username` or `pasword` values are found, the script will interactively prompt for values.


### Certificates

If the `certs` key is present in the job, the script will attempt to add the following:
* `ca_bundle`   // ca bundle certificate
* `client`      // client certifcicate
* `key`         // public key

The script persist in using these settings for the entire job.

## Tasks

a task **must** have:
* `uri`
    The resource to be called for the task
* `name`
    The friendly display name for the task
* `req`. 
    The request type for the task (GET|POST|DELETE|PUT)

if any of there are not present, the script will bail.

a task _may_ have:
* `stub_override`
    a URL that will override the job stub ad hoc.
* `data`
    YAML formatted request payload
* `output`
  * `type` // ( print | store | var )
  * `name` // 
  * `show` [ content | status_code | headers | template ] // print the content or status_code of the current task

### Output

There are three options for output at this time:
* `print`
* `store`
* `var`

#### `print` example

Printing with a Jinja Template string
```
...
    output:
      type: "print"
      show: "template"
      template_str: "{% for item in items}{{ item.name }}{% endfor }"
```

Printing with a Jinja Template file
```
...
    output:
      type: "print"
      show: "template"
      template_file: "templates/example.j2"
```

#### `store` example
Values will be written to a file with name output["name"]. If no name is provided the script will interactively prompt for one. This method is useful for reporting
```
...
    output:
      type: "store"
      show: "content"
      name: "filename.txt"
```

#### `var` example

Values stored as variables can be used in later steps with jinja templating
```
...
    output:
      type: "var"
      show: "content"
      name: "tickets"
```


## Documentation

* https://docs.atlassian.com/software/jira/docs/api/REST/8.13.12/
* https://developer.atlassian.com/server/confluence/confluence-rest-api-examples/

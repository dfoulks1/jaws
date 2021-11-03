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
* `urlencode`   // set data to be urlencoded as the default payload format
* `loop`        // loop through the task.
  * `items`     // list of items to iterate through, may be jinja.
  * `iter_name` // name by which the current item in the iteration can be used. 
* `authentication`
  * `username`
  * `password`
* `verbose` [ True | False ]
* `vars`
* `headers`

### Authentication

If the `authentiation` key is present in the job but no `username` or `pasword` values are found, the script will interactively prompt for values.

The script persists in using these as the default credentials for the entire job.


### Certificates

If the `certs` key is present in the job, the script will attempt to add the following:
* `ca_bundle`   // ca bundle certificate
* `client`      // client certifcicate
* `key`         // public key

The script persist in using these settings for the entire job.

## Tasks

a task **must** have:
* `uri` // The resource to be called for the task
* `name` // The friendly display name for the task
* `action` // The action statement for the task

if any of there are not present, the script will bail.

a task _may_ have:
* `loop`    // iteration statement
* `urlencode` // urlencode payload data for _this_ task.
* `stub_override` // a URL that will override the job stub ad hoc.
* `data` // YAML formatted request payload
* `output`

### Loop
If the loop key is found in the task object the script will construct items via jinja interpretation. The output will be iterated through and can be referenced by iter_name

```
...
    loop:
      iter: "{% raw %}{% for issue in issues %}{{ issue.key }}{% endfor %}{% endraw %}"
      iter_name: "ticket"
```

This will allow you to use {% raw %}`{{ ticket }}`{% endraw $} as a valid variable for a task

### Output
Output is the directive for the output of the current task. There are three options for redirecting output at this time:
* `file`
* `screen`
* `var`

Content May be any one of:
* `template`    // directive to use a template.
  * additionally `template_file` or `template_str` must be provided
* `raw`         // an alias for output.content
* `content`     // response.content
* `headers`     // response.headers
* `status_code` // response.status_code

#### `screen` example

Printing stored variable a Jinja Template string
```
...
    output:
      write_to: "screen"
      content: "template"
      template_str: "{% for item in items}{{ item.name }}{% endfor }"
```

Printing stored variable with a Jinja Template file
```
...
    output:
      write_to: "screen"
      content: "template"
      template_file: "templates/example.j2"
```

Printing task output
```
...
    output:
      write_to: "screen"
```

#### `file` example
Values will be written to a file with name output["name"]. If no name is provided the script will interactively prompt for one. This method is useful for reporting
```
...
    output:
      write_to: "file"
      content: "content"
      name: "filename.txt"
```

#### `var` example

Values stored as variables can be used in later steps with jinja templating
```
...
    output:
      write_to: "var"
      content: "content"
      name: "tickets"
```

## Documentation

* https://docs.atlassian.com/software/jira/docs/api/REST/8.13.12/
* https://developer.atlassian.com/server/confluence/confluence-rest-api-examples/

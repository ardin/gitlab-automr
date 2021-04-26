# gitlab-automr

Script that allows execute merge request for assigned users.


```
gitlab-automr -h 
usage: gitlab-automr [-h] [--config CONFIG] [--debug] [--dry-run]

GitLab Auto Merge Request

optional arguments:
  -h, --help       show this help message and exit
  --config CONFIG  config file
  --debug
  --dry-run
```


## Configuration


You have two configuration methods:

* environment variables
* configuration file


### Config with environment variables

In simple cases, you can use environment variables.

* GITLAB_ADDRESS
* GITLAB_TOKEN
* GITLAB_PROJECT_NAME
* GITLAB_ALLOWED_USERNAMES
* GITLAB_ALLOWED_PATHS
* GITLAB_ASSIGNEE_USERNAME

**Example exports**

```
export GITLAB_ADDRESS='https://gitlab.com'
export GITLAB_TOKEN='secret'
export GITLAB_PROJECT_NAME='mygroup/myrepo'
export GITLAB_ALLOWED_USERNAMES='gitlab.user1 gitlab.user2'
export GITLAB_ALLOWED_PATHS='path1/.* path2/.*txt'
export GITLAB_ASSIGNEE_USERNAME='gitlab.username'
```

**Example invalid operation** 

```
gitlab-automr --dry-run
db*.myserver: add db config by johnny.bravo	
mygroup/myrepo!30528 opened 2021-05-24T13:38:23.951Z

    Skipping: not allowed path path3/letter.txt
```

* This means path3/letter.txt is not configured as allowed path


**Example proper operation** 

```
gitlab-automr --dry-run
db*.myserver: add db config by johnny.bravo	
mygroup/myrepo!30528 opened 2021-05-24T13:38:23.951Z

    Merged (DRY RUN MODE)
```

* in production mode, skip '--dry-run'


### Config file

In complex configurations, you can use config file

```
gitlab_automr:
  my_repo_one:
    address: https://gitlab.com
    token: GITLAB_TOKEN
    project_name: GROUP/REPO
    users:
      - johnny.bravo
      - james.smith
    allowed_paths:
      - my/repo/path/*
      - other/*/allowed/*/path
  my_repo_two:
    address: https://gitlab.com
    token: GITLAB_TOKEN
    project_name: GROUP/REPO
    users:
      - robert.johnson
      - michael.brown
    allowed_paths:
      - allowed/file/path/*
      - other/*.yml
```


#### Run
```
gitlab-automr --config gitlab-automr.conf
```



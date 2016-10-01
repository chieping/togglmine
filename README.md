# togglmine

This simple tool helps your daily inputs from [Toggl](https://toggl.com) to
Redmine time\_entry, searching for your working data stored in Toggl and
printing curl commands to store the data to Redmine.  You should review printed
commands and execute it.  You no longer have to input manually by Web browser.


## Installation

```
gem install togglmine
```

## Configuration

~/.togglmine.yaml

```
---
toggl:
  # apikey(required)
  apikey: 1234567890abcdef1234567890abcdef
  # user_agent(required): Application name or your email address.
  user_agent: chieping@example.com
  # workspace_id(required): Your workspace id. You can find it by accessing
  #   https://toggl.com/app/workspaces and choosing your workspace, then it's
  #   in your location bar.
  workspace_id: 1234567
redmine:
  # url(required)
  url: https://example.com/
  # insecure: If true, add --insecure (-k) option to curl command.
  insecure: false
  # authentication(required)
  authentication:
    # method: api_key is only available method.
    method: api_key
    # apikey(required)
    apikey: 1234567890abcdef1234567890abcdef12345678
# default_issue_id: This is used when Toggl entry's description doesn't contain
#   any number. If it contain number, the number is used for issue id.
default_issue_id: 8888
# tag_mapping: Map data between Toggl tag value and Redmine activity id.
tag_mapping:
  development:
    name: 開発作業
    id: 11
  deployment:
    name: 配備作業
    id: 12
  review:
    name: レビュー
    id: 13
  chore:
    name: 雑作業
    id: 14
# default_activity_id: This id is used when "tag_mapping" can not resolve any
#   tag value to activity id.
default_activity_id: 14
```

## Usage

After setting up configuration, simply execute `togglmine`. Then curl command
will be printed.

```
$ togglmine
## sample task
curl -vX POST  -H 'Content-Type: application/xml' -H 'X-Redmine-API-Key: 1234567890abcdef1234567890abcdef12345678' https://example.com/time_entries.xml --data '
<?xml version="1.0" encoding="UTF-8"?>
<time_entry>
  <issue_id>8888</issue_id>
  <activity_id>14</activity_id>
  <spent_on>2016-10-01</spent_on>
  <hours>0.33</hours>
  <comments></comments>
</time_entry>
'
```

You can do with specific date like this:

```
$ togglmine 2016-10-01
## sample task
...
```

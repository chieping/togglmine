# togglmine

This simple tool helps your daily inputs from [Toggl](https://toggl.com) to
Redmine time\_entry, searching for your working data stored in Toggl and
printing _curl_ commands in order to store the data on Redmine.  You should
review printed commands and run it.  You no longer have to input manually by
Web browser.

## Installation

```
gem install togglmine
```

## Configuration

~/.togglmine.yaml

```yaml
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
# header: This string are printed on the top of the output. Useful if you open
#  the output by vim directly and want to use
#  [vim-quickrun](https://github.com/thinca/vim-quickrun) since Vim can guess
#  filetype and vim-quickrun do properly.
header: '#!/usr/bin/env bash'
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
  <activity_name>雑作業</activity_name>
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

Tip: Open by editor directly (Only zsh can use process substitution):

```
$ vim =(togglmine)

## or

$ togglmine | vim -
```

Tip: Auto detecting into _comments_:

Input some comment after `|`.

![toggl comment example](https://raw.githubusercontent.com/chieping/togglmine/master/asset/toggl_comment.png)

Then,

```
$ togglmine
## タスク #9772 Task name | blar blar
curl -vX POST  -H 'Content-Type: application/xml' -H 'X-Redmine-API-Key: 1234567890abcdef1234567890abcdef12345678' https://example.com/time_entries.xml --data '
<?xml version="1.0" encoding="UTF-8"?>
<time_entry>
  <issue_id>9772</issue_id>
  <activity_id>8</activity_id>
  <activity_name>雑作業</activity_name>
  <spent_on>2016-10-02</spent_on>
  <hours>0.3</hours>
  <comments>blar blar</comments>
</time_entry>
'
```

## todos

- Add a sub command to update `default_issue_id`.
- Add a syntax sugar to abbreviate pipe sign when Redmine ticket doesn't exist in the subject.
- Add a sub command to generate configuration template.
- Improve detection of ticket number.
- Summarize entries that have same issu_id/activity/comments in same day

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/chieping/togglmine.


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

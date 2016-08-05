# togglmine

TODO: Description here

## Installation

```
gem install togglmine
```

```
cat ~/.togglmine

tag-mapping:
  development: 開発作業
  deployment: 配備作業
  chore: 雑作業
  default: 雑作業
default-ticket: 8888
api-key:
  toggl: xxxxxxxxxxxxx
  redmine: xxxxxxxxxxxxx
```

## Usage

```
togglmine -d 2016-08-05
```

```
togglmine -d 2016-08-05 --dry-run
```

## How It Works

TODO: Toggl task name <-> Redmine time entry comment rule here

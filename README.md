
# Funnel

<p align="center">
    <img src="https://user-images.githubusercontent.com/7833164/55665412-1ca67180-580d-11e9-8e63-c09f83d919da.png" height="150"  width="150"></img>
</p>


Funnel is a lightweight yara-based feed scraper. Give a list of inputs and it will check them. Put it in a crontab and it will regularly update the database. If the article gets matched to the yara rule, it will be put into the database. All matched results get put into an sqlite database, with the rule it flagged.

## Installation

Install your required dependencies and you're good to go.

` pip3 install -r requirements.txt `

## Usage:

```bash
Funnel.py [-h] [-v] [-u] rule_path target_path

positional arguments:
  rule_path      path to directory of rules used on list of feeds
  target_path    path to sources list or url

optional arguments:
  -h, --help     show this help message and exit
  -v, --verbose  increase output verbosity
  -u, --url      scan one url instead of using sources list
```

**Example:** 

You want to get every new post on the internet that has your name or personal info in it. You would use as many sources as possible,and fill out the personal_info.yar rule. 

Schedule this command to run regularly using crontab:

`python3 Funnel.py rules/personal/ sources/sources-large.json
`

Want to scan just one url to see if it matches against any of your rule set? 

`python3 Funnel.py -u rules/ https://www.bbc.com/news/world-asia-47844000`

A bar that wants all the newest margharita recipes? You could do that. Every single post about a politician, for a data visualization project on how much each person is talked about? Works too! Just add rules and sources.

### Sources

The sources should be in a json file, with a url and a title for each source in the list. Here is a barebones example:

```json
{
    "sources-rss":[
        {
                "url": "https://www.reddit.com/r/netsec/.rss",
                "title": "netsec subreddit"
        },
        {
                "url": "https://www.reddit.com/r/malware/.rss",
                "title": "malware subreddit"
        }

    ]

}

```
**Tip:** Extract sources from feedly by using the opml_to_json.py file in the util folder to turn your exported feedly opml file into a valid sources file


### Rules

Some sample rules have been provided in the rules folder. Any standard yara rule will work, it is always being compared on text content at this point, no file analysis yet. You can pass in either a directory of rules, a nested directory of rules, or just one rule.

### Database

The database is in sqlite, and works with two tables. The first, is a table of links of matched articles, which have a unique id. The second table is a table of the matched rules with the matched article's id together. This keeps duplicates out of the links table, and makes for easy reference.

## Contribute

Feel free to add your suggestions for what to add to this project, even better if you give me a pull request!


Inspired by ThreatIngestor from InQuest



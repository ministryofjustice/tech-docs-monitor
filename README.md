[![Standards Icon]][Standards Link] 

# Tech Docs Template - Page Expiry Notifier

This repository was [forked](https://github.com/alphagov/tech-docs-monitor) from our friends at [alphagov](https://github.com/alphagov). 🤝

This repo is part of the [tech-docs-template][template], and is used in conjunction with the [page expiry feature][expiry] that is part of the [tech-docs-gem][gem].

GitHub Actions will run the script once a day during weekdays.
It will look at the pages API for your site, find all pages that have expired, and post a Slack message to the owner of each page to let them know that it needs reviewing.

![image](https://user-images.githubusercontent.com/26419401/165932087-27d50763-3491-45b8-b597-1f3f53bebabb.png)

## How can I use this service?

Usage is simple, you will need to:

- update your documentation as per the example below
- submit a PR to enable the notifier.

### Example GitHub Pages Configuration

In [this](https://github.com/ministryofjustice/cloud-operations/blob/main/source/documentation/team-guide/team-tools.html.md.erb) file the following lines define:

- Slack channel the reminder will be sent to
- Title of the reminder
- last reviewed date
- when the document should next be reviewed

```
---
owner_slack: "#mojo-devops"
title: Team Tools
last_reviewed_on: 2021-04-28
review_in: 3 months
---
...
```

⚠️ The `last_reviewed_on` field should be updated via Pull Request when a review is complete.

[template]: https://github.com/alphagov/tech-docs-template
[expiry]: https://alphagov.github.io/tech-docs-manual/#last-reviewed-on-and-review-in
[gem]: https://github.com/alphagov/tech-docs-gem

### Submit a PR

If you are part of the `ministryofjustice` GitHub organisation you can enable the notifier by raising a PR to add your published documentation to the [`Rakefile`](https://github.com/ministryofjustice/tech-docs-monitor/blob/main/Rakefile):

```
pages_urls = [
  "https://ministryofjustice.github.io/cloud-operations/api/pages.json",
  "https://ministryofjustice.github.io/<<your-team-name>>/api/pages.json",

]
```

If you want to limit the number of links that are posted to Slack after a single run, add this to `limits` in the [`Rakefile`](https://github.com/ministryofjustice/tech-docs-monitor/blob/main/Rakefile)

```
limits = {
  "https://ministryofjustice.github.io/<<your-team-name>>/api/pages.json" => 3
}
```

The default behaviour is no limit, and the Slack message will contain all pages discovered.

[Rakefile]: https://github.com/alphagov/tech-docs-monitor/blob/master/Rakefile

- `SLACK_WEBHOOK_URL`: The Slack webhook URL to allow messages to be posted.
- `REALLY_POST_TO_SLACK`: Messages will only be posted to Slack if the value of
  this var is `1`.

#### Slack message customisation

This is the default Slack message when pages expire:

![default-message-example](docs/images/default-message-example.png)

You can customise parts of the Slack message by configuring environment variables. The environment variables you can customise are:

| Environment variable name     | Purpose                                                         | Default value                                                                          |
| ----------------------------- | --------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| OVERRIDE_SLACK_MESSAGE_PREFIX | Sets a custom message prefix.                                   | "Hello :paw_prints:, this is your friendly manual spaniel."                            |
| OVERRIDE_SLACK_CHANNEL        | Sets a single Slack channel to which all messages will be sent. | The owning Slack channel for each page reported in the site's /api/pages.json endpoint |
| OVERRIDE_SLACK_USERNAME       | Sets the username to which Slack messages are attributed.       | "Daniel the Manual Spaniel"                                                            |
| OVERRIDE_SLACK_ICON_EMOJI     | Sets the icon emoji attributed to Slack messages.               | ":daniel-the-manual-spaniel:"                                                          |

## Licence

The gem is available as open source under the terms of the [MIT License](LICENCE).

[Standards Link]: https://github-community.service.justice.gov.uk/repository-standards/tech-docs-monitor "Repo standards badge."
[Standards Icon]: https://github-community.service.justice.gov.uk/repository-standards/api/tech-docs-monitor/badge

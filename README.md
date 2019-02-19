# st


Backstory
===
A stakeholder in your company wants a dashboard that reports pertinent email campaign information. She spoke with her Sailthru account manager and the account manager said she could use the Sailthru Data Exporter API to extract the data she needs.

She's asked you to reliably transfer the data from their API into a data warehouse that she can use to analyze the data.

The Data Exporter API produces a single .zip file every hour in S3. For this exercise, we will not directly connect to S3, but rather, we will use the data in the repository.
Particularly, we are going to be use the `message_blast` object:

```
{
	"profile_id": "53c6e32d41b8f1a31200001e",
	"send_time": "2016-03-31T08:00:03.018",
	"opens": [{
		"ts": {
			"$date": 1459431708000
		}
	}, {
		"ts": {
			"$date": 1459431709000
		}
	}],
	"clicks": [{
		"url": "http://li.about.com/click",
		"ts": {
			"$date": 1459431709000
		}
	}],
	"blast_id": 6364286,
	"message_id": 4,
	"device": "Firefox",
	"message_revenue": null,
	"delivery_status": null
}
```

A `blast` is a campaign, sent to many profiles.
A `message blast` is a message to a specific profile in a campaign.

The stakeholder wants to answer questions like:

How many total blasts have been sent?
How many opens/clicks per blast?
Which users have received more than 100 blasts?
Which users have clicked more than 100 times across all blasts?


Objective
===
Once an hour, about 5 minutes after the hour, Sailthru exports new data. The stakeholder should be able to refresh her dashboard and see the new data.

We want to store our data in a colunmnar data warehouse like Redshift. Design the tables that the ingested data will be inserted into.

Write a basic program that retrieves the newly created file, extracts the contents, and inserts the file into their respective database tables. You do not need to connect to S3 for this exercise. Just read the file from the filesystem.

Submit a repoistory with the application code, the SQL statements to create your database schema, and responses the questions below.


Questions
===
The stakeholder wants the *message_blast* chart to have two new fields: `total_emails_clicked` and `total_emails_opened`. How can we provide this information?

The stakeholder wants the *message_action* chart to have two new fields: `total_historical_emails_sent` and `total_historical_emails_opened`. How can we provide this information?

One day, we realize that if a user opens/clicks on an email several days after it was sent, their `message_blast` record is sent again! We have duplicate data in our system. How can we account for this?

Sometimes Sailthru's API is delayed the the stakeolder isn't sure if she's looking at the most up-to-date data. How can we remedy this?

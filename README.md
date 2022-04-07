## Create objects Question and Choice as per this tutorial [10%]

```python
import datetime

from django.db import models
from django.utils import timezone

class Question(models.Model):
    question_text = models.CharField(max_length=200)
    facilitator = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')
    def __str__(self):
        return self.question_text
    def was_published_recently(self):
        return self.pub_date >= timezone.now() - datetime.timedelta(days=1)

class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
    def __str__(self):
        return self.choice_text
```

## Generate SQL statements needed to create these objects (original SQL script) [10%]

```sql
BEGIN;
--
-- Create model Question
--
CREATE TABLE "polls_question" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "question_text" varchar(200) NOT NULL, "pub_date" datetime NOT NULL);
--
-- Create model Choice
--
CREATE TABLE "polls_choice" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "choice_text" varchar(200) NOT NULL, "votes" integer NOT NULL, "question_id" bigint NOT NULL REFERENCES "polls_question" ("id") DEFERRABLE INITIALLY DEFERRED);
CREATE INDEX "polls_choice_question_id_c5b4b260" ON "polls_choice" ("question_id");
COMMIT;
```
## Then add an additional attribute “facilitator” to Question [10%] and produce SQL statements needed to migrate database schema (migration script) [10%]

```sql
BEGIN;
--
-- Add field facilitator to question
--
CREATE TABLE "new__polls_question" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "facilitator" varchar(200) NOT NULL, "question_text" varchar(200) NOT NULL, "pub_date" datetime NOT NULL);
INSERT INTO "new__polls_question" ("id", "question_text", "pub_date", "facilitator") SELECT "id", "question_text", "pub_date", 'Ryerson' FROM "polls_question";
DROP TABLE "polls_question";
ALTER TABLE "new__polls_question" RENAME TO "polls_question";
COMMIT;
```

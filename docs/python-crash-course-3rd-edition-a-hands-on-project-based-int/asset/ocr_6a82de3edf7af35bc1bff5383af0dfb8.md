```
④ Select an option: 1
⑤ Please enter the default value now, as valid Python
The datetime and django.utils.timezone modules are availabl
e...
Type 'exit' to exit this prompt
⑥ >>> 1
Migrations for 'learning_logs':
    learning_logs/migrations/0003_topic_owner.py
- Add field owner to topic
(ll_env)learning_log$
```

We start by issuing the makemigrations command ①. In the output, Django indicates that we’re trying to add a required (non-nullable) field to an existing model (topic) with no default value specified ②. Django gives us two options: we can provide a default right now, or we can quit and add a default value in models.py ③. Here I’ve chosen the first option ④. Django then asks us to enter the default value ⑤.

To associate all existing topics with the original admin user, ll_admin, I entered the user ID of 1 ⑥. You can use the ID of any user you’ve created; it doesn’t have to be a superuser. Django then migrates the database using this value and generates the migration file 0003_topic_owner.py, which adds the field owner to the Topic model.

Now we can execute the migration. Enter the following in an active virtual environment:

```
(ll_env)learning_log$ python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, learning_l
ogs, sessions
Running migrations:
①   Applying learning_logs.0003_topic_owner... OK
(ll_env)learning_log$
```

Django applies the new migration, and the result is OK ①.
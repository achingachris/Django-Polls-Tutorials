<!-- Procedure Shell -->
# python3 manage.py shell API --playWithCode
from polls.models import Choice, Question  # Import the model classes we just wrote.
Question.objects.all()
from django.utils import timezone
q = Question(question_text="What's new?", pub_date=timezone.now())

Save the object into the database. You have to call save() explicitly.

q.save()
Now it has an ID.
q.id
q.question_text

q.pub_date
Change values by changing the attributes, then calling save().
q.question_text = "What's up?"
q.save()

Question.objects.all()

Django provides a rich database lookup API that's entirely driven by
keyword arguments.
Question.objects.filter(id=1)
Question.objects.filter(question_text__startswith='What')

from django.utils import timezone
current_year = timezone.now().year
Question.objects.get(pub_date__year=current_year)

Request an ID that doesn't exist, this will raise an exception.
Question.objects.get(id=2)
Traceback (most recent call last):

q = Question.objects.get(pk=1)
q.was_published_recently()

Display any choices from the related object set -- none so far.
q.choice_set.all()

Create three choices.
q.choice_set.create(choice_text='Not much', votes=0)
q.choice_set.create(choice_text='The sky', votes=0)
c = q.choice_set.create(choice_text='Just hacking again', votes=0)

c.question
q.choice_set.all()
q.choice_set.count()

The API automatically follows relationships as far as you need.
Use double underscores to separate relationships.
This works as many levels deep as you want; there's no limit.
Find all Choices for any question whose pub_date is in this year
(reusing the 'current_year' variable we created above).
Choice.objects.filter(question__pub_date__year=current_year)

Let's delete one of the choices. Use delete() for that.
c = q.choice_set.filter(choice_text__startswith='Just hacking')
c.delete()

# Admin Login

Admin - mylahv

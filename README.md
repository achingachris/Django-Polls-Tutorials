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

render()¶
It’s a very common idiom to load a template, fill a context and return an HttpResponse object with the result of the rendered template. Django provides a shortcut.

request.POST is a dictionary-like object that lets you access submitted data by key name. In this case, request.POST['choice'] returns the ID of the selected choice, as a string. request.POST values are always strings.

Note that Django also provides request.GET for accessing GET data in the same way – but we’re explicitly using request.POST in our code, to ensure that data is only altered via a POST call.

request.POST['choice'] will raise KeyError if choice wasn’t provided in POST data. The above code checks for KeyError and redisplays the question form with an error message if choice isn’t given.

After incrementing the choice count, the code returns an HttpResponseRedirect rather than a normal HttpResponse. HttpResponseRedirect takes a single argument: the URL to which the user will be redirected (see the following point for how we construct the URL in this case).

As the Python comment above points out, you should always return an HttpResponseRedirect after successfully dealing with POST data. This tip isn’t specific to Django; it’s just good Web development practice.

We are using the reverse() function in the HttpResponseRedirect constructor in this example. This function helps avoid having to hardcode a URL in the view function. It is given the name of the view that we want to pass control to and the variable portion of the URL pattern that points to that view. In this case, using the URLconf we set up in Tutorial 3, this reverse() call will return a string like
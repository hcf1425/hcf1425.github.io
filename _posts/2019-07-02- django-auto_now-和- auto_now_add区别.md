### django-auto_now-和- auto_now_add区别

### DateField

#### class DateField(auto_now=False, auto_now_add=False, **options)

`A date, represented in Python by a datetime.date instance. Has a few extra, optional arguments:`

**DateField.auto_now**

`Automatically set the field to now every time the object is saved.` Useful for “last-modified” timestamps. Note that the current date is always used; it’s not just a default value that you can override.

The field is only automatically updated when calling Model.save(). The field isn’t updated when making

updates to other fields in other ways such as QuerySet.update(), though you can specify a custom value

for the field in an update like that.

**DateField.auto_now_add**

`Automatically set the field to now when the object is first created.` Useful for creation of timestamps. Note that the current date is always used; it’s not just a default value that you can override. So even if you set a value for this field when creating the object, it will be ignored. If you want to be able to modify this field, set the following instead of auto_now_add=True:

• For DateField: default=date.today - from datetime.date.today()

• For DateTimeField: default=timezone.now - from django.utils.timezone.now()

The default form widget for this field is a TextInput. The admin adds a JavaScript calendar, and a shortcut for “Today”. Includes an additional invalid_date error message key.

The options auto_now_add, auto_now, and default are mutually exclusive. Any combination of these options will result in an error.

**Note:** As currently implemented, setting auto_now or auto_now_add to True will cause the field to have editable=False and blank=True set.

**Note: **The auto_now and auto_now_add options will always use the date in the default timezone at the moment of creation or update. If you need something different, you may want to consider simply using your own callable default or overriding save() instead of using auto_now or auto_now_add; or using a DateTimeField instead of a DateField and deciding how to handle the conversion from datetime to date at display time.

**Some people at stackoverflow shows usecase as flow:**

Any field with the [`auto_now`](https://docs.djangoproject.com/en/2.2/ref/models/fields/#django.db.models.DateField.auto_now) attribute set will also inherit `editable=False` and therefore will not show up in the admin panel. There has been talk in the past about making the `auto_now` and [`auto_now_add`](https://docs.djangoproject.com/en/2.2/ref/models/fields/#django.db.models.DateField.auto_now_add) arguments go away, and although they still exist, I feel you're better off just using a [custom `save()` method](https://docs.djangoproject.com/en/2.2/topics/db/models/#overriding-model-methods).

So, to make this work properly, I would recommend not using `auto_now` or `auto_now_add` and instead define your own `save()` method to make sure that `created` is only updated if `id` is not set (such as when the item is first created), and have it update `modified` every time the item is saved.

I have done the exact same thing with other projects I have written using Django, and so your `save()` would look like this:

```
from django.utils import timezone

class User(models.Model):
    created     = models.DateTimeField(editable=False)
    modified    = models.DateTimeField()

    def save(self, *args, **kwargs):
        ''' On save, update timestamps '''
        if not self.id:
            self.created = timezone.now()
        self.modified = timezone.now()
        return super(User, self).save(*args, **kwargs)
```

Any field with the [`auto_now`](https://docs.djangoproject.com/en/2.2/ref/models/fields/#django.db.models.DateField.auto_now) attribute set will also inherit `editable=False` and therefore will not show up in the admin panel. There has been talk in the past about making the `auto_now` and [`auto_now_add`](https://docs.djangoproject.com/en/2.2/ref/models/fields/#django.db.models.DateField.auto_now_add) arguments go away, and although they still exist, I feel you're better off just using a [custom `save()` method](https://docs.djangoproject.com/en/2.2/topics/db/models/#overriding-model-methods).

So, to make this work properly, I would recommend not using `auto_now` or `auto_now_add` and instead define your own `save()` method to make sure that `created` is only updated if `id` is not set (such as when the item is first created), and have it update `modified` every time the item is saved.

I have done the exact same thing with other projects I have written using Django, and so your `save()` would look like this:

```py
from django.utils import timezone

class User(models.Model):
    created     = models.DateTimeField(editable=False)
    modified    = models.DateTimeField()

    def save(self, *args, **kwargs):
        ''' On save, update timestamps '''
        if not self.id:
            self.created = timezone.now()
        self.modified = timezone.now()
        return super(User, self).save(*args, **kwargs)
```

Hope this helps!

**Edit in response to comments:**

The reason why I just stick with overloading `save()` vs. relying on these field arguments is two-fold:

1. The aforementioned ups and downs with their reliability. These arguments are heavily reliant on the way each type of database that Django knows how to interact with treats a date/time stamp field, and seems to break and/or change between every release. (Which I believe is the impetus behind the call to have them removed altogether).
2. The fact that they only work on DateField, DateTimeField, and TimeField, and by using this technique you are able to automatically populate any field type every time an item is saved.
3. Use `django.utils.timezone.now()` vs. `datetime.datetime.now()`, because it will return a TZ-aware or naive `datetime.datetime` object depending on `settings.USE_TZ`.

To address why the OP saw the error, I don't know exactly, but it looks like `created` isn't even being populated at all, despite having `auto_now_add=True`. To me it stands out as a bug, and underscores item #1 in my little list above: `auto_now` and `auto_now_add` are flaky at best.
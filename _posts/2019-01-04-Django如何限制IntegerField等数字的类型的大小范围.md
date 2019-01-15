# Django如何限制IntegerField等数字的类型的大小范围

Django has various numeric fields available for use in models, e.g. [DecimalField](http://docs.djangoproject.com/en/dev/ref/models/fields/#decimalfield) and [PositiveIntegerField](http://docs.djangoproject.com/en/dev/ref/models/fields/#positiveintegerfield). Although the former can be restricted to the number of decimal places stored and the overall number of characters stored, is there any way to restrict it to storing *only* numbers within a certain range, e.g. 0.0-5.0 ?

**answer:**You can use [Django's built-in validators](https://docs.djangoproject.com/en/dev/ref/validators/#built-in-validators)—

~~~
from django.db.models import IntegerField, Model
from django.core.validators import MaxValueValidator, MinValueValidator

class CoolModelBro(Model):
    limited_integer_field = IntegerField(
        default=1,
        validators=[
            MaxValueValidator(100),
            MinValueValidator(1)
        ]
     )
~~~



参见：`https://stackoverflow.com/questions/849142/how-to-limit-the-maximum-value-of-a-numeric-field-in-a-django-model`
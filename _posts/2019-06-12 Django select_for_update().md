### Django `select_for_update()`[¶](https://docs.djangoproject.com/en/2.1/ref/models/querysets/#select-for-update)

`select_for_update`**(***nowait=False***,** *skip_locked=False***,** *of=()***)**

Returns a queryset that will lock rows until the end of the transaction, generating a `SELECT ... FOR UPDATE` SQL statement on supported databases.

For example:

```
from django.db import transaction

entries = Entry.objects.select_for_update().filter(author=request.user)
with transaction.atomic():
    for entry in entries:
        ...
```

When the queryset is evaluated (`for entry in entries` in this case), all matched entries will be locked until the end of the transaction block, meaning that other transactions will be prevented from changing or acquiring locks on them.

Usually, if another transaction has already acquired a lock on one of the selected rows, the query will block until the lock is released. If this is not the behavior you want, call `select_for_update(nowait=True)`. This will make the call non-blocking. If a conflicting lock is already acquired by another transaction, [`DatabaseError`](https://docs.djangoproject.com/en/2.1/ref/exceptions/#django.db.DatabaseError) will be raised when the queryset is evaluated. You can also ignore locked rows by using`select_for_update(skip_locked=True)` instead. The `nowait` and `skip_locked` are mutually exclusive and attempts to call `select_for_update()` with both options enabled will result in a [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError).

By default, `select_for_update()` locks all rows that are selected by the query. For example, rows of related objects specified in [`select_related()`](https://docs.djangoproject.com/en/2.1/ref/models/querysets/#django.db.models.query.QuerySet.select_related) are locked in addition to rows of the queryset’s model. If this isn’t desired, specify the related objects you want to lock in `select_for_update(of=(...))` using the same fields syntax as [`select_related()`](https://docs.djangoproject.com/en/2.1/ref/models/querysets/#django.db.models.query.QuerySet.select_related). Use the value `'self'` to refer to the queryset’s model.

You can’t use `select_for_update()` on nullable relations:

```
>>> Person.objects.select_related('hometown').select_for_update()
Traceback (most recent call last):
...
django.db.utils.NotSupportedError: FOR UPDATE cannot be applied to the nullable side of an outer join
```

To avoid that restriction, you can exclude null objects if you don’t care about them:

```
>>> Person.objects.select_related('hometown').select_for_update().exclude(hometown=None)
<QuerySet [<Person: ...)>, ...]>
```

Currently, the `postgresql`, `oracle`, and `mysql` database backends support `select_for_update()`. However, MySQL doesn’t support the `nowait`, `skip_locked`, and `of` arguments.

Passing `nowait=True`, `skip_locked=True`, or `of` to `select_for_update()` using database backends that do not support these options, such as MySQL, raises a [`NotSupportedError`](https://docs.djangoproject.com/en/2.1/ref/exceptions/#django.db.NotSupportedError). This prevents code from unexpectedly blocking.

Evaluating a queryset with `select_for_update()` in autocommit mode on backends which support `SELECT ...FOR UPDATE` is a [`TransactionManagementError`](https://docs.djangoproject.com/en/2.1/ref/exceptions/#django.db.transaction.TransactionManagementError) error because the rows are not locked in that case. If allowed, this would facilitate data corruption and could easily be caused by calling code that expects to be run in a transaction outside of one.

Using `select_for_update()` on backends which do not support `SELECT ... FOR UPDATE` (such as SQLite) will have no effect. `SELECT ... FOR UPDATE` will not be added to the query, and an error isn’t raised if `select_for_update()` is used in autocommit mode.

Warning

Although `select_for_update()` normally fails in autocommit mode, since [`TestCase`](https://docs.djangoproject.com/en/2.1/topics/testing/tools/#django.test.TestCase)automatically wraps each test in a transaction, calling `select_for_update()` in a `TestCase` even outside an [`atomic()`](https://docs.djangoproject.com/en/2.1/topics/db/transactions/#django.db.transaction.atomic) block will (perhaps unexpectedly) pass without raising a `TransactionManagementError`. To properly test `select_for_update()` you should use[`TransactionTestCase`](https://docs.djangoproject.com/en/2.1/topics/testing/tools/#django.test.TransactionTestCase).

Certain expressions may not be supported

PostgreSQL doesn’t support `select_for_update()` with [`Window`](https://docs.djangoproject.com/en/2.1/ref/models/expressions/#django.db.models.expressions.Window) expressions.


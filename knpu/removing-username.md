# My Users don't have a Username!

Okay: a challenge! In some apps... you don't even *need* a username - you register
and login entirely with an email.

But... with FOSUserBundle, we *always* have a username field. Does that means we're
stuck?

Nope! It's true that you can't remove the `username` field from your `user` table.
But, we can very easily remove it from everywhere else.

## Auto-Setting the Username Field

Start in the `User` class. I'll go to the Code->Generate menu - or Command+N on a
Mac - go to "Override Methods" and choose `setEmail()`. Before the parent call,
add `$this->setUsername($email)`.

Thanks to this, we no longer need to worry about *ever* setting the `username` field.
In the database, `username` will always equal the `email`... which is definitely
redundant and unnecessary, but in practice, it works great.

## Removing the Username Field from the Form

The only other thing we need to do is remove the `username` field from the registration
form. How? Easy: in `RegistrationFormType`, add `->remove('username')`.

Then, in the template, remove its `form_row()` call.

And yea, that's it! Refresh! No more username! Register as `aquanaut4@gmail.com`
and submit. Check it out in the database:

```terminal
php bin/console doctrine:query:sql 'SELECT * FROM user'
```

There it is: the `username` is now equal to the email.

Oh, and if you're using the profile edit page from the bundle, you'll also want to
remove the `username` field from the `ProfileFormType`. It's done in exactly the
same way.
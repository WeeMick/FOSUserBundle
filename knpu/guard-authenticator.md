# Guard Authenticator

It's at this point we understand that FOSUserBundle is just a bundle that gives us a nice user class, and it gives us a bunch of routes and controllers for registration, reset password, edit profile and a few other things. If you don't need these, if you don't need a registration page or a reset password page, then you might not want to use FOSUserBundle. It's easy enough to create a user entity class yourself because, remember, FOSUserBundle doesn't actually provide any security. If we look in our app config security.yml, we're using the form login security. That's the core built-in security mechanism for Symphony. That's completely independent of FOSUserBundle.

One of the questions that I get a lot is how do you use guard authentication with FOSUserBundle? Answer is they're two separate things, so it ends up being completely simple to do. First, why would you use guard with FOSUserBundle? The answer is sometimes form login, even though it's really easy to use, might not be ... Can be a little hard to customize. If you need to ... If you have really custom behavior after you log in successfully, or you have a really complex query when you log in, whatever, guard authentication gives you a little bit more flexibility. Let's use it.

At the root of our project, if you download it, you should have tutorial directory which contains a class called login form authenticator. I'm going to copy this. Inside [SRC 00:01:57] app bundle, I'm going to create a new directory called security, then I'm going to paste the class inside of there. This login form authenticator is almost a copy and paste from our Symphony security tutorial where we created the login form authenticator. The only difference is I've added CSRF token checking, since our login form has a CSRF token in it, and I've updated a few things to work ... And I've updated a couple of other minor things. For example down here, the route name. Your login page is going to be the route name [inaudible 00:02:35] application.

Other than that, this authenticator is very straightforward. It looks for an underscore username and underscore password field from your login form. It doesn't care if you built that login form yourself, or if FOSUserBundle built it. It queries for your user object by email only, then it checks your password to see if your password is valid. Obviously you can write your login form authenticator to do anything.

To get this to work, like all authenticators, we need to register this as a service. I'll say app.security.login_form_authenticator, class login form authenticator, autowire true, then copy that service ID. Then we'll go into app config, security.yml and check this out. I'm going to comment out form login, and instead I'm going to say guard authenticators- and then paste my service ID. That is it. FOSUserBundle does not care who is processing our login form submit.

Let's try it. I'll click log out. I'll click login. We'll log in as our admin user, admin password. Log in as aquanaut5, password turtles, hit enter. Fuck, six. Log in as aquanaut6, password turtles. The other older users actually won't work anymore, because ... Oh shoot [inaudible 00:05:20]. We'll log in as admin@aquanote.com. Notice this still says username, but we know that our authenticator only logs you in via email. Then we'll say password admin, and boom.

Congratulations, you're logged in as admin and you just used guard to do it. You can see it's really simple, and empowers you to use FOSUserBundle because you want all of the registration, reset password, those types of things, but you can take control of your actual login mechanism and do whatever the heck you want. All right guys, hope we had a blast. I know I did. See you guys next time.
---
template: post
title: "The Smart Way To Handle Request Validation In Laravel \U0001F60E"
slug: "The Smart Way To Handle Request Validation In Laravel \U0001F60E"
draft: false
date: 2019-03-23T16:32:31.873Z
description: >-
  Laravel is PHP framework for web artisan. Which helps us to build robust
  application and APIs. As many of you already know there are many ways to
  validate request in Laravel. Handling request validation is very crucial part
  of any application. Laravel has some great feature which deals with this very
  well.
category: laravel
tags:
  - laravel
---
Get Started

Most of us are familiar with using the validator in the controller. And it’s most common way to handle validation for the incoming request.



Here’s how our validator looks like in UserController





Validation in controller

There’s nothing wrong with validating incoming request in the controller but it’s not the best way to do this and your controller looks messy. This is bad practice in my opinion. The controller should do only one thing handle request from the route and return an appropriate response.



Writing validation logic in the controller will break The Single Responsibility Principle. We all know that requirements changes over time and every time requirement get change your class responsibilities is also change. So having many responsibilities in single class make it very difficult to manage.



Laravel has Form Request, A separate request class containing validation logic. To create one you can use below Artisan command.



php artisan make:request UserStoreRequest

Which will create new Request class in app\Http\Request\UserRequest





Laravel Form Request class comes with two default methods auth() and rules(). You can perform any authorization logic in auth() method whether the current user is allowed to request or not. And in rules() method you can write all your validation rule. There’s one additional method messages() where you can pass your own validation messages array.



Now change our UserController to use our UserStoreRequest. You can type-hint our request class and it will automatically resolve and validate before our controller function called.





So our controller is now slim and easy to maintain . Now our controller doesn’t need to worry about any validation logic. We have our own validation class with only one responsibility to handle validation and let controller do there work.



If validation fails, it will redirect the user to the previous location with an error. Depends on your request type error message will be flash in sessions. If the request was an AJAX then a response with 422 status code will be return with an error in JSON format.





✨ Bonus

Keep your application and your User safe by sanitizing inputs. Using sanitizers in your application it’ll ensure you that data is always well-formatted and consistent. In many cases validation failed due to silly formatting mistakes.



A User entered a mobile number like this +99–9999–999999 or +99-(9999)-(999999). It’s very common error to make for that we can’t force our user to re-enter the same details again.



Some other examples are a user entered an email as Foo@Bar.COM or FOO@Bar.com. Or type first name and last name like FOO bAR or foo baR



Sanitizer contains methods for transforming and filtering our data in common format before providing to the validator.



I’m using Waavi/Sanitizer package which has many filters.



Waavi/Sanitizer



Sanitizer - Data sanitizer and form request input sanitation for Laravel 5.

github.com	

Let’s create BaseFormRequest abstract class for our Form Request and use SanitizesInput trait here.





So now we can write our UserStoreRequest like below. Extend your Form Request from our base class so we don’t have to include trait in all request classes.





SanitizesInput trait provides a method filters() for formatting our request data before providing to the validator. filters() method return array of valid filters. Here we converting user email to lowercase and trimming same way converting name to uppercase and escape any HTML tags.



You can read more about available filters from here.



Conclusion

At first, it seems unnecessary to make separate request class for all. But imagine putting all your validations logic in the same controller. It’s like a terrible nightmare 👻 when it’s come to manage your code and worst if someone else has to manage it 😛.



Thank you for reading. 

I’d like to hear your views on it. If you have any questions or suggestion please leave the comment below.



Have a wonderful day.

---
layout: post
title: Implement two-factor authentication now!
---
[2FA](http://en.wikipedia.org/wiki/Two-factor_authentication), two-factor authentication (or two-step verification as it's also called) has been in the news regularly lately, as more and more sites implements it - unfortunately usually after being hacked.

<img src="{{ site.baseurl }}/public/img/2FA.png" alt="Porotype logo" align="right"/>

([Google explains](http://www.google.com/landing/2step/) quite well what it's all about.)

## Seriously!
So why should one want to implement 2FA? Well...

Slack [recently enabled 2FA](http://www.theverge.com/2015/3/27/8301031/slack-office-app-two-factor-authentication-secure) after a serious security breach in February. 
Last October JPMorgan disclosed the  biggest U.S. banking breach of all time, and the [hackers were able to gain access after finding a server unprotected by 2FA](https://threatpost.com/two-factor-snafu-opened-door-to-jpmorgan-breach/110119).
In January 2014 the @N twitter account was [famously stolen](http://www.theverge.com/2014/1/29/5356866/twitter-n-account-attack-naoki-hiroshima-godaddy-paypal), with the rightful owner concluding that [this would not have been possible, had he enabled 2FA](https://medium.com/@N/how-i-lost-my-50-000-twitter-username-24eb09e026dd) on all accounts. [Twitter implemented 2FA in 2013](http://www.theverge.com/2013/4/23/4259342/twitter-reportedly-working-on-two-factor-authentication) following a number of high-profile hacks, [as did Dropbox](https://blogs.dropbox.com/dropbox/2012/07/security-update-new-features/) (weeks after being hacked),
[LinkedIn](http://www.pcmag.com/article2/0,2817,2419790,00.asp) (about a year after being hacked) and [Evernote](http://www.pcmag.com/article2/0,2817,2419713,00.asp) (three months after being hacked).

The list goes on, and continues to grow - and what they all have in common is that the hacks could have been prevented if 2FA had been implemented and enabled _before_ the hacks. Please make sure your application does not make the list!

## Implementing 2FA
Technically 2FA is very easy to implement using existing libraries, and there is really no reason not to. And you can easily verify that your implementation is generating the one-time passwords correctly by using e.g [Google Authenticator](https://support.google.com/accounts/answer/1066447?hl=en) on your phone - if they generate the same numbers, it's working...

There are some additional things you should consider, though.

### Generating the shared key.
The key used to generate the one-time passwords needs to be correctly generated in order to generate secure passwords. If it's not, it might be generating passwords easy to guess (silly example: if you accidentally use the same key for all users, they will all get the same passwords). Unless you really know what you are doing, you should not try to roll your own key-generator. 

Luckily, you do not have to do this yourself - there is a good spec, reference implementation, and a number of open-source libraries available. [Wikipedia is a good starting point](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm), and you can have a look at the [Porotype TOTP example.](https://github.com/Porotype/TOTPassword) 

### Sharing the key or delivering the password
There are basically two ways to get the one-time passwords to the user: 

1. By sharing a secret key, so the user can use an app (e.g Google Authenticator) to generate the passwords. 
2. By sending each generated password via SMS when the user needs it.

Option 1 is easy to implement - you only need to get the key to the user securely once. Usually by showing a URL and/or QR-code when 2FA is enabled by the user. It can also be sent via another channel (like snail-mail) or fetched in-person for ultimate security.

Option 2 might be a bit harder to implement (at least you need reliable SMS sending capabilities), but for many users this is much easier. Setting up an app and understanding how it works might prove overwhelming. 

### Enabling 2FA
One important step, where things can go seriously wrong, is the point when 2FA is enabled for an account. At this point you need to make sure the user has understood and has everything set up in working order. Usually you require the user to input a one-time password ("logging in" once), before actually enabling 2FA for the account.

### Account recovery
Phone numbers change, and phones break, go missing and get stolen - sooner or later you will need to recover an account for a user. If you make it [too easy](http://shubh.am/how-i-bypassed-2-factor-authentication-on-google-yahoo-linkedin-and-many-others/) you might as well not have passwords at all. Make it too hard, and a lot of users will end up locked-out. 

Google, for instance, allows users to print a list of back-up codes. 

Finding the right requirements and procedures for account recovery is ultimately up to you, and should be done up-front and with thought. You can always change your policy later.

### Usability
Asking for a one-time password at login along with username and (regular) password is not the only way to do it. And chances are it's not the best way to do it either.

When and how often to ask for a second authentication really depends on your application. What follows is food for thought - consider how secure your application should be, and apply as appropriate in your case.

Starting up the 2FA application, or waiting for an SMS, can be annoying, and asking at each login might not be needed. Some applications has a "Remember this device [for some amount of time]" option - you will be asked again when logging in from another device (or browser), or when the application deems it necessary. Some applications get suspicious when a user logs in from a different location (e.g country, based on ip address), for instance.

Another thing to consider is that login might not be the only critical operation. You might want to protect some other opration(s) by asking for authentication at that stage. The obvious example being changing password, and disabling 2FA - you should definitely ask for authentication at that point. Some other examples: making a payment, publishing an article, deleting an item, and so on.

In fact, depending on what your application does, you might not want to use 2FA at login at all, instead asking for it when really needed, for maximum protection with minimum annoyance.

For instance, the Nordea Mobile Bank app allows the user to log in and view account balance and transactions using just a pin code - but to make a payment, you need to be fully authenticated, and the app will ask for passwords when needed. It's a smart way of minimizing the friction, while protecting the important stuff.

As mentioned earlier, 2FA might be a difficult concept to grasp for some users - especially if it involves installing apps on your phone. Printed password lists might be an option, and there are also dedicated hardware devices available. Please consider both your security needs and your target audience when sorting out the details for your implementation.

## Conclusion
__Please implement two-factor authentication as soon as possible__ in your applications. Technically it's really simple to do - the main challenge, really, is to achieve suitable usability. As any security measure, 2FA is only effective if it's being used.

I previously [blogged about 2FA](https://vaadin.com/blog/-/blogs/two-factor-authentication-with-google-authenticator) on the Vaadin blog, and an [example application](https://github.com/Porotype/TOTPassword) is available. 

Finally, I leave you with [an article about enabling 2FA everywhere](http://lifehacker.com/5938565/heres-everywhere-you-should-enable-two-factor-authentication-right-now), and an [extensive list](https://twofactorauth.org) of sites supporting or not supporting 2FA.



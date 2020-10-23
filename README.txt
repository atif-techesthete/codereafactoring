Choose ONE of the following tasks.
Please do not invest more than two hours on this.
Upload your results to a Github Gist, for easier sharing and reviewing.

Thank you and good luck!



Code to refactor
=================
1) app/Http/Controllers/BookingController.php
2) app/Repository/BookingRepository.php

Code to write tests
=====================
3) App/Helpers/TeHelper.php method willExpireAt
4) App/Repository/UserRepository.php, method createOrUpdate




What I expect:

A. A readme with:

1.  Your thoughts about the code. What makes it amazing code. Or what makes it ok code. Or what makes it terrible code. How would you have done it. Thoughts on formatting. Logic.. 
2.  Refactor it if you feel it needs formatting. The more love you put into it. The easier for us to asses.  


Make two commits. First commit with original code. Second with your refactor so we can easily trace changes.

Vänliga hälsningar / Best regards
Virpal Singh




1. ------------------ Thoughts about code ----------------------

I overviewd the whole code. Its good but there are few things I noticed that I think that should not be
there if following the professional approaches such as
-> Most of the times the code is duplicted. If the feature is using over and over repeatedly then
do not need to duplicate that , instead create some kind of custom function and just avail that feature by
calling that functions

-> Commenting saves lives : Adding comments to your code can be invaluable — they can quickly show
what a complex function is doing, or maybe even explain why certain things need to happen in a certain order.

->Lines and lines of duplicate code are not only harder to read than a concise and elegant solution,
but also increase the chance for error. The great thing about programming is
that you can express complex commands in tidy, reusable, and clever ways



2. ----------------------- What make it amazing code

-> Most of the beginners do this mistake that they write a function that can handle and
do almost everything (perform multiple tasks). This is the terrible approach instead follow
the "Single Responsbility Principle". Each function/module should focus on single feature

-> Avoid Writing Unnecessary Comments
Good code is its own best documentation. Commenting is good but doing unnecessary commenting is not
always good approach as the development go bigger.

-> Writing reusable code is key feature of professional approach. This will avoid the duplication as i see
most of the times in this project.



================= Code refactoring highlights ===============

These are the just highlights you can view live on github

BookingController.php

function distanceFeed line #195 orginal code

- functionality for checking empty and null values is repeated over and over. So instead of code repetition,
just create a custom function implementing that functionality and use that. I created one with name "ifExist"

public function ifExist($value){
        return isset($value) && !empty($value) ? true:false;
}

- Secondly instead of using if-else block for single statement , instead use the ternaory operator to make it
neat and clean.

$distance   = $this->ifExist($data['distance']) ?   $data['distance'] : "";

Now using the custom function for functionality

Next line # 220 of orginal code

if return statement is used inside the if block then don't need to explicty create the else block
because once the return statement will execute, functione excutation will terminate and none of the other
statment will run. This would give the code a much better and professional approach.


Next line #226 of orginal code
for very simple situation to just assign the value we can refactor code using  ternaory operator


-function resendNotifications & resendSMSNotifications having the same feature but with different functionlity
Still its perfect because of high cohesion but this can be refactored using the one modules differentiating
the both functionality on the base of flag.

pseudo code

if flag : then resendNotification else resendPushNotification

There are various approaches but the main thing is that feature and functionlity should not be compromised
in any case.


BookingRepository.php

Line #58 orginal file

function getUsersJobs(user_id)

The same condition is check in both if and else if block so instead of using this approach just check if
once and then check the rest of the conditions as nested block


Orginal =>
if ($cuser && $cuser->is('customer')) {

        }

        elseif ($cuser && $cuser->is('translator')) {

        }

Refactored =>

if ($cuser ) {
           if($cuser->is('customer')){
           // businness logic in case of customer
           }
           else if($cuser->is('translator')){
           //business logic in case of translator
           }
    }



line #92 function getUsersJobsHistory


- remove the duplicated and unused variables
- instead of using the return statement both in "if" and "else" block just get the data using
the business login in both block and return data finally at once.


line #128  orginal file => function store

- You should never use env() in the code directly. It's a good practice to use config().
           In config files use env() to get the data from .env file.
- $data['job_far'] is used over and over repeatdly just get into a variable and used that instead of
directly using the array value


- use ternaory to make the if-else block much neater and cleaner
- Do not need to duplicate the conditions. Make the duplicate inside nested conditions like the following


Orginal


if(in_array('normal', $data['job_for'])){
                $data['certified'] = 'normal';
        //business logic
}else if (in_array('certified', $data['job_for'])) {
                 $data['certified'] = 'yes';
   }



   if (in_array('normal', $data['job_for']) && in_array('certified', $data['job_for'])) {
                   $data['certified'] = 'both';
               }

//This condition is duplicated. So instead of checking it again just make it nested like  following

if(in_array('normal', $data['job_for'])){
        if(in_array('certified', $data['job_for'])){
            // now this block can handle the "and" situation
        }else{
                // use rest of business logic if above condition doesn't matched
        }
}

- so instead of duplicating the conditions, just make them nest on the base of normal condition
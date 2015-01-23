---
layout: post
title: The Hidden Complexity of Merchant Auto Completion
date:   2015-01-30 22:00:30
category: engineering
---

You might have read recently that we added merchant auto completion in some parts of the Expensify Web App.

This "simple" addition has required some thinking on how people are using our product and what would be the best experience for them.

Like every product change, it is crucial to define how important this feature is and how important the flow supported is. Editing the merchant name of an expense is part of the expense edition flow. While important, this is kind of a secondary flow for us. We are optimizing the usage of our SmartScan and bank import technologies to automate the process of creating expenses in an user account. By using these two technologies in conjunction with Expense Rules, users can automate the vast majority of the expense reporting experience.

Despite that however, the ability to manually change a merchant name is still necessary because:

- SmartScan can fail
- The name of the transaction in the bank account can be terrible
- The user is simply needs to create a cash expense.

Having auto completion for this step is not mandatory, but it can make the user experience much smoother. Details like this make the difference between a great experience and an amazing one.


## Prepping the Build

With all that in mind, we can only spend a few hours building this auto completion mechanism. Why? We are a small and dynamic team, and we need to aggressively prioritize our development hours to projects with the highest ROI.

The first question we asked ourselves regarding auto completion is the origin of the data. Where does the merchant we are suggesting are coming from? Are they merchants the user has previously used? Or are we picking our suggestions from all merchants in our system?

### 1. Using Data From User History vs. All Data History

Basing auto completion on a user's past expenses runs into issues quickly as a new user with a brand new account. With no historical data, it would be very difficult to find merchants for auto complete. On the other hand, if we pick from all merchants in the existing Expensify database, there is a much higher chance that someone else has bought something from this merchant, so we can easily find that merchant and offer it as a suggestion.

### 2. Variations - What Should We Do?

Expensify's database holds millions of different merchants from all over the world and in every business area imaginable. Sadly, this amazing source of data can not be used directly, because for each merchant, there can be a dozen variations due to inconsistent cases, typos, location, or other extra information.

Let's take the Hilton hotels as an example. We have "Hilton", "Hilton hotel", "Hilton hotel San Francisco", "Helton", and possibly many other variations. This can be due to user input but mostly comes from our bank import technology, which allows users to automatically import their transactions coming from their bank account and into Expensify. The same merchant can show up completely different depending on the bank; you might have noticed that merchant names in your bank statements are not exactly nice or accurate, to say the least. We have a few ways to "clean up" the data imported, but the number of possible variations make it impossible to be perfect.

3. Solving the Variations Issue

Why this diversity is a problem? Well if the user starts to type "Hilton," the suggestion he will get is going to be "Hilton", "Hilton hotel," "Hilton hotel San Francisco," or "Hilton New York," which are in fact the same exact merchant. What if the user was looking for a merchant "Hilton BBQ"? It could have appeared immediately if the hotel suggestions were collapsed into one. 

To solve that issue, we could just clean our data set by removing the extra suggestion and keep only "Hilton" for the hotel. Problem, we would need to review every single merchant in our database to check if we want to offer a suggestion with it or not. We can do instead, is to build a smarter algorithm that weights the results.  The autocompletion will not just be based on the merchant that start with the same letters but it will also take into account the frequency/popularity of the merchant. As the results, the most popular merchant will be shown first, and the more letter the user type, the more precise the suggestions are becoming.

In our current implementation, we are using a combination of both: we have have extracted and cleaned merchants and we are using there frequency for the suggestions.

## Building Out the Feature

With all these parameters, how did we actually build the auto categorization feature?

We couldn't go with a pure JavaScript implementation because we have way too many merchants to send with the site, and if we wanted to offer the same functionality in our mobile app, we would need to recode the same algorithms.

So instead, we need an API around that. The API has to be extremely fast to allow us to autocomplete as people are typing. It has to scale with our spiky traffic, should not wake our ops team at night but still be up all the time, etc. To do this, we decided to use Constructor.io, a brand new service offering autocompletion as a service. We send them our data set, and in exchange we get a purely functional, worry-free, API endpoint to get our auto completion suggestion. And because autocompletion is their business, they offer feature that we would not have thought such as typo handling.


*Thanks to [Joanie](https://twitter.com/joanatello) for the proof reading*
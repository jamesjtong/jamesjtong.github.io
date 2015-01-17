---
layout: post
title: "Angular ngOptions Quick and Dirty Understanding"
date: 2015-01-01 16:20:25 -0500
comments: true
categories: ["angularjs", "angular", "ng-options", "angular select"]
---

Understanding ng-options is a lot simpler than one would expect after seeing the angular documentation for it. 

# When do we have to use it?
 We use it when we want to bind a javascript object to a select box (using ng-model). 
(NOTE: ng-repeat can ONLY be used to bind a primitive/string to to a select box, it won't work with a javascript object). 

For example, let's say I want a select box that is able to change the currently selectedCompany. The code would look like the below:

```javascript
// companies.js

$scope.companies = [
  {
    name: "ABC Soap Shop",
    id: 1,
    revenue: 5000,
    type: "brick_and_mortar"
  },
  {
    name: "Pizza Haven",
    id: 2,
    revenue: 100000,
    type: "brick_and_mortar"
  },
  {
    name: "XYZ Mac Repair",
    id: 3,
    revenue: 45000,
    type: "brick_and_mortar"
  },
  {
    name: "Facebook",
    id: 4,
    revenue: 8999999,
    type: "online"
  },
  {
    name: "Apple",
    id: 5,
    revenue: 999999999999,
    type: "online"
  }
]

$scope.selectedCompany = $scope.companies[0];
```

{% raw %}
```
List of Companies
<pre>companies {{companies}}</pre>
<pre>selectedCompany {{selectedCompany}}</pre>
<select ng-model="selectedCompany" ng-options="company.name for company in companies">
</select>
```
{% endraw %}

Great! That is all the code is needed for this all too common situation. You can see from the screenshot below, it works as expected. When you change the selectbox value, then the whole object selectedCompany changes along with it.

{% img /images/ng-option1.png %}  

# Alright, so what's the deal with the the "as" keyword for ng-options? 
The key to understanding this is that the binded value for selectedCompany by default is whatever is in between the "for" and "in" keyword (in our case company). 

We can change that with "as" keyword. Here is an example:
Let's say that instead of binding selectedCompany to the whole company object, we just wanted to bind it to the id of the object. We can achieve this with the "as" keyword.

{% raw %}
```
<span>selectedCompany {{selectedCompany}}</span>
<select ng-model="selectedCompany" ng-options="company.id as company.name for company in companies">
</select>
```

{% endraw %}

{% img /images/ng-option2.png %}  

Notice the flexibility with ng-options. We have the company name as the label, but the model bound value is the company id.

**Just a recap:** The word that is before the "as" keyword is the bound value; it is the value of what you want selectedCompany to be. The word that follows the "as" keyword is the option label. The word that follows the "for" keyword is whatever you want to refer each individual element as. Finally, the word after "in" is the array that you are using for ng-options.

With the "as" syntax, the following 2 selects would be equivalent: 
```html
<select ng-model="selectedCompany" ng-options="company.name for company in companies">
</select>
<select ng-model="selectedCompany" ng-options="company as company.name for company in companies">
</select>
```

#Conclusion: 
ng-options provides a flexible interface for dealing with the html select box and it's options. It is the angular way and you will definitely come across and have to learn it if you are writing production angular code. Although there are a few more things you can do with ng-options (using it with objects instead of arrays, grouping options, etc.), I am going to end here with an excerpt from the angular docs:

> "In many cases, ngRepeat can be used on option elements instead of ngOptions to achieve a similar result. However, ngOptions provides some benefits such as reducing memory and increasing speed by not creating a new scope for each repeated instance, as well as providing more flexibility in how the select's model is assigned via the select as part of the comprehension expression. ngOptions should be used when the select model needs to be bound to a non-string value. This is because an option element can only be bound to string values at present."

## USE ng-options!


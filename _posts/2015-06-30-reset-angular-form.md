---
layout: post
title: How to properly reset Angular ng-form  
---

To validate fields and show error messages I am using this way:
{% highlight javascript %}
<style>
    input:required {
        box-shadow: none;
    }
    input:invalid {
        box-shadow: none;
    }
    .error {
        box-shadow: 1px 1px #ff0000;
    }
</style>
<form name="form">
    <input class="input" name="title"
           ng-model="title"
           ng-required="true"
           ng-pattern="/^[a-z]*$/i"
           ng-class="{'error' : !form.title.$valid && form.title.$dirty}">
    </label>
    <span ng-if="!form.title.$valid && form.title.$dirty">
        The title is wrong
    </span>
</form>
{% endhighlight %}

Let's explain. When you loading the page, input will mark as invalid (first of all because it empty) and get the red shadow. See such a error quite odd, bacause you even didn't write anything. So to set invalid in proper time, use `$dirty` flag. If it's true that means the field was changed. And when `$dirty` is false and `$valid` is true, you are really write something wrong.

Now let's suppouse we wrote proper value in input and click button which should save the value somewhere and reset the form.
{% highlight javascript %}
$scope.title = ''
{% endhighlight %}

Will work, but your `$dirty` flag still is true. And just add this line after reseting the form:
{% highlight javascript %}
$scope.form.$setPristine();
{% endhighlight %}

Ok, now it's working.

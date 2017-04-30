---
layout: post
title:  "Spring asserts"
date:   2017-04-28 00:00:00 +0200
categories: spring code-style
tags: [spring, error-handling, code-style, java]
---
Everywhere there is stated that `do not ever trust user input` but when you create a library the methods should also be aware of the inputvalues.

Though you can never be sure that the method is called with correct input (at least for public methods which can be called from the outside of your library). You should ever check input parameters for validity.

But when you do this you can end up with rather complicated to read methods with many if block like the following easy example.
{% highlight java %}
public static Integer addPositiveNumbers(Integer operand 1, Integer operand 2){
  if(Objects.isNull(operand1)||Objects.isNull(operand2)){
    throw new IllegalArgumentException("Operands are not allowed to be null");
  }
  if(operand1<0){
    throw new IllegalArgumentException("Negative Numbers are not allowed as input [Operand1]");
  }
  if(operand2<0){
    throw new IllegalArgumentException("Negative Numbers are not allowed as input [Operand2]");
  }
  return operand1+operand2;
}
{% endhighlight %}

At the first step we can extract the validation to a separate method. So that the code would look like the following:

{% highlight java %}
public static Integer addPositiveNumbers(Integer operand 1, Integer operand 2){
  validatePositiveNumber(operand1);
  validatePositiveNumber(operand2);
  return operand1+operand2;
}

public static void validatePositiveNumber(Integer operand){
  if(Objects.isNull(operand)){
    throw new IllegalArgumentException("Operand is not allowed to be null");
  }
  if(operand<0){
    throw new IllegalArgumentException("Negative Numbers are not allowed as input");
  }
}
{% endhighlight %}

That made our main method more readable but our validatePositiveNumber methods should be further optimized.

{% highlight java %}
public static Integer addPositiveNumbers(Integer operand 1, Integer operand 2){
  validatePositiveNumber(operand1);
  validatePositiveNumber(operand2);
  return operand1+operand2;
}

public static void validatePositiveNumber(Integer operand){
  Assert.nonNull(operand, "Operand is not allowed to be null");
  Assert.isTrue(operand>=0, "Negative Numbres are not allowed as input");
}
{% endhighlight %}

With this handy method from the Spring framework which does not really do something different than we did before by hand.
We end up with much more readable code.

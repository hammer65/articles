# Switch and Case statements

## Swtich statements. When to use them

My son is learning Java and he asked me today about the use of switch statements. I thought this might be a good tutorial subject so here we go.

Not all programming languages have switch (sometimes called case statements) but certainly many do. You could do a lot of coding and never use a switch/case because the construct really doesn't solve any special problems that can't be handled other ways, however it is very readable and this can be important.

One thing you should always keep in mind when programming is that if you are working on anything of any importance at all, it is likely that someone may have to come along and decipher quickly what you are trying to do with your code and either fix something that is wrong with it, or expand it's capabilities. Readability then comes into play. How easy is it to glance at a piece of code and understand what it's doing?

Sure, there are plenty of other considerations when writing code like speed, error handling and security, so readability is just one more of those things to juggle. Luckily this is easier than it may sound if you get into certain habits.

## If and Switch

Both if and switch constructs allow you to execute code conditionally. If a condition is true, then the code executes if it isn't it doesn't execute. That could be the end of it but maybe you need to check for multiple conditions. A switch statement will make one evaluation and conditionally execute code based on different results of that evaluation. For instance if you have a variable which could be any one of 5 different values and depending on each of these five values different code must be executed a `switch/case` statement is a good choice.

Lets take a menu of food choices as an example. The user can choose from Chicken, Beef, Pork, Fish or Salad. Depending on which one they chose different code might need to be executed. You can certainly do that with an `if/else` if construct, but is it readable and is it easy to code? Maybe, maybe not.

*here's a PHP example*
```javascript
if ($food == 'Chicken'){
// execute Chicken specific code
} else if ($food == 'Beef'){
// execute Beef specific code
} else if ($food == 'Pork'){
// execute Pork related code
} else if ($food == 'Fish'){
// execute Fish related code
} else if ($food == 'Salad'){
// execute Salad related code
}
```

Okay looks fairly orderly but now lets see what a switch can do in the same situation. again in PHP

```javascript
switch($food){
  case 'Chicken':
    // execute Chicken related code
    break;
  case 'Beef':
    // execute Beef related code
    break;
  case 'Pork':
    // execute Pork related code
    break;
  case 'Fish':
    // execute Fish related code
    break;
  case 'Salad':
    // execute Salad related code
}
```

Scanning this to understand it is much easier and there is not a lot of redundant `$food == "x"` stuff going on. You might say though "but I don't code in PHP". Well no problem, the syntax for most languages isn't really that different. In fact Even SQL (structured query language) used in relational databases like MySQL, Postgres and SQL Server has a switch statement.

*SQL*
```sql
CASE food
  WHEN 'Chicken'
    THEN /* chicken code*/
  WHEN 'Beef'
    THEN /* Beef code */
  WHEN 'Pork'
    THEN /* Pork code */
  WHEN 'Fish'
    THEN /* Fish code */
  WHEN 'Salad'
    THEN /* Salad code */
END
```

**C, C++, D, Java, Javascript, Haxe, Actionscript**
```javascript
switch(food){
  case 'Chicken':
    // Chicken code
    break;
  case 'Beef':
    // Beef code
    break;
  case 'Pork':
    // Pork code
    break;
  case 'Fish':
    // Fish Code
    break;
  case 'Salad':
    // Salad Code
}
```

**Perl**
```perl
swich($food){
  case 'Chicken'{
    # Chicken code
  }
  case 'Beef'{
    # Beef code
  }
  case 'Pork'{
    # Pork code
  }
  case 'Fish'{
    # Fish code
  }
  case 'Salad'{
    # Salad code
  }
}
```

**Ruby**
```ruby
case food
  when 'Chicken'
    # Chicken code
  when 'Beef'
    # Beef code
  when 'Pork'
    # Pork code
  when 'Fish'
    # Fish code
  when 'Salad'
    # Salad code
end
```

**Python?**

Nope sorry no switch statement in Python

Now while there are definitely some differences between `C`, `C++`, `D`, `Java`, `Javascript`, `Haxe` and `Actionscript`, those differences don't show up in basic language constructs like `switch`. When you start learning more than one language you will begin to realize that some features are going to be exactly the same while others only have minor differences. Others still are very different and fundamental to the way the language works. It shouldn't scare you. If some features are exactly the same you have less to learn.

## Default

if none of the cases match in a swtich/case statement all of these languages have a `default` condition. In `C` based languages this is usually expressed this way.

```javascript
switch(food){
 case 'Salad':
   // Salad code
 default:
   // if there is no match to any case we execute code here
}
```

Some languages might also use the keyword "else".

## Break on through to the other side

If two or more of your cases can share code then we need a way to "fall through" a case to the next one. This might act something like

```javascript
if(food == 'Chicken' OR food == 'Beef'){
  // Execute code for Chicken or Beef
}
```

But in this case we don't really combine conditions we just omit a keyword you probably noticed in many of the examples above. The `break` keyword. Which "breaks execution out of a block" such as a `switch/case`. Once you are done executing the code for that choice, it breaks out of the switch and we go to the next code whatever that may be.

```javascript
Switch(food){
  case 'Chicken':
  case 'Beef':
    // Chicken "falls through" to Beef. code here for both
    break;
  case 'Pork':
    // Pork code
    break;
  case 'Fish':
    // Fish code
    break;
  case 'Salad'
    // Salad code
    break;
  default:
    alert('Whatz a matter you! You not eh hungry???')
}
```

The `break` keyword is also used in loops to break out of the loop. Here it is a very integral part of how a swtich or case statement works. If you are in the last `case` of a `swtich`, you need not include the break keyword. You code won't wrap back to the beginning or anything it will just leave the swtich statement and go to the next piece of code.

## In Summary
A `switch` and `case` statement is like taking a common source of electricity and routing it to different light bulbs depending on properties of the electricity. We used a "food" variable but we could use any sort of statement with multiple possible results to evaluate. It could be a mathematical equation.

A good general rule with `switch` and `case` statements is that if you have more than three or four results to test for the same statement, use `swtich` and `case` instead of an if construct.

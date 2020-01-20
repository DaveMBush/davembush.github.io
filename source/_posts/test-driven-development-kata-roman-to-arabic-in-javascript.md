---
title: Test Driven Development Kata - Roman to Arabic in JavaScript
tags:
  - javascript
  - kata
  - tdd
url: 3992.html
id: 3992
categories:
  - TDD
date: 2016-07-20 06:30:00
---

Coding Katas are a way of developing your skills as a programmer.  I thought it might be informative to tackle one of the classics as a blog post. Depending on how this works, I may or may not do another one quite so publicly. The rules I’m going to try to adhere by.

1.  I will document what I am doing as I go.
2.  This is not a pre-coded blog post.  You’ll get to “see” me code as I go.
3.  I will write all tests first.
4.  I will only write enough code to make the current tests succeed.

  ![Test Driven Kata - Roman to Arabic](/uploads/2016/07/image-3.png "Test Driven Kata - Roman to Arabic")

Today’s problem:
----------------

This is a pretty classic coding problem that shows up in interviews, home-work assignments, and code katas.

As an interview problem, I find it lacking because it typically does not represent the kind of work you will be doing other than proving that you can solve problems in your chosen language.  It is also a standard coding problem, meaning the person interviewing you is using the most obvious problem to see if you can code.  Much like asking "What is your greatest strength?" and "What is your greatest weakness?"  Finally, it takes longer to complete than I believe interview coding problems should.  This is not to say, I don't jump through these hoops myself.

As a kata exercise, it is good because there are several ways you might solve the problem.  And for our purposes, it also demonstrates what Test Driven Development might look like using JavaScript.

So, what is the problem? Write a function that converts Roman numbers into Arabic numbers and throws an error if the Roman number is in an invalid form.

That sounds pretty easy.  But the first question we need to ask is, “What, exactly are the rules for converting Roman numbers into Arabic numbers?”

1.  The values for Roman numbers are as follows:

| Roman | Arabic |
|:-----:|:------:|
| I     | 1      |
| V     | 5      |
| X     | 10     |
| L     | 50     |
| C     | 100    |
| D     | 500    |
| M     | 1000   |

2.  Repeating a number up to three times adds that value three times.
3.  A Roman ‘digit’ can’t repeat more than three times.  Instead the previous 1, 10, or 100 equivalent value is used to subtract from the next ‘digit’.  That is,

| Roman | Arabic |
|:-----:|:------:|
| IV    | 4      |
| IX    | 9      |
| XL    | 40     |
| XC    | 90     |
| CD    | 400    |
| CM    | 900    |

4.  With the exception of the subtraction rule above, all values must decrease in scale from left to right and are added together.

Test 1
------

Since we will be using JavaScript, the testing framework we will be using is Jasmine.

Our first test will simply test that when we pass in “I” we get back 1.

``` javascript
describe('tests/roman-to-arabic/RomanToArabic.spec.js',function(){
    var returnValue;
    describe('When I is passed in',function(){
        beforeEach(function(){
            returnValue = romanToArabic("I");
        });
        it('should return 1',function(){
            expect(returnValue).toBe(1);
        });
    })
});
```

And the code that passes this test is:

``` javascript
function romanToArabic(romanNumber){
    return 1;
}
```

You might think, what’s the point?  Why just return 1 when you know you are going to have to do more?  Well, when you are doing TDD, you have to work off of what you are testing for now, not what you might test for later.  So, we return 1.

Test 2
------

From here, we can go a couple of different directions.  How about testing for rule 3 next.  How would we do that? To start with, we might just make sure that if we pass in IV, we get back 4.  Now our code is getting a bit more complicated.  But sticking with our TDD principles, we’ll just test for IV.  Another happy path test.

``` javascript
describe('tests/roman-to-arabic/RomanToArabic.spec.js',function(){
    var returnValue;
    describe('When I is passed in',function(){
        beforeEach(function(){
            returnValue = romanToArabic("I");
        });
        it('should return 1',function(){
            expect(returnValue).toBe(1);
        });
    })
    describe('When IV is passed in', function(){
        beforeEach(function(){
            returnValue = romanToArabic("IV");
        });
        it('should return 4',function(){
            expect(returnValue).toBe(4);
        });
    })
});
```

And the code that implements it:

``` javascript
function romanToArabic(romanNumber){
    switch(romanNumber){
        case 'I':
            return 1;
        case 'IV':
            return 4;
    }
}
```

Test 3
------

OK.  What happens if IV shows up twice?  That should be a failure.  IV should never show up more than once.  In fact, none of the codes that show up in rule three should show up more than once.  Let’s make sure they don’t.

The tests for this will be pretty simple, and to save time, we will code for all of them at once.  In fact, we are going to eventually need the conversion tables above, so let’s go ahead and put them in now.

Here is what our test file looks like now.  Notice that we’ve put several tests in at once.

Notice that I didn’t have to write multiple tests to do this.  I just created one test and iterated over it.

``` javascript
describe('tests/roman-to-arabic/RomanToArabic.spec.js',function(){
    var returnValue;
    var minusOneTable = {
        IV: 4,
        IX: 9,
        XL: 40,
        XC: 90,
        CD: 400,
        CM: 900
    };
    describe('When I is passed in',function(){
        beforeEach(function(){
            returnValue = romanToArabic('I');
        });
        it('should return 1',function(){
            expect(returnValue).toBe(1);
        });
    });
    describe('When IV is passed in', function(){
        beforeEach(function(){
            returnValue = romanToArabic('IV');
        });
        it('should return 4',function(){
            expect(returnValue).toBe(4);
        });
    });
    for(var prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy+propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
});
```

And the solution also iterates.

``` javascript
function romanToArabic(romanNumber){
    var minusOneTable = {
        IV: 4,
        IX: 9,
        XL: 40,
        XC: 90,
        CD: 400,
        CM: 900
    };
    var baseTable = {
        I: 1,
        V: 5,
        X: 10,
        L: 50,
        C: 100,
        D: 500,
        M: 1000
    };

    // This block is probably inefficient, but it is
    // easy to reason about.
    for(var prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
           continue;
        }
        var rx = new RegExp(prop,'g');
        if((romanNumber.match(rx) || []).length > 1){
            throw 'Poorly formed Roman number!';
        }

    }

    switch(romanNumber){
        case 'I':
            return 1;
        case 'IV':
            return 4;
    }
}
```

Test 4
------

Just to make sure we aren’t counting how many times in a row this all shows up, let’s add a valid Roman Number in between.

``` javascript
describe('tests/roman-to-arabic/RomanToArabic.spec.js',function(){
    var returnValue;
    var minusOneTable = {
        IV: 4,
        IX: 9,
        XL: 40,
        XC: 90,
        CD: 400,
        CM: 900
    };
    describe('When I is passed in',function(){
        beforeEach(function(){
            returnValue = romanToArabic('I');
        });
        it('should return 1',function(){
            expect(returnValue).toBe(1);
        });
    });
    describe('When IV is passed in', function(){
        beforeEach(function(){
            returnValue = romanToArabic('IV');
        });
        it('should return 4',function(){
            expect(returnValue).toBe(4);
        });
    });
    var prop;
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy+propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + 'I' + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy + 'I' + propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
});
```

Test 5
------

Next, we need to make sure that none of the items in our base table show up more than 3 times.  We’ll write a similar test to what we’ve already written with a twist.  If any of those numerals show up more than 4 times, we know we have a problem either because they are out of order or because they show up all in a row.  So, we only need to check the count.  If there are four of any of them, we have a problem.

``` javascript
describe('tests/roman-to-arabic/RomanToArabic.spec.js',function(){
    var returnValue;
    var minusOneTable = {
        IV: 4,
        IX: 9,
        XL: 40,
        XC: 90,
        CD: 400,
        CM: 900
    };
    var baseTable = {
        I: 1,
        V: 5,
        X: 10,
        L: 50,
        C: 100,
        D: 500,
        M: 1000
    };
    describe('When I is passed in',function(){
        beforeEach(function(){
            returnValue = romanToArabic('I');
        });
        it('should return 1',function(){
            expect(returnValue).toBe(1);
        });
    });
    describe('When IV is passed in', function(){
        beforeEach(function(){
            returnValue = romanToArabic('IV');
        });
        it('should return 4',function(){
            expect(returnValue).toBe(4);
        });
    });
    var prop;
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy+propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + 'I' + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy + 'I' + propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in baseTable){
        if(!baseTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + prop + prop + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy+propCopy+propCopy+propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
});
```

And the code to implement it.function romanToArabic(romanNumber){
    var minusOneTable = {
        IV: 4,
        IX: 9,
        XL: 40,
        XC: 90,
        CD: 400,
        CM: 900
    };
    var baseTable = {
        I: 1,
        V: 5,
        X: 10,
        L: 50,
        C: 100,
        D: 500,
        M: 1000
    };

    // This block is probably inefficient, but it is
    // easy to reason about.
    for(var prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
           continue;
        }
        var rx = new RegExp(prop,'g');
        if((romanNumber.match(rx) || []).length > 1){
            throw 'Poorly formed Roman number!';
        }

    }
    for(var prop in baseTable){
        if(!baseTable.hasOwnProperty(prop)){
            continue;
        }
        var rx = new RegExp(prop,'g');
        if((romanNumber.match(rx) || []).length > 3){
            throw 'Poorly formed Roman number!';
        }

    }

    switch(romanNumber){
        case 'I':
            return 1;
        case 'IV':
            return 4;
    }
}

Test 6
------

So, before we move on to actually computing the value of the Roman number, we should ask ourselves if there are any other ways a Roman number could be passed in incorrectly.

The next one that occurs to me is this.  If you have a number that contains a ‘digit’ from the minusOneTable, the character that is used to subtract should never follow the digit.  For example, if “IV” shows up, there should not be an “I” immediately after it.  That is, we shouldn’t see “IVI” anywhere in our string.  So let’s add that to our tests.

``` javascript
describe('tests/roman-to-arabic/RomanToArabic.spec.js',function(){
    var returnValue;
    var minusOneTable = {
        IV: 4,
        IX: 9,
        XL: 40,
        XC: 90,
        CD: 400,
        CM: 900
    };
    var baseTable = {
        I: 1,
        V: 5,
        X: 10,
        L: 50,
        C: 100,
        D: 500,
        M: 1000
    };
    describe('When I is passed in',function(){
        beforeEach(function(){
            returnValue = romanToArabic('I');
        });
        it('should return 1',function(){
            expect(returnValue).toBe(1);
        });
    });
    describe('When IV is passed in', function(){
        beforeEach(function(){
            returnValue = romanToArabic('IV');
        });
        it('should return 4',function(){
            expect(returnValue).toBe(4);
        });
    });
    var prop;
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy+propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + 'I' + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy + 'I' + propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in baseTable){
        if(!baseTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + prop + prop + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy+propCopy+propCopy+propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        var newProp = prop + prop.substr(0,1);
        describe('When ' + newProp + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy)}).toThrow();
            });
        }).bind(this,newProp));
    }
});
```

And the code to implement it:

``` javascript
function romanToArabic(romanNumber){
    var minusOneTable = {
        IV: 4,
        IX: 9,
        XL: 40,
        XC: 90,
        CD: 400,
        CM: 900
    };
    var baseTable = {
        I: 1,
        V: 5,
        X: 10,
        L: 50,
        C: 100,
        D: 500,
        M: 1000
    };

    // This block is probably inefficient, but it is
    // easy to reason about.
    var prop;
    var rx;
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
           continue;
        }
        rx = new RegExp(prop,'g');
        if((romanNumber.match(rx) || []).length > 1){
            throw 'Poorly formed Roman number!';
        }
        rx = new RegExp(prop + prop.substr(0,1),'g');
        if((romanNumber.match(rx) || []).length > 0){
            throw 'Poorly formed Roman number';
        }

    }
    for(prop in baseTable){
        if(!baseTable.hasOwnProperty(prop)){
            continue;
        }
        rx = new RegExp(prop,'g');
        if((romanNumber.match(rx) || []).length > 3){
            throw 'Poorly formed Roman number!';
        }

    }

    switch(romanNumber){
        case 'I':
            return 1;
        case 'IV':
            return 4;
    }
}
```

Step 7
------

There is one more possible problem we could encounter.  What if someone passes in a character that isn’t a valid Roman number character.  We need to make sure that the only characters that show up are Roman number characters.

Since testing for all of the characters isn’t practical, we are just going to test for a few and assume that if this were a real business problem we’d go to the trouble of testing more.  But, the basic test is going to look the same.  Toss in bad characters in an otherwise valid string and make sure we throw an error.

``` javascript
describe('tests/roman-to-arabic/RomanToArabic.spec.js',function(){
    var returnValue;
    var minusOneTable = {
        IV: 4,
        IX: 9,
        XL: 40,
        XC: 90,
        CD: 400,
        CM: 900
    };
    var baseTable = {
        I: 1,
        V: 5,
        X: 10,
        L: 50,
        C: 100,
        D: 500,
        M: 1000
    };
    describe('When I is passed in',function(){
        beforeEach(function(){
            returnValue = romanToArabic('I');
        });
        it('should return 1',function(){
            expect(returnValue).toBe(1);
        });
    });
    describe('When IV is passed in', function(){
        beforeEach(function(){
            returnValue = romanToArabic('IV');
        });
        it('should return 4',function(){
            expect(returnValue).toBe(4);
        });
    });
    var prop;
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy+propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + 'I' + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy + 'I' + propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in baseTable){
        if(!baseTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + prop + prop + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy+propCopy+propCopy+propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        var newProp = prop + prop.substr(0,1);
        describe('When ' + newProp + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy)}).toThrow();
            });
        }).bind(this,newProp));
    }
    // just test for one each of upper case, lower case, number and lower case valid character
    var invalidCharacters = {A:1,a:1,'2': 1, i: 1};
    for(prop in invalidCharacters){
        if(!invalidCharacters.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + ' is added',(function(propCopy){
            it('should throw an exception',function() {
                expect(function () {romanToArabic(propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
});
```

And the solution, which is pretty simple:

``` javascript
function romanToArabic(romanNumber){
    var minusOneTable = {
        IV: 4,
        IX: 9,
        XL: 40,
        XC: 90,
        CD: 400,
        CM: 900
    };
    var baseTable = {
        I: 1,
        V: 5,
        X: 10,
        L: 50,
        C: 100,
        D: 500,
        M: 1000
    };

    // This block is probably inefficient, but it is
    // easy to reason about.
    var prop;
    var rx;
    // make sure minusOne only shows up once
    // and first character isn't also the last character. (IVI for example)
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
           continue;
        }
        rx = new RegExp(prop,'g');
        if((romanNumber.match(rx) || []).length > 1){
            throw 'Poorly formed Roman number!';
        }
        rx = new RegExp(prop + prop.substr(0,1),'g');
        if((romanNumber.match(rx) || []).length > 0){
            throw 'Poorly formed Roman number';
        }

    }

    var included = '';
    // make sure digits only show up 3 times

    for(prop in baseTable){
        if(!baseTable.hasOwnProperty(prop)){
            continue;
        }
        included += prop;
        rx = new RegExp(prop,'g');
        if((romanNumber.match(rx) || []).length > 3){
            throw 'Poorly formed Roman number!';
        }
    }

    // make sure only I, V, X, L, C and D are the only characters that show up
    rx = new RegExp('[^' + included + ']','g')
    if((romanNumber.match(rx) || []).length > 0){
        throw 'Poorly formed Roman number';
    }


    switch(romanNumber){
        case 'I':
            return 1;
        case 'IV':
            return 4;
    }
}
```

Step 8
------

OK. I think that gets all of the validations except for ensuring that the numbers show up in numerical order.  To make this work, we are going to need to work through the array and assign each digit a value.  The test we need to write is going to put values in the wrong order.

Our test will put the next value up after the value.

``` javascript
describe('tests/roman-to-arabic/RomanToArabic.spec.js',function(){
    var returnValue;
    var minusOneTable = {
        IV: 4,
        IX: 9,
        XL: 40,
        XC: 90,
        CD: 400,
        CM: 900
    };
    var baseTable = {
        I: 1,
        V: 5,
        X: 10,
        L: 50,
        C: 100,
        D: 500,
        M: 1000
    };

    describe('When I is passed in',function(){
        beforeEach(function(){
            returnValue = romanToArabic('I');
        });
        it('should return 1',function(){
            expect(returnValue).toBe(1);
        });
    });
    describe('When IV is passed in', function(){
        beforeEach(function(){
            returnValue = romanToArabic('IV');
        });
        it('should return 4',function(){
            expect(returnValue).toBe(4);
        });
    });
    var prop;
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy+propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + 'I' + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy + 'I' + propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in baseTable){
        if(!baseTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + prop + prop + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy+propCopy+propCopy+propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        var newProp = prop + prop.substr(0,1);
        describe('When ' + newProp + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy)}).toThrow();
            });
        }).bind(this,newProp));
    }
    // just test for one each of upper case, lower case, number and lower case valid character
    var invalidCharacters = {A:1,a:1,'2': 1, i: 1};
    for(prop in invalidCharacters){
        if(!invalidCharacters.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + ' is added',(function(propCopy){
            it('should throw an exception',function() {
                expect(function () {romanToArabic(propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    var badOrderPairs = {DM: 1, LC: 1, VX: 1, IXXL: 1, IXM: 1};
    for(prop in badOrderPairs){
        if(!badOrderPairs.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + ' is added',(function(propCopy){
            it('should throw an exception',function() {
                expect(function () {romanToArabic(propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
});
```

And the solution will put them in order and add them along the way.

``` javascript
function romanToArabic(romanNumber){
    var minusOneTable = {
        IV: 4,
        IX: 9,
        XL: 40,
        XC: 90,
        CD: 400,
        CM: 900
    };
    var baseTable = {
        I: 1,
        V: 5,
        X: 10,
        L: 50,
        C: 100,
        D: 500,
        M: 1000
    };
    var minusOneSub = {
        IV: 'v',
        IX: 'x',
        XL: 'l',
        XC: 'c',
        CD: 'd',
        CM: 'm'
    };
    var compositeValueTable = {
        I: 1,
        v: 4,
        V: 5,
        x: 9,
        X: 10,
        l: 40,
        L: 50,
        c: 90,
        C: 100,
        d: 400,
        D: 500,
        m: 900,
        M: 1000
    };

    // This block is probably inefficient, but it is
    // easy to reason about.
    var prop;
    var rx;
    // make sure minusOne only shows up once
    // and first character isn't also the last character. (IVI for example)
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
           continue;
        }
        rx = new RegExp(prop,'g');
        if((romanNumber.match(rx) || []).length > 1){
            throw 'Poorly formed Roman number!';
        }
        rx = new RegExp(prop + prop.substr(0,1),'g');
        if((romanNumber.match(rx) || []).length > 0){
            throw 'Poorly formed Roman number';
        }
    }

    var included = '';
    // make sure digits only show up 3 times

    for(prop in baseTable){
        if(!baseTable.hasOwnProperty(prop)){
            continue;
        }
        included += prop;
        rx = new RegExp(prop,'g');
        if((romanNumber.match(rx) || []).length > 3){
            throw 'Poorly formed Roman number!';
        }
    }

    // make sure only I, V, X, L, C and D are the only characters that show up
    rx = new RegExp('[^' + included + ']','g');
    if((romanNumber.match(rx) || []).length > 0){
        throw 'Poorly formed Roman number';
    }

    // substitute the minusOnes with tokens we can use to compute value.
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        rx = new RegExp(prop,'g');
        romanNumber = romanNumber.replace(rx,minusOneSub[prop]);
    }

    var romanNumberArray = romanNumber.split('');
    var returnValue = 0;
    var lastValue = 0;
    var currentValue = 0;
    for(var numberIndex = 0;numberIndex < romanNumberArray.length;numberIndex++){
        currentValue = compositeValueTable[romanNumberArray[numberIndex]];
        returnValue += currentValue;
        if(numberIndex > 0 && currentValue > lastValue){
            throw 'Poorly formed Roman number';
        }
        lastValue = currentValue;
    }
    return returnValue;

    // switch(romanNumber){
    //     case 'I':
    //         return 1;
    //     case 'IV':
    //         return 4;
    // }
}
```

Step 9
------

And finally we add some test to verify that we get the right result.

``` javascript
describe('tests/roman-to-arabic/RomanToArabic.spec.js',function(){
    var returnValue;
    var minusOneTable = {
        IV: 4,
        IX: 9,
        XL: 40,
        XC: 90,
        CD: 400,
        CM: 900
    };
    var baseTable = {
        I: 1,
        V: 5,
        X: 10,
        L: 50,
        C: 100,
        D: 500,
        M: 1000
    };

    describe('When I is passed in',function(){
        beforeEach(function(){
            returnValue = romanToArabic('I');
        });
        it('should return 1',function(){
            expect(returnValue).toBe(1);
        });
    });
    describe('When IV is passed in', function(){
        beforeEach(function(){
            returnValue = romanToArabic('IV');
        });
        it('should return 4',function(){
            expect(returnValue).toBe(4);
        });
    });
    var prop;
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy+propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + 'I' + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy + 'I' + propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in baseTable){
        if(!baseTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + prop + prop + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy+propCopy+propCopy+propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        var newProp = prop + prop.substr(0,1);
        describe('When ' + newProp + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy)}).toThrow();
            });
        }).bind(this,newProp));
    }
    // just test for one each of upper case, lower case, number and lower case valid character
    var invalidCharacters = {A:1,a:1,'2': 1, i: 1};
    for(prop in invalidCharacters){
        if(!invalidCharacters.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + ' is added',(function(propCopy){
            it('should throw an exception',function() {
                expect(function () {romanToArabic(propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    var badOrderPairs = {DM: 1, LC: 1, VX: 1, IXXL: 1, IXM: 1};
    for(prop in badOrderPairs){
        if(!badOrderPairs.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + ' is added',(function(propCopy){
            it('should throw an exception',function() {
                expect(function () {romanToArabic(propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    var romanNumbers = {
        I: 1,
        II: 2,
        III: 3,
        IV: 4,
        V: 5,
        VI: 6,
        VII: 7,
        VIII: 8,
        IX: 9,
        X: 10,
        XI: 11,
        XII: 12,
        XIII: 13,
        XIV: 14,
        XV: 15,
        XVI: 16,
        XVII: 17,
        XVIII: 18,
        XIX: 19,
        XX: 20,
        XXI: 21,
        XXII: 22,
        XXIII: 23,
        XXIV: 24,
        XXV: 25,
        XXVI: 26,
        XXVII: 27,
        XXVIII: 28,
        XXIX: 29,
        XXX: 30,
        XL: 40,
        XLIV: 44,
        XLV: 45,
        XLVI: 46,
        L: 50,
        LIV: 54,
        LV: 55,
        LVI: 56,
        LX: 60,
        XC: 90,
        C: 100,
        CI: 101,
        CXI: 111,
        CMXCIX: 999,
        MDCLXVI: 1666

    };
    for(prop in romanNumbers){
        if(!romanNumbers.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + ' is added',(function(propCopy){
            beforeEach(function(){
                returnValue = romanToArabic(propCopy);
            });
            it('should return ' + romanNumbers[propCopy] ,function() {
                expect(returnValue).toBe(romanNumbers[propCopy]);
            });
        }).bind(this,prop));
    }
});
```

Review
------

What?! I thought we were done? Well, we are and we aren’t.  We have code that works.  But is it the best code we can write? Here is what I like about this code:

1.  It is easy to reason about.
2.  It allows me to add additional roman numbers by expanding my tables.  No additional code would be needed. (And while it is hard to represent using Arabic letters, there are additional symbols.)
3.  It works reasonably fast.

But, it does seem to me that we might make it a bit more efficient without sacrificing these advantages too much.  And the great news is, since we have our test in place, we can refactor with impunity.  No worries about breaking something because we will know as soon as we have and we can revert back to the code that was working.

Step 12
-------

One pretty simple change we can make is that our error message is scattered throughout our code.  Let’s make that a variable and just throw the variable.

The other thing I wonder about is how much of our validations can be combined? One check we might safely eliminate at this point is the check to make sure the minus values only show up once.  If any of them were to show up more than once, they would either show up one after the other, which we still can check for, or they would be in the wrong value order which our final check will catch.  So, let’s eliminate that code as well and re-run our tests to make sure nothing broke.

And now that we’ve eliminated that check, we can create one big string for checking conditions like “IVI” into one string and check it once instead of creating a new RegExp object multiple times.

Here is the code we have so far:

``` javascript
describe('tests/roman-to-arabic/RomanToArabic.spec.js',function(){
    var returnValue;
    var minusOneTable = {
        IV: 4,
        IX: 9,
        XL: 40,
        XC: 90,
        CD: 400,
        CM: 900
    };
    var baseTable = {
        I: 1,
        V: 5,
        X: 10,
        L: 50,
        C: 100,
        D: 500,
        M: 1000
    };

    describe('When I is passed in',function(){
        beforeEach(function(){
            returnValue = romanToArabic('I');
        });
        it('should return 1',function(){
            expect(returnValue).toBe(1);
        });
    });
    describe('When IV is passed in', function(){
        beforeEach(function(){
            returnValue = romanToArabic('IV');
        });
        it('should return 4',function(){
            expect(returnValue).toBe(4);
        });
    });
    var prop;
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy+propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + 'I' + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy + 'I' + propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in baseTable){
        if(!baseTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + prop + prop + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy+propCopy+propCopy+propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        var newProp = prop + prop.substr(0,1);
        describe('When ' + newProp + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy)}).toThrow();
            });
        }).bind(this,newProp));
    }
    // just test for one each of upper case, lower case, number and lower case valid character
    var invalidCharacters = {A:1,a:1,'2': 1, i: 1};
    for(prop in invalidCharacters){
        if(!invalidCharacters.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + ' is added',(function(propCopy){
            it('should throw an exception',function() {
                expect(function () {romanToArabic(propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    var badOrderPairs = {DM: 1, LC: 1, VX: 1, IXXL: 1, IXM: 1};
    for(prop in badOrderPairs){
        if(!badOrderPairs.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + ' is added',(function(propCopy){
            it('should throw an exception',function() {
                expect(function () {romanToArabic(propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    var romanNumbers = {
        I: 1,
        II: 2,
        III: 3,
        IV: 4,
        V: 5,
        VI: 6,
        VII: 7,
        VIII: 8,
        IX: 9,
        X: 10,
        XI: 11,
        XII: 12,
        XIII: 13,
        XIV: 14,
        XV: 15,
        XVI: 16,
        XVII: 17,
        XVIII: 18,
        XIX: 19,
        XX: 20,
        XXI: 21,
        XXII: 22,
        XXIII: 23,
        XXIV: 24,
        XXV: 25,
        XXVI: 26,
        XXVII: 27,
        XXVIII: 28,
        XXIX: 29,
        XXX: 30,
        XL: 40,
        XLIV: 44,
        XLV: 45,
        XLVI: 46,
        L: 50,
        LIV: 54,
        LV: 55,
        LVI: 56,
        LX: 60,
        XC: 90,
        C: 100,
        CI: 101,
        CXI: 111,
        CMXCIX: 999,
        MDCLXVI: 1666

    };
    for(prop in romanNumbers){
        if(!romanNumbers.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + ' is added',(function(propCopy){
            beforeEach(function(){
                returnValue = romanToArabic(propCopy);
            });
            it('should return ' + romanNumbers[propCopy] ,function() {
                expect(returnValue).toBe(romanNumbers[propCopy]);
            });
        }).bind(this,prop));
    }
});
```

Step 11
-------

Now that we have our IVI check down to one string, it occurs to me that we can combine this check with our invalid character check.  So, let’s do that next.

``` javascript
describe('tests/roman-to-arabic/RomanToArabic.spec.js',function(){
    var returnValue;
    var minusOneTable = {
        IV: 4,
        IX: 9,
        XL: 40,
        XC: 90,
        CD: 400,
        CM: 900
    };
    var baseTable = {
        I: 1,
        V: 5,
        X: 10,
        L: 50,
        C: 100,
        D: 500,
        M: 1000
    };

    describe('When I is passed in',function(){
        beforeEach(function(){
            returnValue = romanToArabic('I');
        });
        it('should return 1',function(){
            expect(returnValue).toBe(1);
        });
    });
    describe('When IV is passed in', function(){
        beforeEach(function(){
            returnValue = romanToArabic('IV');
        });
        it('should return 4',function(){
            expect(returnValue).toBe(4);
        });
    });
    var prop;
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy+propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + 'I' + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy + 'I' + propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in baseTable){
        if(!baseTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + prop + prop + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy+propCopy+propCopy+propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        var newProp = prop + prop.substr(0,1);
        describe('When ' + newProp + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy)}).toThrow();
            });
        }).bind(this,newProp));
    }
    // just test for one each of upper case, lower case, number and lower case valid character
    var invalidCharacters = {A:1,a:1,'2': 1, i: 1};
    for(prop in invalidCharacters){
        if(!invalidCharacters.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + ' is added',(function(propCopy){
            it('should throw an exception',function() {
                expect(function () {romanToArabic(propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    var badOrderPairs = {DM: 1, LC: 1, VX: 1, IXXL: 1, IXM: 1};
    for(prop in badOrderPairs){
        if(!badOrderPairs.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + ' is added',(function(propCopy){
            it('should throw an exception',function() {
                expect(function () {romanToArabic(propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    var romanNumbers = {
        I: 1,
        II: 2,
        III: 3,
        IV: 4,
        V: 5,
        VI: 6,
        VII: 7,
        VIII: 8,
        IX: 9,
        X: 10,
        XI: 11,
        XII: 12,
        XIII: 13,
        XIV: 14,
        XV: 15,
        XVI: 16,
        XVII: 17,
        XVIII: 18,
        XIX: 19,
        XX: 20,
        XXI: 21,
        XXII: 22,
        XXIII: 23,
        XXIV: 24,
        XXV: 25,
        XXVI: 26,
        XXVII: 27,
        XXVIII: 28,
        XXIX: 29,
        XXX: 30,
        XL: 40,
        XLIV: 44,
        XLV: 45,
        XLVI: 46,
        L: 50,
        LIV: 54,
        LV: 55,
        LVI: 56,
        LX: 60,
        XC: 90,
        C: 100,
        CI: 101,
        CXI: 111,
        CMXCIX: 999,
        MDCLXVI: 1666

    };
    for(prop in romanNumbers){
        if(!romanNumbers.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + ' is added',(function(propCopy){
            beforeEach(function(){
                returnValue = romanToArabic(propCopy);
            });
            it('should return ' + romanNumbers[propCopy] ,function() {
                expect(returnValue).toBe(romanNumbers[propCopy]);
            });
        }).bind(this,prop));
    }


});
```

Step 12
-------

And the last place we can optimize is our three digit check.  But instead of limiting it to valid Roman numbers, let’s just check for any sequence of characters that repeats 4 times or more.

And once we know that is working, we can combine it with our other Regular Expressions.

``` javascript
describe('tests/roman-to-arabic/RomanToArabic.spec.js',function(){
    var returnValue;
    var minusOneTable = {
        IV: 4,
        IX: 9,
        XL: 40,
        XC: 90,
        CD: 400,
        CM: 900
    };
    var baseTable = {
        I: 1,
        V: 5,
        X: 10,
        L: 50,
        C: 100,
        D: 500,
        M: 1000
    };

    describe('When I is passed in',function(){
        beforeEach(function(){
            returnValue = romanToArabic('I');
        });
        it('should return 1',function(){
            expect(returnValue).toBe(1);
        });
    });
    describe('When IV is passed in', function(){
        beforeEach(function(){
            returnValue = romanToArabic('IV');
        });
        it('should return 4',function(){
            expect(returnValue).toBe(4);
        });
    });
    var prop;
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy+propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + 'I' + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy + 'I' + propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in baseTable){
        if(!baseTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + prop + prop + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy+propCopy+propCopy+propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        var newProp = prop + prop.substr(0,1);
        describe('When ' + newProp + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy)}).toThrow();
            });
        }).bind(this,newProp));
    }
    // just test for one each of upper case, lower case, number and lower case valid character
    var invalidCharacters = {A:1,a:1,'2': 1, i: 1};
    for(prop in invalidCharacters){
        if(!invalidCharacters.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + ' is added',(function(propCopy){
            it('should throw an exception',function() {
                expect(function () {romanToArabic(propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    var badOrderPairs = {DM: 1, LC: 1, VX: 1, IXXL: 1, IXM: 1};
    for(prop in badOrderPairs){
        if(!badOrderPairs.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + ' is added',(function(propCopy){
            it('should throw an exception',function() {
                expect(function () {romanToArabic(propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    var romanNumbers = {
        I: 1,
        II: 2,
        III: 3,
        IV: 4,
        V: 5,
        VI: 6,
        VII: 7,
        VIII: 8,
        IX: 9,
        X: 10,
        XI: 11,
        XII: 12,
        XIII: 13,
        XIV: 14,
        XV: 15,
        XVI: 16,
        XVII: 17,
        XVIII: 18,
        XIX: 19,
        XX: 20,
        XXI: 21,
        XXII: 22,
        XXIII: 23,
        XXIV: 24,
        XXV: 25,
        XXVI: 26,
        XXVII: 27,
        XXVIII: 28,
        XXIX: 29,
        XXX: 30,
        XL: 40,
        XLIV: 44,
        XLV: 45,
        XLVI: 46,
        L: 50,
        LIV: 54,
        LV: 55,
        LVI: 56,
        LX: 60,
        XC: 90,
        C: 100,
        CI: 101,
        CXI: 111,
        CMXCIX: 999,
        MDCLXVI: 1666

    };
    for(prop in romanNumbers){
        if(!romanNumbers.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + ' is added',(function(propCopy){
            beforeEach(function(){
                returnValue = romanToArabic(propCopy);
            });
            it('should return ' + romanNumbers[propCopy] ,function() {
                expect(returnValue).toBe(romanNumbers[propCopy]);
            });
        }).bind(this,prop));
    }
});
```

Step 13
-------

We’ve cleaned up about all of the logic we can without hard coding the values.  But I would like to combine the lookup tables next.  I don’t like having three tables.  Let’s see if we can pull those into one table.

The first step to doing this is combining the minusOneTable and baseTable into one table.  To differentiate in the code that is using those tables, we’ll just look at the length of the property.

In the process of doing this, we notice that it would also make sense to combine the substitution table.  And since the values we’ve assigned to the table aren’t even being used at this point, we’ll just use the substitutions instead of the values.

And if we make the value of each element in the baseTable an array, we could combine the values into that table as well.  But that would over complicate our lookup logic when we compute the value.  So, I think we’ll just leave that as it is.

``` javascript
describe('tests/roman-to-arabic/RomanToArabic.spec.js',function(){
    var returnValue;
    var minusOneTable = {
        IV: 4,
        IX: 9,
        XL: 40,
        XC: 90,
        CD: 400,
        CM: 900
    };
    var baseTable = {
        I: 1,
        V: 5,
        X: 10,
        L: 50,
        C: 100,
        D: 500,
        M: 1000
    };

    describe('When I is passed in',function(){
        beforeEach(function(){
            returnValue = romanToArabic('I');
        });
        it('should return 1',function(){
            expect(returnValue).toBe(1);
        });
    });
    describe('When IV is passed in', function(){
        beforeEach(function(){
            returnValue = romanToArabic('IV');
        });
        it('should return 4',function(){
            expect(returnValue).toBe(4);
        });
    });
    var prop;
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy+propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + 'I' + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy + 'I' + propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in baseTable){
        if(!baseTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + prop + prop + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy+propCopy+propCopy+propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        var newProp = prop + prop.substr(0,1);
        describe('When ' + newProp + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy)}).toThrow();
            });
        }).bind(this,newProp));
    }
    // just test for one each of upper case, lower case, number and lower case valid character
    var invalidCharacters = {A:1,a:1,'2': 1, i: 1};
    for(prop in invalidCharacters){
        if(!invalidCharacters.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + ' is added',(function(propCopy){
            it('should throw an exception',function() {
                expect(function () {romanToArabic(propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    var badOrderPairs = {DM: 1, LC: 1, VX: 1, IXXL: 1, IXM: 1};
    for(prop in badOrderPairs){
        if(!badOrderPairs.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + ' is added',(function(propCopy){
            it('should throw an exception',function() {
                expect(function () {romanToArabic(propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    var romanNumbers = {
        I: 1,
        II: 2,
        III: 3,
        IV: 4,
        V: 5,
        VI: 6,
        VII: 7,
        VIII: 8,
        IX: 9,
        X: 10,
        XI: 11,
        XII: 12,
        XIII: 13,
        XIV: 14,
        XV: 15,
        XVI: 16,
        XVII: 17,
        XVIII: 18,
        XIX: 19,
        XX: 20,
        XXI: 21,
        XXII: 22,
        XXIII: 23,
        XXIV: 24,
        XXV: 25,
        XXVI: 26,
        XXVII: 27,
        XXVIII: 28,
        XXIX: 29,
        XXX: 30,
        XL: 40,
        XLIV: 44,
        XLV: 45,
        XLVI: 46,
        L: 50,
        LIV: 54,
        LV: 55,
        LVI: 56,
        LX: 60,
        XC: 90,
        C: 100,
        CI: 101,
        CXI: 111,
        CMXCIX: 999,
        MDCLXVI: 1666

    };
    for(prop in romanNumbers){
        if(!romanNumbers.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + ' is added',(function(propCopy){
            beforeEach(function(){
                returnValue = romanToArabic(propCopy);
            });
            it('should return ' + romanNumbers[propCopy] ,function() {
                expect(returnValue).toBe(romanNumbers[propCopy]);
            });
        }).bind(this,prop));
    }
});
```

Step 14
-------

Finally, I think we want to consolidate the loops into one loop.

``` javascript
describe('tests/roman-to-arabic/RomanToArabic.spec.js',function(){
    var returnValue;
    var minusOneTable = {
        IV: 4,
        IX: 9,
        XL: 40,
        XC: 90,
        CD: 400,
        CM: 900
    };
    var baseTable = {
        I: 1,
        V: 5,
        X: 10,
        L: 50,
        C: 100,
        D: 500,
        M: 1000
    };

    describe('When I is passed in',function(){
        beforeEach(function(){
            returnValue = romanToArabic('I');
        });
        it('should return 1',function(){
            expect(returnValue).toBe(1);
        });
    });
    describe('When IV is passed in', function(){
        beforeEach(function(){
            returnValue = romanToArabic('IV');
        });
        it('should return 4',function(){
            expect(returnValue).toBe(4);
        });
    });
    var prop;
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy+propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + 'I' + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy + 'I' + propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in baseTable){
        if(!baseTable.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + prop + prop + prop + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy+propCopy+propCopy+propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    for(prop in minusOneTable){
        if(!minusOneTable.hasOwnProperty(prop)){
            continue;
        }
        var newProp = prop + prop.substr(0,1);
        describe('When ' + newProp + ' is added',(function(propCopy){
            it('should throw an exception',function(){
                expect(function(){romanToArabic(propCopy)}).toThrow();
            });
        }).bind(this,newProp));
    }
    // just test for one each of upper case, lower case, number and lower case valid character
    var invalidCharacters = {A:1,a:1,'2': 1, i: 1};
    for(prop in invalidCharacters){
        if(!invalidCharacters.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + ' is added',(function(propCopy){
            it('should throw an exception',function() {
                expect(function () {romanToArabic(propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    var badOrderPairs = {DM: 1, LC: 1, VX: 1, IXXL: 1, IXM: 1};
    for(prop in badOrderPairs){
        if(!badOrderPairs.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + ' is added',(function(propCopy){
            it('should throw an exception',function() {
                expect(function () {romanToArabic(propCopy)}).toThrow();
            });
        }).bind(this,prop));
    }
    var romanNumbers = {
        I: 1,
        II: 2,
        III: 3,
        IV: 4,
        V: 5,
        VI: 6,
        VII: 7,
        VIII: 8,
        IX: 9,
        X: 10,
        XI: 11,
        XII: 12,
        XIII: 13,
        XIV: 14,
        XV: 15,
        XVI: 16,
        XVII: 17,
        XVIII: 18,
        XIX: 19,
        XX: 20,
        XXI: 21,
        XXII: 22,
        XXIII: 23,
        XXIV: 24,
        XXV: 25,
        XXVI: 26,
        XXVII: 27,
        XXVIII: 28,
        XXIX: 29,
        XXX: 30,
        XL: 40,
        XLIV: 44,
        XLV: 45,
        XLVI: 46,
        L: 50,
        LIV: 54,
        LV: 55,
        LVI: 56,
        LX: 60,
        XC: 90,
        C: 100,
        CI: 101,
        CXI: 111,
        CMXCIX: 999,
        MDCLXVI: 1666

    };
    for(prop in romanNumbers){
        if(!romanNumbers.hasOwnProperty(prop)){
            continue;
        }
        describe('When ' + prop + ' is added',(function(propCopy){
            beforeEach(function(){
                returnValue = romanToArabic(propCopy);
            });
            it('should return ' + romanNumbers[propCopy] ,function() {
                expect(returnValue).toBe(romanNumbers[propCopy]);
            });
        }).bind(this,prop));
    }
});
```

Review
------

There are probably a few other optimizations that could be made here.  But I’m afraid that each would sacrifice either the flexibility.  That is, we could hard code the regular expression validations, which would significantly reduce the number of lines of code.  Or we would sacrifice the readability.  Neither of which I am willing to do.

If you are only interested in the result, you can find the finished project at [https://github.com/DaveMBush/JavaScriptKatas](//github.com/DaveMBush/JavaScriptKatas "https://github.com/DaveMBush/JavaScriptKatas")

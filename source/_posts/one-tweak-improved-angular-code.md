---
title: This One Tweak Improved my Angular Code
tags:
  - angular
  - typescript
url: 4438.html
id: 4438
categories:
  - Angular
date: 2017-09-12 06:30:10
---

I made a tweak to my Angular code process over the last month or so that has resulted in greater productivity in my development environment and fewer bugs. 

Now, I didn't make this change because I thought it would improve my productivity.  At least that wasn't the primary reason.  I made the change because I thought it would reduce the chance of introducing bugs into my code.  And while it does reduce the number of bugs in my code, the result has been generally improved productivity. 

What is this great secret? <figure>![](/uploads/2017/09/2017-09-12.jpg "This One Tweak Improved my Angular Code")<figcaption>Photo credit: [docoverachiever](//www.flickr.com/photos/sheila_sund/36859429262/) via [Visual Hunt](//visualhunt.com/re/d75fb5) / [ CC BY](//creativecommons.org/licenses/by/2.0/)</figcaption></figure>

<!-- more --> 

History
-------

Before I tell you the secret, I want to tell you how I got here.  Yesterday, I was listening to a podcast that was reviewing another framework with the creators of that framework.  All during the podcast they were comparing their framework to Angular and React.  Fine, compare and contrast is good.  Except, they were comparing and contrasting something they knew really well, their framework, to something they had a lesser understanding of.  So, in the process, one of their complaints about Angular was that Angular doesn't take full advantage of TypeScript.  Or, it doesn't take as full advantage of TypeScript as their framework does. 

As I listened to this I realized that most Angular developers probably aren't taking as full advantage of TypeScript as they could.  I think this is by design.  But, it is too our hurt.  Fortunately, we can turn things on that Angular leaves turned off.

Type Safety
-----------

Now, if you are like most developers I know, you just want to get your code working.  And so, you don't pay attention to things like Linters.  And you have learned enough TypeScript to get your code working.  This is especially true of those who are coming from "old school" JavaScript. 

Now, I'm going to challenge you to do two things.  First, use TSLint.  Next, make the rules as rigid as possible.

### TSLint

If you are using the Angular CLI, TSLint is built in.  To run TSLint, all you need to do is to run 

``` shell
npm run lint
```

On the command line. 

This will verify that you haven't made any really stupid syntax mistakes by default. 

Next, the editor you are using should have a way of hooking TSLint up to the editor so that you can tell you have a problem in your code as you write your code.  If your editor doesn't have a way of doing this, find a new editor.  The two editors I recommend are VS Code and WebStorm.  Each have their own strengths and weaknesses.

### Rules

The rules that come with the Angular CLI are, in my opinion, too relaxed.  They are strong enough to not annoy JavaScript programmers.  They protect you from making really stupid TypeScript programming errors.  But, they don't help you write code that you have a high assurance will work correctly the first time without running it. 

We are going to fix this. 

The main thing we want to do is that we want to tighten up type checking.  So, instead of using the defaults that either let variables be anything or default them to the type they are assigned to when they are declared if that can be determined, we are going to force variables to be declared.

Why This Helps
--------------

When I first made this kind of change, I was doing it to [improve my NgRX code](/typescript-functional-programming/).  Once I did that, I immediately saw that by enforcing type safety, I could easily tell if the functions I was using in my Observable chains was going to end up returning what I thought it was going to.  This means, I can write my code and if I don't get any tslint warnings in my editor, I can have a high degree of certainty that the code will run correctly once I compile it and run it. 

Maybe you think type safety is for sissies.  OK then, you can continue to run your code multiple times to find the same error that I can find before I ever run the code.  I think we all know who will be more productive. 

Once I saw this productivity gain, I determined to add type safety to my entire project.  This took much less time than you would expect and I was able to apply the rules incrementally so that I was not disrupting the progress of the project I was working on.

Implementation
--------------

The implementation is actually pretty simple.  The first thing you are going to want to do is that you'll want to change the npm lint script from: 

`"lint": "ng lint"` 

to 

`"lint": "ng lint --type-check --fix"` 

This does two things.  First it allows us to change to tslint rules to use rules that require type checking.  And second it will cause the tslint to fix any problems it finds that it can fix automatically. 

Next, you'll want to install tslint-immutable, which will allow us to add in rules for immutability that I mentioned in my previous post that I linked to above.  As of this writing, I'm using version 4.1.0 

`npm install --save-dev tslint-immutable` 

Next, open up tslint.json which should be at the root of your application. 

Inside this file, right before the rulesDirectory section, add this code: 

`"extends": ["tslint-immutable"],` 

This will allow us to access the immutable rules we just added with `npm install`.

### typedef

The first thing we want to do is that we want to force everything to have a type definition. 

``` javascript
"typedef": [ true,
  "call-signature",
  "arrow-call-signature",
  "parameter",
  "arrow-parameter",
  "property-declaration",
  "variable-declaration",
  "member-variable-declaration",
  "object-destructuring",
  "array-destructuring"
],
```

The one place where you might have a problem is that by default, the no-inferable-types rule is turned on, which is what we want.  Currently, tslint is not smart enough to know that no-inferable-types should take precedence over typedef.  So, you'll need to occasionally exclude the rule using the [tslint comment exclusion](//palantir.github.io/tslint/usage/rule-flags/).  I exclude the typedef when I run into this problem. 

If you are interested, you can google each of the typedef declarations (rather than linking to them and have the links go stale).

### no-any

Now that we are forcing everything to have a type definition, the next thing you want to do is that you'll want to disallow using "any" as a type definition.  Otherwise, why force the type definition at all?  I'd love to be able to disallow "object" as well, but there is currently no definition for that so until someone creates that rule, you'll just have to use discipline.

`"no-any": true,`

You may complain that sometimes you want to have the variable really be anything.  Anything?  Really?  I bet most of the time what you really want to be able to do is that you'll want to define the variable as one of two, maybe three types.  You can achieve this definition as of TypeScript 2.4, by using the pipe operator.

`var stringOrNumber: string | number;`

### no-unused-variable

To keep your code clean, the next rule you want to enable is to disallow creating variables that you aren't using.  This will have the side effect of cleaning up your import statements as well. 

`"no-unused-variable": true,` 

Those are all the rules I use that impact type safety.  Here are a few more you might want to consider adding.

### Cyclomatic Complexity

Cyclomatic Complexity is a measure of how complicated your functions and methods are.  While the default implementation for this is 20, I find that if I've written a function that is that complex, even I have trouble understanding it any more.  While keeping my complexity lower than 10 contributes to extremely readable code.  Yes, there are times when there really isn't any good way of breaking the code into smaller chunks.  But that is rare and code comment exclusions will let you handle those exceptions. 

`"cyclomatic-complexity": [true, 10],`

Optional Rules
--------------

Some other rules you might want to consider adding in that could improve your code include:

### array-type generic

Coming from a C# background, I prefer to define my arrays as Array<T> instead of T\[\].  By default nothing is defined.  If it is just you working on the code, it probably doesn't matter than much.  But on a team, I find consistency useful. 

`"array-type": [true, "generic"],`

### readonly-array

For my NgRX code, I like to have readonly-array turned on to ensure immutability.  I've found that I rarely need to turn this off in the rest of my code since NgRX is where I would be mutating anything that isn't local to a function.  So, I just turn this rule on globally. 

`"readonly-array": [true, "ignore-local"],`

Final Notes
-----------

I mentioned above that if you are using TypeScript 2.4.n, you can use the pipe operator to combine types.  You also get stronger type checking if you use that version.  However, if you are using codelyzer, as of this writing, you'll have trouble if you use TypeScript 2.4.n with a version of Codelyzer greater than 3.0.1.  I keep trying the newer versions to see if this issue has been corrected.  Anyhow, your mileage may vary moving your code to TypeScript 2.4.n.

My TSLint file
--------------

For those who are curious, my full tslint.json file is below. 

``` javascript
{
    "extends": ["tslint-immutable"],
    "rulesDirectory": [
        "node_modules/codelyzer"
    ],
    "rules": {
        "typedef": [true, 
            "call-signature",
            "arrow-call-signature",
            "parameter",
            "arrow-parameter",
            "property-declaration",
            "variable-declaration",
            "member-variable-declaration",
            "object-destructuring",
            "array-destructuring"
        ],
        "array-type": [true, "generic"],
        "readonly-keyword": false,
        "readonly-array": [true, "ignore-local"],
        "no-let": false,
        "no-any": true,
        "cyclomatic-complexity": [true, 10],
        "no-unused-variable": true,
        "arrow-return-shorthand": true,
        "callable-types": true,
        "class-name": true,
        "comment-format": [
            true,
            "check-space"
        ],
        "curly": true,
        "eofline": true,
        "forin": true,
        "import-blacklist": [true, "rxjs"],
        "import-spacing": true,
        "indent": [
            true,
            "spaces",
            4
        ],
        "interface-over-type-literal": true,
        "label-position": true,
        "max-line-length": [
            true,
            140
        ],
        "member-access": false,
        "member-ordering": [
            true,
            "static-before-instance",
            "variables-before-functions"
        ],
        "no-arg": true,
        "no-bitwise": false,
        "no-console": [
            true,
            "debug",
            "info",
            "time",
            "timeEnd",
            "trace"
        ],
        "no-construct": true,
        "no-debugger": true,
        "no-duplicate-super": true,
        "no-duplicate-variable": true,
        "no-empty": false,
        "no-empty-interface": true,
        "no-eval": true,
        "no-inferrable-types": true,
        "no-misused-new": true,
        "no-non-null-assertion": true,
        "no-shadowed-variable": true,
        "no-string-literal": false,
        "no-string-throw": true,
        "no-switch-case-fall-through": true,
        "no-unnecessary-initializer": true,
        "no-trailing-whitespace": true,
        "no-unused-expression": true,
        "no-use-before-declare": true,
        "no-var-keyword": true,
        "object-literal-sort-keys": false,
        "one-line": [
            true,
            "check-open-brace",
            "check-catch",
            "check-else",
            "check-whitespace"
        ],
        "prefer-const": true,
        "quotemark": [
            true,
            "single"
        ],
        "radix": true,
        "semicolon": [
            "always"
        ],
        "triple-equals": [
            true,
            "allow-null-check"
        ],
        "typedef-whitespace": [
            true,
            {
                "call-signature": "nospace",
                "index-signature": "nospace",
                "parameter": "nospace",
                "property-declaration": "nospace",
                "variable-declaration": "nospace"
            }
        ],
        "typeof-compare": true,
        "unified-signatures": true,
        "variable-name": false,
        "whitespace": [
            true,
            "check-decl",
            "check-operator",
            "check-separator",
            "check-type"
        ],
        "directive-selector": [true, "attribute", "app", "camelCase"],
        "component-selector": [true, "element", "app", "kebab-case"],
        "use-input-property-decorator": true,
        "use-output-property-decorator": true,
        "use-host-property-decorator": true,
        "no-input-rename": true,
        "no-output-rename": true,
        "use-life-cycle-interface": true,
        "use-pipe-transform-interface": true,
        "component-class-suffix": true,
        "directive-class-suffix": true,
        "no-access-missing-member": true,
        "templates-use-public": true,
        "invoke-injectable": true
    }
}
```

---
layout: posts
title: Automating JavaScript testing in a Microsoft environment
categories: [javascript, testing]
---

Writing software with JavaScript can be a lot like writing software in the dark. There is no static typing and no compiler, most errors are not found untill the code is executed. Hopefully that is in a test environment, and not when a user is working with your software. I think that a lot of emphasis should be on automated testing and analysing the code. At my current project, we have a rather nice setup in place to do this and guard our quality.

The thing that caused a bit more work is that we are working in a Microsoft environment. So we have Visual Studio (2013) and TFS (2013). The rest of our project consists of C# and we like to work in Visual Studio. Any tooling that we add is preferred to integrate with our current tools. We already have gated checkins and continous integration builds for the .NET code. The CI build runs everything that is in the gated checkin build plus more (and slower) tests.

### Code analysis

We started without any code analysis on our JavaScript. We had to do lots of testing to find bugs before checking in/ releasing/ deploying. And we started to get more and more different coding patterns and styles. We wanted to get rid of this time, therefore we integrated [ESLint][1] into our development process. There are more linters for JavaScript available. ESLint however was started with the promise to enable people to write their own rules, specific for their architecture. [Nicholas Zakas][2] being one of our inspirations while evolving our architecture, the decision for ESLint was made rather quick.

Running ESLint on your code is one thing, integrating and forcing it to pass on every checkin is another. What we did is building a C# Unit test that executes ESLint on our files. It fails when there is an error or warning, and passes when everything is fine. This test is run during the gated checkin, so we know that the code in our source control system is following our rules.

We started with some debt, as we needed to cleanup. What we did was enabling all rules that passed, and worked from there to meet our goals (a set of rules and limits that we decided on). This allowed us to move gradually to a more stricter rule set without slipping and adding more stuff that we needed to cleanup one day.

Rules that really help us to keep a clean code base are things like unused variables, naming conventions, scope issues, using _strict_ mode and lots more. Things that we find less important are [order][11] (using vars that are assigned further on the file) and [defining all vars on top of the function][12]. But it sure helps us to keep things nice and clean.

### Unit tests

<p style="float:right; width: 400px; margin-left: 20px;">
  <a href="http://www.leonardscomic.com/comic/68/unit-testing/">
    <img src="/images/unit_testing.png" alt="Unit testing" title="From Leonard's Comic - Unit Testing" style="width:400px;">
  </a>
  <span style="font-style: italic;">From Leonard's Comic - Unit Testing</span>
</p>

A test suite was also something on our todo list. We started out with [QUnit][3] and a static html file to run the tests. QUnit is a simple test framework, with few concepts. That's why we like it so much, it is dead simple. There are many more frameworks, so go find the one that suits you.

The first approach, using one static html file for all QUnit tests, became soon unmanageable. And we wanted to execute those tests in our build. So we looked at some test runners for Visual Studio and TFS. We already use Resharper for Visual Studio and they have some support for running JavaScript tests. But is far from complete, for instance it misses basic support for [RequireJS][5]. We also found [Chutzpah][4], an open source project with much more features and support. With Chutzpah, we could get rid of our html files and execute the tests from the build. Chutzpah can run the tests in [PhantomJS][5] or in the browser. Running them in the browser gives us much better support for debugging and solving problems.

In previous versions, Chutzpah also didn't really have support for RequireJS, the module loader that we use. We implemented a mock of RequireJS instead, that enabled us to run our tests and make sure that we didn't need to modify our code. It was far from beautiful. We had a double administration of dependencies (both in RequireJS and in QUnit tests). But hurray, the latest version has [support for RequireJS][6]. We deleted our mock and now we can really run our tests the way we want. 

There is one thing that made our setup a little more difficult. We like to make separate Visual Studio Projects for code and test-code. With C# tests, this is no problem. For JavaScript, it is somewhat harder. We have our code in code-library projects, no web projects. We release and deploy through nuget packages, that's way we do not need web projects. Our QUnit tests are in a separate code-library project under the folder _tests_ (our code is under _src_). To make sure that RequireJS can find the files, we have a special RequireJS config for the QUnit tests, in which we use the [path][7] config option to point to the right source folders.

### Integration tests

Another thing that we wanted to test was the JavaScript code that we generate. Our current architecture consists of generated code and framework code combined. But we wanted to make sure that certain generated code patterns were tested. Therefore we added C# tests that calls our generator and executes JavaScript tests on that code, all from PhantomJS. A hacky way of testing, but it works rather well in our case. The generated code is linted with ESLint (using a different set of rules) and then loaded in PhantomJS. We insert the test code and execute them. The result is a passing or failing test.

The upside of this is that we automatically check a number of code generation patterns and know for sure that they work. The downside is that those tests are hard to debug. When we need to attach a debugger, we start PhantomJS with the debug option and connect to the right port. It works, but is far from beautifull. Lucky for us that we do not need it that much (most failing tests are solvable without attaching a debugger).

### Code coverage

The last step that we took was enabling code coverage. The latest and greatest version of Chutzpah also has support for code coverage, through [Blanket.js][8]. It instruments every code file and returns a report of lines that were executed.

Now of course we know the downside of code coverage and that it can be misused. But the one thing that is useful, is to show you that certain code is never executed under tests. It gives some direction and level of confidence in our code. We see that it tells us something about our test level, not using code coverage would mean that we know nothing about this level except some gut feeling.

First we enabled it for our QUnit tests. That was easy. _Although you need to know that the Chutzpah config file is being cached, so changing it will not always directly work. See this [bug][9]._ But we had some tests failing because of timeouts. That was odd. Then we saw that it were tests that not exactly tested a unit, they loaded most of the framework. And with the framework, most of the third party libraries (with both JQuery and Handlebars). Blanket.js does not really handle those files well. So we needed to exclude those files, not that we wanted coverage on them anyway. But we hit a minor missing feature in Chutzpah, excluding files for coverage does not play well with RequireJS. We made a local fix for our situation, but I am working on a pull request to get this fixed (and filled an [issue][10]).

After that we enabled coverage in our integration tests. We altered the C# code to insert Blanket.js in the generated code, and wrote some extra code to export the coverage results to disk. By then we had tens of coverage files, all telling us something about the code coverage from their point of view. A little C# console app fixed the last hurdle: combining all that information into a single coverage report, telling us how much code we covered.

### Conclusion

And now we have a nightly build. It runs all of our tests with code coverage enabled. It gives us two reports, a coverage file for the C# code, and html file for the JavaScript code. And all was well :-)

_One thing is missing: the html file is in the drop folder. But we want it to be attached to the build report like the coverage file. If someone knows how to do this, I would love to hear!_

_Any suggestions? I love to hear them!_


[1]: http://eslint.org
[2]: http://www.nczonline.net/
[3]: https://qunitjs.com
[4]: http://chutzpah.codeplex.com
[5]: http://RequireJS.org
[6]: http://matthewmanela.com/blog/chutzpah-3-0-mocha-requirejs-and-more/
[7]: http://requirejs.org/docs/api.html#config
[8]: http://Blanketjs.org
[9]: http://chutzpah.codeplex.com/workitem/193
[10]: http://chutzpah.codeplex.com/workitem/201

[11]: http://eslint.org/docs/rules/no-use-before-define.html
[12]: http://eslint.org/docs/rules/one-var.html

[13]: http://www.leonardscomic.com/comic/68/unit-testing/
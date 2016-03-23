# C# Coding Standards

We have two simple principles we apply when writing software:

* **Testability**. 
We build software for testability (unit test, integration test and functional tests).

* **Readability**.
It is very likely that you will not be the only one to work on this code.

The statements above are general knowledge. But they can be interpreted in different ways. In order to make them less subjective for our code bases we have defined rules and examples why we write our code this way. In pull request feedback we refer to these rules. 

## Design
| # | |
|:--|:--| 
| D01 | <a name="D01"></a>**Use the Single Responsibility Principle (SRP) of SOLID.** This would normally mean that an interface or class has a limited number of methods and properties. Repositories for example will only deal with one type of entity. Classes dealing with reading files will separate the reading of the file from the actual interpretation of the contents. |
| D02 | <a name="D02"></a>**An interface must have 6 or less methods and properties.** If your interface has more than 6 methods and properties (in total) then it will, in most scenarios, be violating SRP. Refactor the interface into multiple interfaces which you will be injecting the implementation (class), based on the new interface into the interface that was previously violating the SRP. |
| D03 | <a name="D03"></a>**Don't use a constructor with more than 4 parameters.** Similar to the reasoning for the Interface, this is an indication that you're violating the SRP. Please reconsider your design and reduce the coupling. |
| D04 | <a name="D04"></a>**Use the Liskov substitution principle of SOLID.**  In our case this means that all classes implementing an interface must behave the same for all implementations. The caller of the interface should therefore not have to act differently if class A or class B implements the interface. |
| D05 | <a name="D05"></a>**Use the Interface segregation principle of SOLID.** We must always be talking to interfaces which have a specific, small contextual meaning for the consuming party. |
| D06 | <a name="D06"></a>**Use the Dependency inversion principle of SOLID.** Follow this principle to facilitate loose coupling: High-level modules should not depend on low-level modules - both should depend on abstractions (interfaces). Abstractions should not depend on details - details should depend on abstractions (interfaces). |
| D07 | <a name="D07"></a>**Use composition over inheritance.** Use interfaces injected into classes to achieve an object oriented pattern, including polymorphism. Don't use (abstract) base classes to separate definition between implementation. |
| D08 | <a name="D08"></a>**Use the Open/Closed Principle of SOLID.** We use composition over inheritance so the 'normal' (abstract) base class open/closed principle implementations are not possible. We can however achieve the same result by using an interface and a concrete implementation of this interface in a class. Where we only have to change the behaviour of the class and not the interface. Closely related to the Liskov substitution and Interface segregation principles of SOLID. |
| D09 | <a name="D09"></a>**Abide by the Law of Demeter.** In order to achieve loose coupling one should only be calling methods and properties internal to its class, internally created objects, parameters for the method or injected (through the constructor) dependencies to the class. The level of calling should not go deeper than that. Constructions like `myClass._injectedInterface.PublicMethod()` is fine. If `_injectedInterface` however exposes a nested class (which also has its own public methods) then you should change your design and inject the nested class as an additional dependency (using its interface). |
| D11 | <a name="D11"></a>**The Dependency Injection container should never be made publicly available or be passed along to classes instantiated in the container.** The implementations should not even be aware of the DI container; please inject the required elements for that class in the constructor. The DI container should only be used in 'bootstrapper' code. |
| D10 | <a name="D10"></a>**Use the Dependency Injection container to instantiate all classes except POCOs and DTOs.** Do not store any entities/DTOs/POCOs/etc. in the DI container. |
| D12 | <a name="D12"></a>**Use Microsoft Unity as a Dependency Injection container.** |
| D13 | <a name="D13"></a>**Use Constructor Injection.** |

## Coding
| # | |
|:--|:--| 
| C01 | <a name="C01"></a>**Don't use continue or goto statements.** |
| C02 | <a name="C02"></a>**Only use the break keyword in a switch statement.** |
| C03 | <a name="C03"></a>**Don't use multiple return statements.** |
| C04 | <a name="C04"></a>**Don't repeat code.** Refactor if you do. |
| C05 | <a name="C05"></a>**Maximum method length is 30 lines.** A long method has the tendency to impair the readability and maintainability of a method. Keep your methods concise and clear. |
| C06 | <a name="C06"></a>**Only use recursion if you really need to.** Which is not very often. |
| C07 | <a name="C07"></a>**Don't use the static keyword.** Not for classes, methods or 'variables'. Yes, you will have a small performance increase if you do, but you will otherwise open up your class to usage by everybody. While we want to ensure that we keep dependencies to other functionality low; unit testing is also harder to do because you cannot easily replace a static implementation with other (possibly mocked) functionality. There is one exception to this rule: the Main method in console applications. That obviously needs to be static.|
| C08 | <a name="C08"></a>**Don't create extension methods.** They are static by nature. |
| C09 | <a name="C09"></a>**Use the var keyword only when the type can easily be inferred.** Normally, if you're calling a method, the return type is not defined, so declare the expected return type, e.g.: `List<int> valueList = _integerRepo.GetValues()`. If you're initialising a variable, you should use the expected type as the initial value, e.g.: `var valueList = new List<int>();` |
| C10 | <a name="C10"></a>**Use Path.Combine when dealing with concatenation of paths and filenames.** It ensures that the slashes go in the right direction and saves you from manually escaping strings. |
| C11 | <a name="C11"></a>**Make all strings, except logging strings, constants.** If they are internal to the implementation, make them private constants at the top of the class. If they are used publicly, put them in a separate file. For example a `Constants.cs`. (*This is sometimes known as the "magic strings" rule.*) |
| ~~C12~~ | <a name="C12"></a>~~**Put all strings shown to an end user in a Resource Dictionary.** This also allows your application to be internationalised easily.~~ |
| C13 | <a name="C13"></a>**Make all numbers constants.** Similar to C11, make all numeric values constants in the same way. (*This is sometimes known as the "magic number" rule.*) **Don't:** `GetXForSubsidiary(1)` **Do:** `GetXForSubsidiary(SubsidiaryConstants.SUBSIDIARY_NETHERLANDS)` |
| C14 | <a name="C14"></a>**Don't (re)throw exceptions.** Handle them as close to the source as possible and remember; exceptions should be exceptional. |
| C15 | <a name="C15"></a>**Don't use Tuples.** Make a class instead. If you really want to use a Tuple, use a KeyValuePair instead. |
| C16 | <a name="C16"></a>**Use an object initializer when creating new objects directly.** See [MSDN](https://msdn.microsoft.com/en-us/library/bb384062.aspx) for more information. **Do:** `var pepijn = new Person { Name = "Pepijn" };` **Don't:** `var pepijn = new Person(); pepijn.Name = "Pepijn";`  |
| C17 | <a name="C17"></a>**Use LINQ to iterate over collections.** |
| C18 | <a name="C18"></a>**Use LINQ "method syntax" instead of "query syntax"** See [MSDN](https://msdn.microsoft.com/en-us/library/bb397947.aspx) for more information. **Do:** `var reviewers = developers.Where(d => ...` **Don't:** `var reviewers = from d in developers where ...` |
| C19 | <a name="C19"></a>**Don't initialize strings to null.** This means you can use `String.IsEmpty()` and never have to use `String.IsNullOrEmpty()`. |
| C20 | <a name="C20"></a>**Use String.Empty instead of "" to initialize it.** Never use null. |
| C21 | <a name="C21"></a>**Use String.Format, String.Concat or String Interpolation for concatenating strings.** Do not: string s = "string1" + "string2"; |
| C22 | <a name="C22"></a>**Try to avoid the nullable (`Nullable<int>` or `int?`) values.** It introduces another state which you need to check everywhere. On boolean type it would for example be strange to also make it nullable. Something is either false or true. If you add nullable and give it another meaning, for example an error scenario, you should use a different type (probably a boolean) for making the distinction between success or failure. |
| C23 | <a name="C23"></a>**Don't check for null on your parameters as a guard-clause within your method.** Simply don't call those methods when there is nothing valid to be processed in the method. The calling method should know why some variables are null and should act accordingly. |
| C24 | <a name="C24"></a>**Use the lower case 'version' of types when declaring them.** For example; use `string` instead of `String` as a variable |
| C25 | <a name="C25"></a>**Use the upper case 'version' of types when using their static methods** For example; use `String.IsEmpty()` instead of `string.IsEmpty()` |
| C26 | <a name="C26"></a>**Use the highest possible abstraction for an interface when working with Lists and Collections.** This is normally `IEnumerable<T>`. |
| C27 | <a name="C27"></a>**Always initialise Lists and Collections.** And return empty collections if processing of a certain piece of logic failed. |
| [C28](SupportingDocs/C28.md) | <a name="C28"></a>**Don't use the `dynamic` keyword.** It’s used for things that we shouldn't do. And there are always other coding solutions which don’t involve using the `dynamic` keyword.|
| C29 | <a name="C29"></a>**Use the functionality in the Coolblue utils (Github repo).** `Credential Manager`, `Humanizer`, `ConnectionStringCreator` |
| C30 | <a name="C30"></a>**A class implementing an interface should never call the public methods also defined in the interface.** Basically, don't call yourself, let the caller call you. |
| C31 | <a name="C31"></a>**Make a member variable readonly if it's only manipulated in the constructor.** This can also be done by making the auto-implemented properties setter private. |
| C32 | <a name="C32"></a>**Avoid Reflection.** |
| C33 | <a name="C33"></a>**Use the as keyword when casting.** And check whether the outcome of the case is not null. |
| C34 | <a name="C34"></a>**Use the decimal type when dealing with prices/money.** |
| C35 | <a name="C35"></a>**Implement the IDisposable pattern when dealing with unmanaged resources.** |
| C36 | <a name="C36"></a>**Implement the IDisposable pattern when you want to free up resources before the Garbage Collector comes along.** This is something you might want to do when you are using large amounts of memory or database connections. |
| C37 | <a name="C37"></a>**Don't use partial classes.** They obfuscate the complexity of a class. If you feel the need to write a partial class, just split them up in two separate classes. |
| C38 | <a name="C38"></a>**Avoid the use of Attributes.** Certainly not writing your own attributes. If you really need them, (for example, mapping external DTOs) make sure it is something you want to be doing. If you need to do too many tricks with the attributes, write a separate testable mapper. |
| C39 | <a name="C39"></a>**Don't use pragmas.** Not for debug scenario's or anything else.  |
| C40 | <a name="C40"></a>**Don't use arrays. Use Collections or Lists.** `.First()` instead of indexers. `.Any()` instead of `.Count > 0` |
| C41 | <a name="C41"></a>**No usernames and/or passwords in code.** Anywhere. Ever. |
| C42 | <a name="C42"></a>**Don't use regions.** If you feel the need to organize your code with regions, it is probably violating the SRP. |
| C43 | <a name="C43"></a>**Favor auto properties over hand written ones.** Making the setter private is also preferred. |
| C44 | <a name="C44"></a>**Encapsulate .NET static functionality into an interface, with a class.** This will let you write unit tests. Favor abstraction over the simplest encapsulation/wrapping when writing such a construction. |
| C45 | <a name="C45"></a>**Make all classes public.** |
| C45 | <a name="C45"></a>**Don't use the internal keyword, unless it's in a private NuGet package.** The only valid use case for having `internal` in your code is when, in a private NuGet package, you want to hide classes or methods from the consumer, e.g. a constructor used internally but that is not part of how the package is meant to be used. |
| C46 | <a name="C46"></a>**Code must be written in English.** |
| C47 | <a name="C47"></a>**Don't use Int16, Int32 and Int64 as variable types.** Use short, int, longs. |
| C48 | <a name="C48"></a>**Names should be unique throughout a solution.** Don't reuse names even if they are part of a different namespace. |
| C49 | <a name="C49"></a>**Initialize boolean return variables with false.** |
| C50 | <a name="C50"></a>**Don't allow warnings** Keep the build clean by configuring all projects to [treat all warnings as errors](https://msdn.microsoft.com/en-us/library/406xhdz3.aspx). In Visual Studio, you can do that on the [build page of the project designer](https://msdn.microsoft.com/en-us/library/kb4wyys2.aspx).
| C51 | <a name="C51"></a>**Keep single-implementation interfaces in the same file as the class.** If your interface is only implemented once, keep both in the same file. Once it gets more than one implementation move it into its own file.|
| C52 | <a name="C52"></a>**Don't make methods for internal use in a class** This prevents unit testing and probably means you're doing too much in a single class. Extract it, simplify it, and inject it. Methods that are used as a base for WPF commands are the only exception. |

## Exception Handling
| # | |
|:--|:--| 
| E01 | <a name="E01"></a>**Keep exception scope as small as possible.** Don't bubble up exceptions. |
| E02 | <a name="E02"></a>**Prevent exceptions from happening.** If you can avoid an exception, then do. This way you get a clear separation between external systems errors, configuration errors and programming errors.  |
| E03 | <a name="E03"></a>**Don't let a constructor throw exceptions.** Constructors should only be for assignment, not operations or instantiation. |
| E04 | <a name="E04"></a>**External systems might not be available.** Use exception handling to handle those situations gracefully. |
| E05 | <a name="E05"></a>**Handle exceptions which might be generated from within the .NET framework** Streams, Uri, File handling etc. |

## Testing
| # | |
|:--|:--| 
| T01 | <a name="T01"></a>**Use [Moq](https://www.nuget.org/packages/moq) for mocking your interfaces.** 
| T02 | <a name="T02"></a>**Use [AutoFixture](https://www.nuget.org/packages/AutoFixture ) for setting up test data.** |
| T03 | <a name="T03"></a>**Use [FluentAssertions](https://www.nuget.org/packages/FluentAssertions/) to test the outcome of a unit test.** |
| T04 | <a name="T04"></a>**Use MSTest (Microsoft Test) as the testing framework.** |
| T05 | <a name="T05"></a>**Code coverage should be 80% or above.** |
| T06 | <a name="T06"></a>**Name tests using the convention `UnitOfWorkName_ScenarioUnderTest_ExpectedBehavior`**. e.g. `IsValidFileName_BadExtension_ReturnsFalse()` |
| T07 | <a name="T07"></a>**Test projects should include the name of the project being tested and end in ".Tests".** e.g. `CalculatorService.Tests` for the `CalculatorService` project |
| T08 | <a name="T08"></a>**Only use test objects (e.g. mocks, test data) as class variables if they're used by all or most of the tests.** Objects used in only 1 or 2 tests should be created in the tests themselves. |
| T09 | <a name="T09"></a>**Don't set up all data, mocks, etc. at test initialization.** Stuff that refers to a single test (e.g. set up a mock to return a specific output for a given input) should be included in the test method. |
| T10 | <a name="T10"></a>**Unit test only what you can control.** The connection with external systems (e.g. database, external services, file system) should not be the target of unit tests. Leave these for integration testing. |
| T11 | <a name="T11"></a>**Setup test data and mocks in the correct places.** In a `[ClassInitialize]` method for anything that only needs to be initialized once. In a `[TestInitialize]` method for things that need to be reset on every test. Don't initialize anything in the constructor. |
| T12 | <a name="T12"></a>**Unit tests should be readable and concise.** Keep test methods small and readable, just like production code |
| T13 | <a name="T13"></a>**Unit test scope should be as small as possible.** Only test a single input/use case/assumption per test method |
| T14 | <a name="T14"></a>**Create a separate instance of the tested class for each test.** This makes it possible to control better how the tested object is created (i.e. dependencies) and makes it very easy to understand how the test works |
| T15 | <a name="T15"></a>**Only do one assertion per unit test.** Multiple assertions hide potentially failing test cases. When an assertion fails all subsequent assertions are not executed |
| T16 | <a name="T16"></a>**Suffix your mocked objects with `Mock`** e.g. `var requiredObjectMock = new Mock<Object>();` |
| T17 | <a name="T17"></a>**Your subject under test should be called `sut`** e.g. `var sut = new SubjectUnderTest(requiredObjectMock);` |
| T18 | <a name="T18"></a>**Name your expected result value expectedResult** |

## Comments
| # | |
|:--|:--| 
| M01 | <a name="M01"></a>**Don't use TODO statements.** They will simply become outdated. If you do use them, at least make sure they don't make it into the PR |
| M02 | <a name="M02"></a>**If you need comments, your design is too complex.** The code you write should be clean and clear. Naming & structure will help you here. |
| M03 | <a name="M03"></a>**Don't comment out code, delete it.** It will still be in source control if you need it later. Make additional branches if need be. |
| M04 | <a name="M04"></a>**Don't use XML comments** They are comments and will get outdated. |

## Naming
| # | |
|:--|:--| 
| N01 | <a name="N01"></a>**Interface and Class names should indicate capabilities, not actions.** Writer or Dispatcher instead of Write or Dispatch |
| N02 | <a name="N02"></a>**Use names as close to correct English as possible.** e.g. `ParseConfirmationFile()` instead of `ConfirmationFileParse()` |
| N03 | <a name="N03"></a>**Use meaningful, clear variable names and log messages.** Don't be afraid to have long names and log messages. |
| N04 | <a name="N04"></a>**Method names must be verbs, not nouns.** e.g. `GetData()` instead of `Data()`. |
| N05 | <a name="N05"></a>**Use the `Entity` suffix for objects related to the database, `Dto` for the ones sent to/received from other systems.** If a distinction is needed between entities stored in a local data store (e.g. RavenDB) and in Oracle, the local store ones should get the `Document` suffix.|
| N06 | <a name="N06"></a>**The name of the solution should match the name of the Github repository.** If your repo is `net-generic-application` there should be a `net-generic-application.sln` available in the top level directory. |
| N07 | <a name="N07"></a>**Namespaces should always match the file structure.** Resharper will warn you when this is not the case. |
| N08 | <a name="N08"></a>**Project names (and namespaces) should be written in `PascalCase`.** If your repo is `net-generic-application`, the projects should all start with `GenericApplication` (e.g. `GenericApplication`, `GenericApplication.Tests`, etc.)|

## GitHub & Pull Requests
| # | |
|:--|:--| 
| G01 | <a name="G01"></a>**Only small pull requests.** Keep those babies small. Small pull requests are more likely to be accepted. A single day of work is the recommended amount. |
| G02 | <a name="G02"></a>**When creating a new solution - make an empty initial commit (and matching PR) before starting.** We expect an empty solution and project, without any added functionality. |
| G03 | <a name="G03"></a>**Include a proper `README` that describes the reason, the design and the expectations.** Look at a few of the latest repositories for a sample |
| G04 | <a name="G04"></a>**Use the correct `.gitignore` file (Visual Studio).** See the [Document Service .gitignore](https://github.com/coolblue-development/net-document-webservice/blob/master/.gitignore) for an example. |
| G05 | <a name="G05"></a>**Add folders/files to the .gitignore that contain any binary files.** We don't want them in source control. The packages folder that contains the downloaded NuGet packages is excluded [(P07)](#P07). Assets, e.g. RavenDB files are also excluded from this rule. |
| G06 | <a name="G06"></a>**Run ReSharper's `Code Cleanup` tool with the `Development` profile on the solution.** When starting work on an existing project, immediately create a Pull Request containing just the results of the cleanup (and name it appropriately). After that (or when creating a project from the start) always run the cleanup as a separate commit just before creating the Pull Request. The Build Server will fail the build if it detects too many formatting violations. |
| G07 | <a name="G07"></a>**Implement the feedback you get.** Feedback you get, is feedback that you implement. Save the conversation ​about​ coding standards for another occasion. Your code needs to be consistent, testable and maintainable, and coding standards help you to accomplish that. |
| G08 | <a name="G08"></a>**Write meaningful commit messages which describe why you did your work explaining the value this commit adds.** Don't be lazy. People will have to maintain your work in five years. Write the message as if you are explaining to your fellow colleague you just met. **Don't:** *fixes build* **Do:** *Fixes inconsistency where mstest regards unit tests failing as opposed to Visual Studio's test runner. Fixes it by resolving an inconsistency in the AutoMapper dependency across projects in the solution.* **Don't:** *merge* **Do:** *Merges the coolblue-development/master to <my private fork>.* **Don't:** *various bug fixes* **Do:** In separate commits: *Adds a visual fix where a person's full user name is not properly formatted in case the user has no insertion (tussenvoegsel).* *Fixes the application mistakenly complaining about 'CookieMonsterWebServiceUri' while actually the 'WhoopsyDaisyWebServiceUri' was incorrectly formatted.* **Don't:** *fixes pr comments* **Do:** In separate commits: *Reverts adding custom mechanism for validating settings since it's preferred to use CoolblueUtils.* *Fixes accidental tabs ending up in files by replacing them with spaces.* *Fixes misspelling of Dictoinary to Dictionary.* |

## Logging
| # | |
|:--|:--| 
| L01 | <a name="L01"></a>**Use Log4Net for logging.** |
| L02 | <a name="L02"></a>**In Release mode, use a SmtpAppender to send error e-mails.** The email address to send 'back end' errors to is: `vanessalogs@coolblue.nl` The correct SMTP server is `nlam01-vmmail02.servers.coolblue.nl`. See the [Document Service .Release config](https://github.com/coolblue-development/net-document-webservice/blob/master/net-document-webservice/Web.Release.config#L52) for an example. |
| L03 | <a name="L03"></a>**Error e-mails must result in code change.** Log errors only if someone should take action (e.g. If the program can't connect but will retry later by itself, that's not an error) |
| ~~L04~~ | <a name="L04"></a>~~**In Release mode, use a LogFileAppender to log warning and error messages.** See the [Document Service .Release config](https://github.com/coolblue-development/net-document-webservice/blob/master/net-document-webservice/Web.Release.config#L40) for an example.~~ |
| L05 | <a name="L05"></a>**Don't use Console.Write(Line) for logging.** |
| L06 | <a name="L06"></a>**Don't use Debug.Write(Line) for logging.** |
| L07 | <a name="L07"></a>**Log the full exception, not the Message.** |
| L08 | <a name="L08"></a>**In Release mode, use an EventLog appender to log error messages.** See the [Competitor Pricing .Release config](https://github.com/coolblue-development/net-competitor-pricing-webservice/blob/master/CompetitorPricingWebservice/Web.Release.config#L27) for an example. |
| L09 | <a name="L09"></a>**In Release mode, implement Application Insights to capture metrics and exceptions** See the [Competitor Pricing .Release config](https://github.com/coolblue-development/net-competitor-pricing-webservice/blob/master/CompetitorPricingWebservice/Web.Release.config#L25) for an example. |

## NuGet Packages
| # | |
|:--|:--| 
| P01 | <a name="P01"></a>**Use NuGet packages only from the [preferred list on devWiki](https://sites.google.com/a/coolblue.nl/it-wiki/-net/knowledge/useful-nuget).** If you want to use a different one for the same purpose, check with your team lead whether the one you want to use is acceptable. |
| P02 | <a name="P02"></a>**Don't reference versions for dependencies in the .nuspec file of our private NuGet packages. Remove the version attribute altogether** This makes sure we're always working on the latest versions of these dependencies. The continuous integration environment will update all packages when creating the build. |
| P03 | <a name="P03"></a>**Always use the latest version of a NuGet package.** This also means always updating to the latest version when you change a project. |
| P04 | <a name="P04"></a>**Add OctoPack NuGet package to projects which need to be deployed.** Automated deployment will fail with an unclear message if OctoPack is not included in your project. You will waste lots of time if you skip this step. |
| P05 | <a name="P05"></a>**Add a .nuspec file to the solution** A sample NuSpec file can be found in the [Purchase Order service](https://github.com/coolblue-development/net-purchase-order-webservice/blob/master/PurchaseOrderService.Api/PurchaseOrderService.Api.nuspec) |
| P06 | <a name="P06"></a>**The .nuspec file should have the same name as the project and reside in the same directory.** This is a NuGet convention, and our deployment relies on this. |
| P07 | <a name="P07"></a>**Add the packages folder of your solution to source control.** This makes it easy to checkout a repository and start working. |

## Console/Service Solutions
| # | |
|:--|:--| 
| S01 | <a name="S01"></a>**Make a .debug and .release config.** For non web projects, you can use the [Configuration Transform VS extension](https://visualstudiogallery.msdn.microsoft.com/579d3a78-3bdd-497c-bc21-aa6e6abbc859) |
| S02 | <a name="S02"></a>**Disable vshost process creation in release config (for non-web projects).** Switch to the Release configuration. Go to `Project > Properties > Debug`, untick all the Enabled Debuggers. Save, Clean & Build. |
| S03 | <a name="S03"></a>**Your console app should be small** You should only really need `net-generic-application` and `net-generic-application.Tests`. If you need more than this, discuss it with your reviewer. |

## WPF - Coolblue WPF Toolkit 
| # | |
|:--|:--| 
| WPF.T 01 | <a name="WPFT01"></a>**Add custom controls to the WPF Toolkit.** When developing custom controls which have a probability of being used in a different application, add these to the WPF toolkit. |
| WPF.T 02 | <a name="WPFT02"></a>**Always add an example of a custom control you're developing to the PresentationCoolkit.** When creating a control for the toolkit, add an example to the PresentationCoolkit showcasing the proper usage of the control so that developers can easily see how to properly use it. |
| WPF.T 03 | <a name="WPFT03"></a>**When creating or updating a company-wide style, this should reside in the WPF toolkit.** Colors defined by UX Designers, such as FogGrey or CoolBlue should be the same for every WPF application we develop. |
| WPF.T 04 | <a name="WPFT04"></a>**When creating a pull request, make sure the description is very clear.** When creating a pull request for the WPF toolkit, add an illustration and an example usage for the feature you have developed. The explanation is used to promote communication and understanding since you are creating cross-team functionality for both designers and technical fellows. |
| WPF.T 05 | <a name="WPFT05"></a>**Always put Converters (IValueConverters) in the WPF Toolkit.** This promotes the readability for WPF developers across solutions by means of consistency. |
| WPF.T 06 | <a name="WPFT06"></a>**Always mark Converters as MarkupExtensions.** This makes sure you don't have to declare the Converter as a StaticResource in the consuming XAML. If there are useful 3rd party converters, please wrap them and put to the Coolkit, so that they are easy to find and conforms to our standards. |

## WPF - WPF solution structure
| # | |
|:--|:--| 
| WPF.S01 | <a name="WPFS01"></a>**WPF solutions should have the following projects, adhering to the following constraints:** `Coolblue.ProjectName.Client`: The Client project contains the Bootstrapper and the PRISM infrastructure. `Coolblue.ProjectName.ModuleA`: The PRISM Module containing the ViewModels and Views. Consumes data via the Coolblue.ProjectName.Services project. Binding to an entity returned from a Service directly is not allowed. It is required to map entities to ViewModels when binding the data to a view. `Coolblue.ProjectName.Services`: The Services namespace is allowed to work with entities and its purpose is to combine Repositories (RestClients) to store or retrieve the entities defined in the Coolblue.ProjectName.Entities project. `Coolblue.ProjectName.Entities`: The Entities namespace contains the objects which can be manipulated by the Services. This also includes POCO ViewModels. `Coolblue.ProjectName.CrossCutting`: This contains the CrossCutting concerns of the application. This assembly may only be referenced to and may never reference to another assembly in the same solution. Example: logging functionality. |

## WPF - General
| # | |
|:--|:--| 
| WPF.G 01 | <a name="WPFG01"></a>**Import styles with ResourceDictionaries in the App.xaml file.** |
| ~~WPF.G 02~~ | <a name="WPFG02"></a>~~**For Blendability use DesignModeResourceDictionaries, or mark controls with an x:Name attribute.** Adding the same ResourceDictionary over multiple files can cause unexpected behavior in production scenarios. The DesignModeResourceDictionary detects when you're developing and becomes active only when it's necessary.~~ |
| WPF.G 03 | <a name="WPFG03"></a>**Do not use ConfigureAwait(false) from the UI thread** Use it only in deeper parts of the call chain. Callbacks from the UI thread should always be returned to the UI thread |
| WPF.G 04 | <a name="WPFG04"></a>**Separate data from logic** Data that is received from services should be contained in POCO view models that do not contain any logic. Rule of thumb: if you need a creator for a POCO view model to inject dependencies that's not a POCO. |

## WPF - MVVM
| # | |
|:--|:--| 
| WPF.MVVM 01 | <a name="WPFMVVM01"></a>**When a viewmodel needs to implement INotifyPropertyChanged or derive from BindableBase, instead inherit your ViewModel from ViewModelBase from the Coolkit.** This class derives from PRISM's BindableBase. |
| WPF.MVVM 02 | <a name="WPFMVVM02"></a>**When creating a validatable ViewModel which updates its validation status to the GUI, inherit from WpfValidatableViewModelBase.** Opt-in to validation results for a ViewModel using the SetAndValidateProperty method. |
| WPF.MVVM 03 | <a name="WPFMVVM03"></a>**In the setter of a ViewModel's public property, use SetProperty to notify listeners of the changes.** It is a method from BindableBase which sets the value and calls OnNotifyPropertyChanged(). |
| WPF.MVVM 04 | <a name="WPFMVVM04"></a>**Don't put custom code in the code-behind.** There's no unit testing the code-behind. Therefore there shouldn't be any code.  |
| WPF.MVVM 05 | <a name="WPFMVVM05"></a>**Use PRISM's DelegateCommands with their canExecute method.** Use DelegateCommands instead of events when reacting to user input. When there is a need to disable/enable the command given a certain circumstance, define the constraint in a method supplied to the CanExecute parameter. When the relevant circumstances change, use RaiseCanExecute(command) to propagate these changes. |
| WPF.MVVM 06 | <a name="WPFMVVM06"></a>**Make public properties on a ViewModel self-sufficient.** This means that you should keep the implementation of a public property as close as possible to the definition. **Do:** For a Person with a Firstname and Surname, the FullName would be defined as such: `public string Fullname { get { return String.Format("{0} {1}", Firstname, Surname); } }` `public string Firstname { get { return _firstName }; set {        _firstname = value; RaisePropertyChanged(() => Fullname); } }` In the above case Firstname and Surname would both call OnPropertyChanged(() => Fullname); in their public setters. **Don't:** `public string Firstname { get { return _firstName }; set { _firstname = value; _fullName = string.Format("{0} {1}", value, _lastName); } }` **Don't:** `public string Firstname { get { return _firstName }; set { _firstname = value; UpdateFullname(); } } private void UpdateFullname() { Fullname = String.Format("{0} {1}", FirstName, LastName); }`  |
| WPF.MVVM 07 | <a name="WPFMVVM07"></a>**Don't use event triggers, they bypass the MVVM pattern.** Instead, bind to the properties of the control instead, and have the view model handle the logic in the getter/setters.  |
| WPF.MVVM 08 | <a name="WPFMVVM08"></a>**Do not enrich POCO ViewModels in other ViewModels** If you need to call a number of services to enrich one POCO it's better to just create an aggregate service for that.  |


## Web API Solution
| # | |
|:--|:--| 
| W01 | <a name="W01"></a>**Web API solutions should have 5 projects.** In a sample solution for `net-generic-webservice`, you should have the following: Webservice Client: `GenericWebservice.Client`, DTOs: `GenericWebservice.Dto`, Unit tests: `GenericWebservice.Tests`, Client unit tests: `GenericWebservice.Client.Tests` and the API itself: `GenericWebservice`. |
| W02 | <a name="W02"></a>**Include Octopack and a .nuspec file in the Api and REST client projects.** They will both be deployed separately, so they need to have the proper instructions. See the NuGet section for more information about Octopack and .nuspec |
| W03 | <a name="W03"></a>**Use the Repository pattern to access data stores.** One repository class per entity, handling all the CRUD operations for the corresponding entity |
| W04 | <a name="W04"></a>**Use the Unity.WebApi NuGet package.** This will give you a `UnityConfig` class in your `App_Start` folder and register Unity as the Web Api container. You can use this class to register all your implementations. |
| ~~W05~~ | <a name="W05"></a>~~**Provide XML comments with expected responses on each Controller action.** This will help to ensure that consumers of the Web API can anticipate each of the responses. See the [Document Service controller](https://github.com/coolblue-development/net-document-webservice/blob/master/net-document-webservice/Controllers/DocumentController.cs#L30) as an example.~~ |
| W06 | <a name="W06"></a>**Ensure there is a .debug and .release config.** Also ensure that the values are set correctly (see the Logging section for more information) |
| W07 | <a name="W07"></a>**Use attributes to specify HTTP verbs for controller methods** Don't rely on having methods named Get(...), Put(...), etc. Give them a better name and add the `[HttpGet]`, `[HttpPut]`, etc. attributes to them. |
| W08 | <a name="W08"></a>**Expose only async API in web service clients** Do not make sync wrappers for async API. Let the consuming code decide how to execute you async API |
| W09 | <a name="W09"></a>**Always return understandable HTTP Responses** To make interacting with services simpler, controllers should only return responses with simple, well understood HTTP status codes. The list of acceptable responses that a controller should be able to return are: `200 OK`, `201 Created`, `400 Bad Request`, `401 Unauthorised`, `404 Not Found`, `500 Internal Server Error` |

## Using Async 
| # | |
|:--|:--| 
| A01 | <a name="A01"></a>**All awaitable method names must end with Async suffix** All methods that have async keyword in the method signature and/or return ``Task`` or ``Task<T>`` are awaitable. Such naming makes it easier to spot misusages of async methods |
| A02 | <a name="A02"></a>**Do not create async void methods** Async methods must return ``Task`` or ``Task<T>``. Exceptions in async void methods crash the execution and you will never know that it failed. **The only exception** is WPF event handling. Since WPF still does not handle async code we do not really have a choice here. But such methods should be just wrappers for async ``Task`` or async ``Task<T>`` methods. Name them accordingly |
| A03 | <a name="A03"></a>**Do not use Task.Wait(), Task.Run() or Task.Result** Using any of these means you are misusing the async code and it can break your application. For example, calling Task.Result in the UI thread will create a deadlock. There is one exception to this rule. When consuming async code in a console application, you will have to wait for the asynchronous code to complete.  |
| A04 | <a name="A04"></a>**Do not make your method async, if it's not needed** If a method only has one `await` expression, and it doesn't contain any statements after that, it is not needed. Return the `Task`, and remove the `async` modifier from the method signature. |


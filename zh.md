# 單元測試的藝術 ch7
```text
7.1 執行自動化測試的自動化建置
     7.1.1 建置腳本結構
     7.1.2 觸發建置和整合
 
7.2 依據速度和種類對應的測試分類
     7.2.1 分開整合測試和單元測試的人為因素
     7.2.2 綠色安全區域
 
7.3 確保測試程式是版本庫管理的一部分

7.4 將測試類別的位置與被測試程式相對應
     7.4.1 將測試對應到專案
     7.4.2 把測試對應到類別
     7.4.3 將測試對應到明確的工作單元入口
 
7.5 注入橫切面關注點

7.6 為應用程式建立測試API
     7.6.1 使用繼承類別繼承模式
     7.6.2 建立測試輔助類別和方法
     7.6.3 把你的API 介紹給開發人員
 
7.7 小結
```

* 無論您執行什麼測試（無論您如何執行），都將其自動化，使用自動化構建過程在白天或晚上盡可能多地運行它，並***儘可能連續地交付產品***。
    > 那什麼是沒有自動化的單元測試呢？

* 將整合測試與單元測試（慢速測試與快速測試）***分開***，以便您的團隊可以擁有一個安全的綠色區域，***所有測試都必須通過***。
    > 維護綠色安全是最基本的要求。

* 按項目和類型劃分測試（單元測試與集成測試，慢速測試與快速測試），並將它們分為不同的專案，文件夾或名稱空間（或全部）。我通常使用所有三種類型的分離。
    >  透過明顯的物理區分，讓測試目標可以更明確的執行，而且耗費的心力相對少。

* 使用測試類層次結構將相同的測試集應用於層次結構中被測試的多個相關類型或共享公共接口或基類的類型。
> o

* 如果測試類的繼承使測試的可讀性降低，那就使用輔助類別(helper classes)和工具類別(utiity classes)而不是繼承結構，尤其是在基類中使用共享的setup的情況下。不同的人對何時使用哪種有不同的看法，但是可讀性通常是不使用繼承結構的主要原因。
  * Use helper classes and utility classes instead of hierarchies if the test class hierarchy makes tests less readable, especially if there’s a shared setup method in the base class. Different people have different opinions on when to use which, but readability is usually the key reason for not using hierarchies.
  > reuse 程式碼的方式有很多種，而繼承偏偏是最難回去看懂，但是很簡單就可以實作的。
    
* 讓您的團隊知道您的API。如果您不這樣做，您將浪費時間和金錢，因為團隊成員在不知不覺中一遍又一遍地重新發明了API。
    > 不只是其他團隊成員，其他專案也會有這個問題。

---


## 7.1 執行自動化測試的自動化建置 p.154
bulid process 有 
1. build scripts, 
2. bulid integaration servers, 
3. build triggers, and 
4. a share teamunderstanding and acceptance of how code is deployed and integrated.

  >   7.1.1 建置腳本結構
  > 使用多個腳本, 一個腳本完成一個功能 p.155 
* A continuous integration (CI) build script
* A nightly build script 任何不再 CI bs 裡面的任務（無關、不夠重要）
* A deployment build script

	> because I want my build script actions to be version aware (or version controlled), so I can always get back to any version of the source and my build actions will be relevant to that version.

  >   7.1.2 觸發建置和整合
CI server 主要功能包含：
* Trigger a build script based on specific events
* Provide build script context and data such as version, source code, and artifacts from other builds, build script parameters, and so on
* Provide an overview of build history and metrics
* Provide the current status of all the active and inactive builds

Bulid Configuration 有 command
Bulid Configuration 有 context 如 commit tree、Set Unix ENV、bash direct parameters、上次可重用部分

 
7.2 依據速度和種類對應的測試分類
     7.2.1 分開整合測試和單元測試的人為因素
     7.2.2 綠色安全區域
 
7.3 確保測試程式是版本庫管理的一部分
Tests must be part of source control. 
you should treat your test code as thoughtfully as you treat your production code. 
Because unit tests are so connected to the code and API, they should always stay attached to the version of the code they’re testing. Obtaining version 1.0.1 of your product means also getting version 1.0.1 of the tests for your product; version 1.0.2 of your product and its tests will be different.

7.4 將測試類別的位置與被測試程式相對應
	* Look at a project and find all the tests that relate to it
	* Look at a class and find all the tests that relate to it
	* Look at a method and find all the tests that relate to it

     7.4.1 將測試對應到專案
     7.4.2 把測試對應到類別
     7.4.3 將測試對應到明確的工作單元入口
 
7.5 注入橫切面關注點
https://en.wikipedia.org/wiki/Cross-cutting_concern
實驗組、對照組，唯一變量

```csharp=
public static class TimeLogger
{
    public static string CreateMessage(string info)
    {
              // DateTime.Now 是 cross cutting corcern
        return DateTime.Now.ToShortDateString() + " " + info;
    }
}
```

A question many developers ask me when I point out this example is, “How do we make sure everyone uses this class?” My answer is that I do code reviews, and in them I make sure nobody uses DateTime directly.
I try not to rely on tools too much, because I believe true learning happens when two people (or more) are sitting close enough to hear and see each other and can work together and take turns working with the same keyboard to talk about code. But if this is an existing project that we’re converting to use SystemTime, I simply do a “find in files” for code that uses DateTime, and if possible, I simply do a “replace” on all the things I find. SystemTime is named so that it’s easy to find and replace.


7.6 為應用程式建立測試API
	* Use inheritance in your test classes for code reuse, guidance, and more.
	* Create test utility classes and methods.
	* Make your API known to developers.

     7.6.1 使用繼承類別繼承模式
	* Abstract test infrastructure class
	* Template test class
	* Abstract test driver class

	> Listing 7.3 An example of not following the DRY principle in test classes
	```csharp=
	//This class uses the LoggingFacility Internally
	public class LogAnalyzer
	    {
		public void Analyze(string fileName)
		{
		    if (fileName.Length < 8)
		    {
		       LoggingFacility.Log("Filename too short:" + fileName);
		    }
		    //rest of the method here
		}
	}
	//another class that uses the LoggingFacility internally
	public class ConfigurationManager 
	    {
		public bool IsConfigured(string configName)
		{
		    LoggingFacility.Log("checking " + configName);
		    return result;
		}
	}
	public static class LoggingFacility
	    {
		public static void Log(string text)
		{
		    logger.Log(text);
		}
		private static ILogger logger;
		public static ILogger Logger
		{
		    get { return logger; }
		    set { logger = value; }
		}
	}
	   [TestFixture]
	    public class LogAnalyzerTests
	    {
		[Test]
		public void Analyze_EmptyFile_ThrowsException()
		{
		   LogAnalyzer la = new LogAnalyzer();
		   la.Analyze("myemptyfile.txt");
		   //rest of test
		}
		
		[TearDown]
		public void teardown()
		{
		    // need to reset a static resource
		    //  between tests
		   LoggingFacility.Logger = null;
		} 
	}


	[TestFixture]
	public class ConfigurationManagerTests
	{
	    [Test]
	    public void Analyze_EmptyFile_ThrowsException()
	    {
	      ConfigurationManager cm = new ConfigurationManager();
	      bool configured = cm.IsConfigured("something");
	      //rest of test
	    }
	    [TearDown]
	    public void teardown()
	    { 
	     // need to reset a static resource between tests
	    LoggingFacility.Logger = null;
	    }
	}
	```

	> Listing 7.4 A refactored solution
	```csharp=
	[TestFixture]
	public class BaseTestsClass
	{
	     public ILogger FakeTheLogger()
	     // Refactors into a common readable utility method 
	     // to be used by derived classes
	     {
		 LoggingFacility.Logger =
			Substitute.For<ILogger>();
		 return LoggingFacility.Logger;
	     }
	    [TearDown]
	    public void teardown()
	     // Automatic cleanup for derived classes
	    {
	       // need to reset a static resource between tests
	       LoggingFacility.Logger = null;
	    }
	}
	[TestFixture]
	public class ConfigurationManagerTests:BaseTestsClass
	{
	    [Test]
	     public void Analyze_EmptyFile_ThrowsException()
	     {
		FakeTheLogger(); // Call base class helper method
		ConfigurationManager cm = new ConfigurationManager();
		bool configured = cm.IsConfigured("something");
		 //rest of test 
		 } 
	}
	[TestFixture]
	public class LogAnalyzerTests : BaseTestsClass
	{
	    [Test]
	    public void Analyze_EmptyFile_ThrowsException()
	    {
	       FakeTheLogger(); // Call base class helper method
	       LogAnalyzer la = new LogAnalyzer();
	       la.Analyze("myemptyfile.txt");
		//rest of test
	    }
	}
	```

	Here’s a list of possible steps for refactoring your test class:
	1. Refactor: extract the superclass.
	    * Create a base class (BaseXXXTests).
	    * Move the factory methods (like GetParser) into the base class.
	    * Move all the tests to the base class.
	    * Extract the expected outputs into public fields in the base class.
	    * Extract the test inputs into abstract methods or properties that the derived classes will create.
	2. Refactor: make factory methods abstract, and return interfaces.
	3. Refactor: find all the places in the test methods where explicit class types are used, and change them to use the interfaces of those types instead.
	4. In the derived class, implement the abstract factory methods and return the explicit types.

     7.6.2 建立測試輔助類別和方法

     7.6.3 把你的API 介紹給開發人員
	 There are several ways to make sure your APIs are used:
	* Have teams of two people write tests together (at least once in a while), where one is familiar with the existing APIs and can teach the other, as they write new tests, about the existing benefits and code that could be used.
	* Have a short document (no more than a couple of pages) or a cheat sheet that details the types of APIs out there and where to find them. You can create short documents for specific parts of your testing framework (APIs specific to the data layer, for example) or a global one for the whole application. If it’s not short, no one will maintain it. One possible way to make sure it’s up to date is by automating the generation process:
		   * Have a known set of prefixes or postfixes on the API helpers’ names (helper [something], for example).
		   * Have a special tool that parses out the API names and their locations and generates a document that lists them and where to find them, or have some simple directives that the special tool can parse from comments you put on them.
		   * Automate the generation of this document as part of the automated build process.
		   * Discuss changes to the APIs during team meetings—one or two sentences outlining the main changes and where to look for the significant parts. That way the team knows that this is important and it’s always a consideration.
	* Go over this document with new employees during their orientation.
	* Perform test reviews (in addition to code reviews) that make sure tests are up to standards of readability, maintainability, and correctness, and ensure that the right APIs are used when needed. For more on that practice, see http:// 5whys.com/blog/step-4-start-doing-code-reviews-seriously.html on my blog for software leaders.



7.7 小結

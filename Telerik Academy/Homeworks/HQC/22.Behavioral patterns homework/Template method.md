# Template method #
----------
## Обобщение ##


Този шаблон се прилага когато имаме даден алгоритъм който изпълнява дадена задача в определена последователност, но част от стъпките могат да се изпълняват по различен начин в зависимост от имплементацията. 

Алгоритъмът се дефинира като последователност от операции в базов клас с използване на абстрактни методи, което позволява наследниците на класа да променят конкретната имплементация (чрез презаписване на абстрактните методи) без да променят последователността на прилагането им. На наследниците не се позволява да променят структурата на алгоритъма, а само логиката на конкретен прилаган метод.
 
## Цели ##
- Да дефинира основния алогритъм на дадена операция, давайки възможност на наследници да променят конкретни методи в него
- да се избегне повтарянето на код при имплементация на операции, които се различават само в определени стъпки (методи)
- Да се контролира какво е позволено наследниците да променят


## Приложимост ##

Шаблонът се прилага, когато

- се налага да имплементираме общи, непроменяеми части от алгоритъм, оставяйки на наследниците да имплементират вариациите в поведение
- се рефракторира съществуващ код и се установи повтаряне на код в класовете. Повтарящият се код се изнася в общия темплейтен клас




## Структура ##
![](http://www.dofactory.com/images/diagrams/net/template.gif)

**AbstractClass** - дефинира примитивните абстрактни методи, които съставят определния алгоритъм, както и общите методи

**ConcreteClass** - имплементира абстрактните методи, които варират сспоред конкретната имплементация

## Имплементация

	using System;
	using System.Data;
	using System.Data.OleDb;
	 
	namespace DoFactory.GangOfFour.Template.RealWorld
	{
	  /// <summary>
	  /// MainApp startup class for Real-World 
	  /// Template Design Pattern.
	  /// </summary>
	  class MainApp
	  {
	    /// <summary>
	    /// Entry point into console application.
	    /// </summary>
	    static void Main()
	    {
	      DataAccessObject daoCategories = new Categories();
	      daoCategories.Run();
	 
	      DataAccessObject daoProducts = new Products();
	      daoProducts.Run();
	 
	      // Wait for user
	      Console.ReadKey();
	    }
	  }
	 
	  /// <summary>
	  /// The 'AbstractClass' abstract class
	  /// </summary>
	  abstract class DataAccessObject
	  {
	    protected string connectionString;
	    protected DataSet dataSet;
	 
	    public virtual void Connect()
	    {
	      // Make sure mdb is available to app
	      connectionString =
	        "provider=Microsoft.JET.OLEDB.4.0; " +
	        "data source=..\\..\\..\\db1.mdb";
	    }
	 
	    public abstract void Select();
	    public abstract void Process();
	 
	    public virtual void Disconnect()
	    {
	      connectionString = "";
	    }
	 
	    // The 'Template Method' 
	    public void Run()
	    {
	      Connect();
	      Select();
	      Process();
	      Disconnect();
	    }
	  }
	 
	  /// <summary>
	  /// A 'ConcreteClass' class
	  /// </summary>
	  class Categories : DataAccessObject
	  {
	    public override void Select()
	    {
	      string sql = "select CategoryName from Categories";
	      OleDbDataAdapter dataAdapter = new OleDbDataAdapter(
	        sql, connectionString);
	 
	      dataSet = new DataSet();
	      dataAdapter.Fill(dataSet, "Categories");
	    }
	 
	    public override void Process()
	    {
	      Console.WriteLine("Categories ---- ");
	 
	      DataTable dataTable = dataSet.Tables["Categories"];
	      foreach (DataRow row in dataTable.Rows)
	      {
	        Console.WriteLine(row["CategoryName"]);
	      }
	      Console.WriteLine();
	    }
	  }
	 
	  /// <summary>
	  /// A 'ConcreteClass' class
	  /// </summary>
	  class Products : DataAccessObject
	  {
	    public override void Select()
	    {
	      string sql = "select ProductName from Products";
	      OleDbDataAdapter dataAdapter = new OleDbDataAdapter(
	        sql, connectionString);
	 
	      dataSet = new DataSet();
	      dataAdapter.Fill(dataSet, "Products");
	    }
	 
	    public override void Process()
	    {
	      Console.WriteLine("Products ---- ");
	      DataTable dataTable = dataSet.Tables["Products"];
	      foreach (DataRow row in dataTable.Rows)
	      {
	        Console.WriteLine(row["ProductName"]);
	      }
	      Console.WriteLine();
	    }
	  }
	}	

## Особености ##

- базовия клас не е нужно да е абстрактен. Същият може да има функционалност и  само да съдържа определен (темплейтен) метод, който подлежи на презаписване от наследниците

- важно е да се изнесе максимално количество код в темплейтния клас. Това от една страна намалява повтарянето на кода, а от друга страна улеснява добавянето на наследници

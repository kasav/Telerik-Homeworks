# Chain of responsibility #
----------
## Обобщение ##


Този шаблон позволява даден обект да изпрати команда без да знае кой обект ще я приеме и изпълни. Заявката се препраща верижно от един обект на друг и всеки във веригата може да изпълни командата, да я предаде на следващия или да направи и двете. Всеки от обектите във веригата съдържа логика, която дефинира командите, които съответните обекти са способни да изпълняват. 
 
## Цели ##
- Да се създаде ясно дефинирана йерархия на отговорности в дадена система
- Да избегне директна връзка между изпращача и получателя на заявката, като по този начин даде възможност и на други да я изпълнят. 
- Да позволи заявка да се прерпраща верижно по отделни обекти докато бъде изпълнена


## Приложимост ##

- когато изпълнителя не е известен предварително
- когато изпълнителя трябва да бъде избран автоматично
- когато заявката трябва да се адресира до група обекти без да се указва кой ще я изпълнява
- когато изпълнителя следва да се създаде динамично, в процеса на работа на програмата
- когато повече от един обект може да изпълни съответна заявка



## Структура ##
![](http://images.techhive.com/images/idge/imported/article/jvw/2004/08/jw-0816-chain1-100156541-orig.gif)

**Handler** - дефинира интерфейса за изпълняване на заявката

**RequestHandler** - обработва заявките, за които е отговорен. Ако може да обработи подадената заявка го прави, в противен случай прераща заявката на свой наследник

**Client** - изпраща заявката на първия във веригата

## Имплементация

	using System;
	 
	namespace DoFactory.GangOfFour.Chain.RealWorld
	{
	  /// <summary>
	  /// MainApp startup class for Real-World 
	  /// Chain of Responsibility Design Pattern.
	  /// </summary>
	  class MainApp
	  {
    /// <summary>
    /// Entry point into console application.
    /// </summary>
    static void Main()
    {
      // Setup Chain of Responsibility
      Approver larry = new Director();
      Approver sam = new VicePresident();
      Approver tammy = new President();
 
      larry.SetSuccessor(sam);
      sam.SetSuccessor(tammy);
 
      // Generate and process purchase requests
      Purchase p = new Purchase(2034, 350.00, "Assets");
      larry.ProcessRequest(p);
 
      p = new Purchase(2035, 32590.10, "Project X");
      larry.ProcessRequest(p);
 
      p = new Purchase(2036, 122100.00, "Project Y");
      larry.ProcessRequest(p);
 
      // Wait for user
      Console.ReadKey();
	    }
	  }
	 
	  /// <summary>
	  /// The 'Handler' abstract class
	  /// </summary>
	  abstract class Approver
	  {
    protected Approver successor;
 
    public void SetSuccessor(Approver successor)
    {
      this.successor = successor;
    }
 
    public abstract void ProcessRequest(Purchase purchase);
	  }
	 
	  /// <summary>
	  /// The 'ConcreteHandler' class
	  /// </summary>
	  class Director : Approver
	  {
    public override void ProcessRequest(Purchase purchase)
    {
      if (purchase.Amount < 10000.0)
      {
        Console.WriteLine("{0} approved request# {1}",
          this.GetType().Name, purchase.Number);
      }
      else if (successor != null)
      {
        successor.ProcessRequest(purchase);
      }
    }
	  }
	 
	  /// <summary>
	  /// The 'ConcreteHandler' class
	  /// </summary>
	  class VicePresident : Approver
	  {
    public override void ProcessRequest(Purchase purchase)
    {
      if (purchase.Amount < 25000.0)
      {
        Console.WriteLine("{0} approved request# {1}",
          this.GetType().Name, purchase.Number);
      }
      else if (successor != null)
      {
        successor.ProcessRequest(purchase);
      }
    }
	  }
	 
	  /// <summary>
	  /// The 'ConcreteHandler' class
	  /// </summary>
	  class President : Approver
  		{
    public override void ProcessRequest(Purchase purchase)
    {
      if (purchase.Amount < 100000.0)
      {
        Console.WriteLine("{0} approved request# {1}",
          this.GetType().Name, purchase.Number);
      }
      else
      {
        Console.WriteLine(
          "Request# {0} requires an executive meeting!",
          purchase.Number);
      }
    }
	  }
	 
	  /// <summary>
	  /// Class holding request details
	  /// </summary>
	  class Purchase
	  {
	    private int _number;
	    private double _amount;
	    private string _purpose;
	 
    // Constructor
    public Purchase(int number, double amount, string purpose)
    {
      this._number = number;
      this._amount = amount;
      this._purpose = purpose;
    }
 
    // Gets or sets purchase number
    public int Number
    {
      get { return _number; }
      set { _number = value; }
    }
 
    // Gets or sets purchase amount
    public double Amount
    {
      get { return _amount; }
      set { _amount = value; }
    }
 
    // Gets or sets purchase purpose
    public string Purpose
    {
      get { return _purpose; }
      set { _purpose = value; }
    }
	  }
	}



## Особености ##

- Заявките се обаботват на принципа 'обработи са или предай на друг', тоест едни заявки се обработват на нивото, на което са получени, а други се препращат на обекти от друго ниво

- Всички заявки трябва да могат да се обработят от поне един обект в системата. Следва да се осигури механизъм за добавяне на нови обекти във веригата

- Може да съществува повече от един хендлър, но само един обработва конкретна заявка

- Изпращача на заявката има референция само към един обект, без да знае колко обекта има във веригата, на кого ще се предаде заявката, колко обекта има по веригата. Промяната на броя на обектите във веригата не променя работата на системата

- След намиране на първия обект, способен да изпълни заявката, заявката се обработва и веригата се терминира

## Максимално съкратена версия на шаблона :)) ##

![](http://lh3.googleusercontent.com/-IuQY8iN5ukk/Tr9ztDayo1I/AAAAAAAABO0/kylQbTI35tQ/s1600/image%25255B25%25255D.png)




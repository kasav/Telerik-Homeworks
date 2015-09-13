# Object pool #
----------

## Цели ##
Да се подобри ефективността и да се намалят ресурсите, нужни на приложението посредством създаване на нови обекти чрез клониране на вече съществуващ обект.  При операцията се избягва използването на new и свързаното с нея заделяне на ресурси или скъпо струващи операции като достъп до база данни.

## Приложимост ##

Шаблонът Prototype позволява създаването на нови обекти без да е необходимо да се знае класът им или детайли, свързани с тяхното създаване. Обикновено се прилага, когато се иска да се създаде клонинг, представляващ true copy на друг обект *по време на изпълнение на програмата*. *True copy* обозначава обект, чиито арибути са същите като на оригиналния, за разлика от *новото инстанциране* (с използването на new), при което атрибутите на новия обект се задават наново. 

Подобен подход е подходящ при

- избягване на трудоемка операция като инстанциране на нов обект при което е нужно свързване с база данни и търсене на атрибути, които вече са налични в друг обект
- запазване на моментното състояние на обект, за да може да се достъпи на по-късен етап, например save функционалността при игри, настройки и т.н.



## Структура ##
![](https://upload.wikimedia.org/wikipedia/commons/thumb/1/14/Prototype_UML.svg/600px-Prototype_UML.svg.png)



**Client** - създава нов обект като подава команда на прототипа да създаде свой клонинг

**Prototype** - декларира интерфейс, който позволява клонирането му

**ConcretePrototype** - изпълнява операцията за своето клониране

Процесът на клониране започва от инстанциран и инициализиран клас. Клиентът поисква нова инстанция от обекта и изпраща заявката към класа *Prototype*. Конкретния прототип ще извърши клонирането си чрез clone() метода

## Имплементация

	/// <summary>
	/// The 'Prototype' interface
	/// </summary>
	public interface IEmployee
	{
	 IEmployee Clone();
	 string GetDetails();
	}
	 
	/// <summary>
	/// A 'ConcretePrototype' class
	/// </summary>
	public class Developer : IEmployee
	{
	 public int WordsPerMinute { get; set; }
	 public string Name { get; set; }
	 public string Role { get; set; }
	 public string PreferredLanguage { get; set; }
	 
	 public IEmployee Clone()
	 {
	 // Shallow Copy: only top-level objects are duplicated
	 return (IEmployee)MemberwiseClone();
	 
	 // Deep Copy: all objects are duplicated
	 //return (IEmployee)this.Clone();
	 }
	 
	 public string GetDetails()
	 {
	 return string.Format("{0} - {1} - {2}", Name, Role, PreferredLanguage);
	 }
	}
	 
	/// <summary>
	/// A 'ConcretePrototype' class
	/// </summary>
	public class Typist : IEmployee
	{
	 public int WordsPerMinute { get; set; }
	 public string Name { get; set; }
	 public string Role { get; set; }
	 
	 public IEmployee Clone()
	 {
	 // Shallow Copy: only top-level objects are duplicated
	 return (IEmployee)MemberwiseClone();
	 
	 // Deep Copy: all objects are duplicated
	 //return (IEmployee)this.Clone();
	 }
	 
	 public string GetDetails()
	 {
	 return string.Format("{0} - {1} - {2}wpm", Name, Role, WordsPerMinute);
	 }
	}
	 
	/// <summary>
	/// Prototype Pattern Demo
	/// </summary>
	 
	class Program
	{
	 static void Main(string[] args)
	 {
	 Developer dev = new Developer();
	 dev.Name = "Rahul";
	 dev.Role = "Team Leader";
	 dev.PreferredLanguage = "C#";
	 
	 Developer devCopy = (Developer)dev.Clone();
	 devCopy.Name = "Arif"; //Not mention Role and PreferredLanguage, it will copy above
	 
	 Console.WriteLine(dev.GetDetails());
	 Console.WriteLine(devCopy.GetDetails());
	 
	 Typist typist = new Typist();
	 typist.Name = "Monu";
	 typist.Role = "Typist";
	 typist.WordsPerMinute = 120;
	 
	 Typist typistCopy = (Typist)typist.Clone();
	 typistCopy.Name = "Sahil";
	 typistCopy.WordsPerMinute = 115;//Not mention Role, it will copy above
	 
	 Console.WriteLine(typist.GetDetails());
	 Console.WriteLine(typistCopy.GetDetails());
	 
	 Console.ReadKey();
	 
	 }
	}

## Особености ##
- използването на deep copy срещу използването на shallow copy. Обикновено втория вариант е по-прост, но при прилагане може да остане зависимост между обекта и копието му, в случай, че атрибутите на оригинала сочат към референтни данни. В подобен случай новосъздадения клонинг ползва същите референции и е възможно двата обекта да достъпват едни и същи данни
- когато приложените използва множество прототипи, които се създават и изтриват динамично, се налага създаването на регистър на прототипите


## Проблеми ##
Съществуват ситуации, в които вътрешното състояние на обект трябва да се зададе *след*, а не в *момента на* създаването му. Подобни случаи могат да наложат създаване на метод, който да подава като параметри стойностите, които трябва да бъдат зададени на по-късен етап





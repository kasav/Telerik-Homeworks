# Decorator #
----------

## Цели ##
- Да добави допълнителна функционалност на обект по време на изпълнение на програмата (динамично). 
- Функционалността да може да се използва преди, след или вместо оригиналната функционалност на обекта, без това да засяга останалите обекти от класа



## Приложимост ##
- за добавяне на поведение или състояние на обект (декориране) по време на изпълнение, в случаи, в които наследяването не е подходяща опция, тъй като е статично и се отнася за целия клас, към който принадлежи обекта, следователни и за всички останали инстанции. Декораторът предлага решение без създаване на подкласове
- за разширение на запечатани (sealed) класове
- за добавяне на функционалности на UI контролери


## Структура ##
![](http://www.oodesign.com/images/design_patterns/structural/decorator-design-pattern-implementation-uml-class-diagram.png)

**Component** -  Интерфейс за обекти, на които динамично може да бъде добавяна функционалност

**ConcreteComponent** - Дефинира обект, на който ще бъде зададена допълнителна функционалност

**Decorator** - съдържа референция към Component обект и дефинира интерфейс, който съответства на интерфейса на Component-a.

**Concrete Decorators** - разширяват функционалността на обекта, добавяйки състояние или поведение

## Имплементация

	/// <summary>
	/// The 'Component' interface
	/// </summary>
	public interface Vehicle
	{
	 string Make { get; }
	 string Model { get; }
	 double Price { get; }
	}
	 
	/// <summary>
	/// The 'ConcreteComponent' class
	/// </summary>
	public class HondaCity : Vehicle
	{
	 public string Make
	 {
	 get { return "HondaCity"; }
	 }
	 
	 public string Model
	 {
	 get { return "CNG"; }
	 }
	 
	 public double Price
	 {
	 get { return 1000000; }
	 }
	}
	 
	/// <summary>
	/// The 'Decorator' abstract class
	/// </summary>
	public abstract class VehicleDecorator : Vehicle
	{
	 private Vehicle _vehicle;
	 
	 public VehicleDecorator(Vehicle vehicle)
	 {
	 _vehicle = vehicle;
	 }
	 
	 public string Make
	 {
	 get { return _vehicle.Make; }
	 }
	 
	 public string Model
	 {
	 get { return _vehicle.Model; }
	 }
	 
	 public double Price
	 {
	 get { return _vehicle.Price; }
	 }
	 
	}
	 
	/// <summary>
	/// The 'ConcreteDecorator' class
	/// </summary>
	public class SpecialOffer : VehicleDecorator
	{
	 public SpecialOffer(Vehicle vehicle) : base(vehicle) { }
	 
	 public int DiscountPercentage { get; set; }
	 public string Offer { get; set; }
	 
	 public double Price
	 {
	 get
	 {
	 double price = base.Price;
	 int percentage = 100 - DiscountPercentage;
	 return Math.Round((price * percentage) / 100, 2);
	 }
	 }
	 
	}
	 
	/// <summary>
	/// Decorator Pattern Demo
	/// </summary>
	class Program
	{
	 static void Main(string[] args)
	 {
	 // Basic vehicle
	 HondaCity car = new HondaCity();
	 
	 Console.WriteLine("Honda City base price are : {0}", car.Price);
	 
	 // Special offer
	 SpecialOffer offer = new SpecialOffer(car);
	 offer.DiscountPercentage = 25;
	 offer.Offer = "25 % discount";
	 
	 Console.WriteLine("{1} @ Diwali Special Offer and price are : {0} ", offer.Price, offer.Offer);
	 
	 Console.ReadKey();
	 
	 }
	}

## Подобни шаблони ##
- Adaptor

## Особености ##

- Decorator шаблонът дава допълнителна функционалност на съществуващ обект и предоставя същия, но вече *подобрен* интерфейс, докато  Adaptor е предназначен да предостави нов, *различен* интерфейс
- представлява алтернатива на наследяването, при която може да се манипулира единствена инстанция от клас, без да се засягат останалите
- върху един обект може да се изпълни повече от един декоратор, всеки от които добавя нова функционалност







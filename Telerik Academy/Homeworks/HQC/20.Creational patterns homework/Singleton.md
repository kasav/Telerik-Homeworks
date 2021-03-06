# Сингълтон (Singleton) #
----------

## Цели ##
- Да контролира създаването на обекти и да гарантира създаването на една единствена инстанция


- Да предостави глобална точка на достъп до създадения обект.

## Приложимост ##

Сингълтонът се използва в случаи, когато трябва да се осигури да съществува една единствена инстанция от даден клас, която може да се достъпва от известно място за достъп, например
 
- клас управляващ появяването на диалогови прозорци
- обекти  използвани за водене на логове
- обекти съхраняващи настройки
- предпочитания или настройки в регистрите
- обекти функциониращи като драйвери за принтери, графични карти, бази данни и др.


## Структура ##
 ![](http://www.oodesign.com/images/design_patterns/creational/singleton_implementation_-_uml_class_diagram.gif)

Сингълтон шаблонът притежава най-елементарната клас диаграма сред всички останали шаблони – съдържа един клас, което е предпоставка да бъде възприеман за един от най-лесно реализуемите.

Определя операция getInstance(), която предоставя на потребителите достъп до нейната уникална инстанция.

Клиентите достъпват шаблона единствено чрез операцията за вземане на единствената създадена инстанция.

## Имплементация

	using System;	
	public class Singleton
	{
	   private static Singleton instance;	
	   private Singleton() {}	
	   public static Singleton Instance
	   {
	      get 
	      {
	         if (instance == null)
	         {
	            instance = new Singleton();
	         }
	         return instance;
	      }
	   }
	}

Горната имплементация има 2 основни достойнства - класът може да притежава допълнителна функционалност, като освен това обектът не се инициализира докато изрично не бъде поискан - т.нар. "lazy loading". Недостатък се явяват потенциални проблеми при работа в многонишкова среда. Уникалността на инстанцията в многонишкови приложения се налага да бъде запазена чрез заключване на достъпа на повече от една нишка:

	using System;
		public sealed class Singleton
		{
		   private static volatile Singleton instance;
		   private static object syncRoot = new Object();
		
		   private Singleton() {}
		
		   public static Singleton Instance
		   {
		      get 
		      {
		         if (instance == null) 
		         {
		            lock (syncRoot) 
		            {
		               if (instance == null) 
		                  instance = new Singleton();
		            }
		         }
		         return instance;
		      }
		   }
		}


## Особености ##
- Контролиране на достъпа до единствената инстанция. Тъй като сингълтонът енкапсулира неговата единствена инстанция, той може да определя времето и начина на достъп до него.
- Подобрява производителността. В случай, че имаме обект без състояние, който се създава многократно, Сингълтонът премахва необходимостта от постоянното създаване и унищожаване на обекти.
- Като алтернатива, могат да се използват статичните методи на класа.
- Ограничаване на претрупването с имена. Представлява подобрение над глобалните променливи. Предпазва от претрупване на пространството от имена с глобални променливи, които съхраняват уникални инстанции.
- Позволява по-прецизното дефиниране на операции и инстанции на класове. Сингълтон класът е възможно да се наследява, следователно е възможно конфигурирането на програма, така че по време на изпълнението и да се инстанцира желаният сингълтон клас. 
- Възможно е създаваненето на повече от една инстанция на сингълтона, като контролирането на създаването и броя инстанции се извършва в метода предоставящ  достъп до инстанцията на сингълтона.
- Гъвкавост. Шаблонът ни предлага по-голяма свобода от статичните методи в клас и от статичните класове. В случай на нужда, предлага възможност за промяна на дизайна, за да бъде възможно инстанцирането на повече от един обект. Освен това, статичните член функции не са никога virtual, така класовете наследници не могат да ги препокрият. За да изберем между използването на статичен клас и сингълтон, се ръководим от следните разсъждения: ако имаме набор от функции, които трябва да бъдат държани заедно, тогава избираме static клас.  Всичко останало, което се нуждае от достъп до ресурси, е желателно да бъде имплементирано като сингълтон. 

## Проблеми ##
- Могат да се получат неумишлени или несъзнателни промени – програмистът може да промени стойността на някоя глобална променлива на едно място и да предполага, че променливата е останала непроменена на други места в кода.
- Множество препратки към един обект – възможно е да се обръщаме към една стойност от две разливни имена, без да забележим, че те сочат към една променлива.
- Трудно преизползване на кода – Класовете зависещи от глобалната променлива могат да се използват само в приложения, в които тя съществува.
- Лоша модулярност – Всичките класове, които използват глобалните данни са плътно свързани (tightly coupled) 
- Затруднения при тестването – По трудно се указва на даден клас да използва макет на обекта вместо глобалния, без да се правят промени по кода. 
- Нужно е да се отдели специално внимание на шаблона при работа в многонишкова среда, за да се гарантира уникалността на инстанцията






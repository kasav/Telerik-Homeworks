# Strategy #
----------
## Обобщение ##


Този шаблон се прилага когато имаме алгоритми, които решават един и същ проблем, но по различни начини. В тези случаи често се оказва практично да се изолират алгоритмите в отделни класове, за да има възможност тези алгоритми да се избират и да се прилагат самостоятелно. Входните и изходните данни остават еднакви, като начинът да се достигне крайния резултат е въпрос на избор на определен алгоритъм (стратегия).

## Цели ##
- Да се енкапсулира логиката на определена операция
- Да се позволи да се варира с прилагания алгоритъм и така да стане възможна взаимната заменяемост на алгоритми
- Да се отдели конкретен алгоритъм от начина на представяне на резултатите


## Приложимост ##

Шаблонът се прилага, когато

- кодът е утежнен от наличието на множество if/switch оператори
- когато се налага добавяне на нова операция, извършвана по определен алгоритъм, което би довело до модифициране на класа



## Структура ##
![](https://upload.wikimedia.org/wikipedia/commons/3/39/Strategy_Pattern_in_UML.png)

**Strategy** - дефинира интерфейс, общ за всички приложими алгоритми *Context* използва този интерфейс за да извика алгоритмите, дефинирани в *ConcreteStrategy*.

**ConcreteStrategy** - всяка конкретна стратегия (имплементиран алгоритъм).

**Context** -  съдържа референция към стратегията за обекта. Може да дефинира инерфейс, който позволява стратегията (алгоритъма) да достъпва данните, които съдържа.


## Имплементация

	public interface IShippingStrategy
    {
        double Calculate(Order order);
    }

	public class SchenkerShippingStrategy : IShippingStrategy
    {
        public double Calculate(Order order)
        {
            return 3.00d;
        }
    }

	public class UpsShippingStrategy : IShippingStrategy
    {
        public double Calculate(Order order)
        {
            return 4.25d;
        }
    }

	public class FedexShippingStrategy : IShippingStrategy
    {
        public double Calculate(Order order)
        {
            return 5.00d;
        }
    }

	public class CostCalculationService_WithStrategy
    {
        private readonly IShippingStrategy _shippingStrategy;
 
        public CostCalculationService_WithStrategy(IShippingStrategy shippingStrategy)
        {
            _shippingStrategy = shippingStrategy;
        }
 
        public double CalculateShippingCost(Order order)
        {
            return _shippingStrategy.Calculate(order);
        }
    }

## Особености ##

- Стратегиите принципно не могат да използват данните, съдържащи се в конкретния обект (клас). Това може да се позволи със създаването на нови класове, съдържащи нужната енкапсулирана информация, или чрез подаването на самия обект като аргумент на стратегията
- Тестването на кода е улеснено, тъй като се позволява избора на алгоритъм и директното му тестване
- Стратегиите могат да бъдат mock-нати (имитирани) и при тестване може да се имитира даден обект без да се налага достъп до него
- Добавянето на нова или подменянето на съществуваща стратегия не променя поведението на програмата

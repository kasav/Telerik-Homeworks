# Facade #
----------

## Цели ##
- Да се създаде опростено представяне на сложен интерфейс чрез представяне на нов, опростен интерфейс над първия. 


## Приложимост ##

- при големи и сложни системи
- при нужда от отделен достъп до слоеве на многослойно приложение
- при tight coupling на абстракции и имплементации на подсистеми
- неудобно API на ползвана библиотека

Шаблонът се прилага често в случаи, в които дадена система е обширна и трудна за работа, тъй като се състои от голям брой класове и сорс кода и не е достъпен. Фасадата е външен обект, който скрива сложността на системата, пред която е поставен, и дава на клиента прост интерфейс за работа.




## Структура ##
![](http://www.dofactory.com/images/diagrams/net/facade.gif)

**Facade**

- Знае кои подсистеми са отговорни за заявката.
- Делегира заявките към подходящите подсистеми.

**Subsystem classes**

- имплементира функционалността на приложението
- не знае за фасадата и няма референция към нея


## Имплементация

	/* Complex parts */
	
	class CPU {
	    public void freeze() { ... }
	    public void jump(long position) { ... }
	    public void execute() { ... }
	}
	
	class Memory {
	    public void load(long position, byte[] data) { ... }
	}
	
	class HardDrive {
	    public byte[] read(long lba, int size) { ... }
	}
	
	/* Facade */
	
	class ComputerFacade {
	    private CPU processor;
	    private Memory ram;
	    private HardDrive hd;

    public ComputerFacade() {
        this.processor = new CPU();
        this.ram = new Memory();
        this.hd = new HardDrive();
    }

    public void start() {
        processor.freeze();
        ram.load(BOOT_ADDRESS, hd.read(BOOT_SECTOR, SECTOR_SIZE));
        processor.jump(BOOT_ADDRESS);
        processor.execute();
    }
	}
	
	/* Client */
	
	class You {
	    public static void main(String[] args) {
	        ComputerFacade computer = new ComputerFacade();
	        computer.start();
	    }
	}

## Подобни шаблони ##
- Adaptor

## Особености ##
- Фасадата дефинира нов интерфейс за разлика от Adaptor шаблона, който кара два интерфейса да работят заедна вместо да създава нов
- Фасадата показва как се създава един обект, който да представя цяла подсистема, докато Flyweight показва създането на множество малки обекти
- Обектите на Фасадата често са Сингълтън обекти, тъй като е необходимо създаването на единствен обект
- Възможно е по-сложни системи да бъдат представени от няколко различни фасади, всяка от които дава опростен достъп до различни подсистеми или набор от функционалности






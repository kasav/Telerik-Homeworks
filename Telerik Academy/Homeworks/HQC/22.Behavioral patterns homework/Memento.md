# Memento #
----------
## Обобщение ##


Този шаблон позволява запазването на моментното състояние на обект с цел да стане възможно връщането на обекта в това състояние. Клиентският код не знае как  различните състояния се съхраняват или възстановяват. Достъпът до обекта в който състоянието е запазено, като всички взаимодействия с него, се осъществяват чрез специален клас, отговорен за създаването му.
 
## Цели ##
- Да се запази моментното състояние на обект
- Да се позволи връщането на обект в предварително запазеното моментно състояние


## Приложимост ##

- Осъществяване на undo/redo функционалности
- Позволяване на save/load функционалности



## Структура ##
![](http://www.oodesign.com/images/design_patterns/structural/memento-design-pattern-implementation-uml-class-diagram.png)

**Memento** - запаза вътрешното състояние на обекта *Originator* в променливата *state*.

**Originator** - създава *memento* обект, в който се съхранява вътрешното състояние на *originator*. *Мemento* се използа за възстановяване на това (вече предишно) състояние.

**Caretaker** - класът, който управлява списъка от *memento* обекти. Чрез него се осъществява връзката с клиентския код.

## Имплементация ##

	public class Book
	{
    private string _isbn;
    private string _title;
    private string _author;
    private DateTime _lastEdited;
 
    public string ISBN
    {
        get { return _isbn; }
        set
        {
            _isbn = value;
            SetLastEdited();
        }
    }
 
    public string Title
    {
        get { return _title; }
        set
        {
            _title = value;
            SetLastEdited();
        }
    }
 
    public string Author
    {
        get { return _author; }
        set
        {
            _author = value;
            SetLastEdited();
        }
    }
 
    public Book()
    {
        SetLastEdited();
    }
 
    private void SetLastEdited()
    {
        _lastEdited = DateTime.UtcNow;
    }
 
    public Memento CreateUndo()
    {
        return new Memento(_isbn, _title, _author, _lastEdited);
    }
 
    public void RestoreFromUndo(Memento memento)
    {
        _title = memento.Title;
        _author = memento.Author;
        _isbn = memento.ISBN;
        _lastEdited = memento.LastEdited;
    }
 
    public void ShowBook()
    {
        Console.WriteLine(
            "{0} - '{1}' by {2}, edited {3}.", ISBN, Title, Author, _lastEdited); 
    }
	}
 
 
	public class Memento
	{
    public string ISBN { get; private set; }
    public string Title { get; private set; }
    public string Author { get; private set; }
    public DateTime LastEdited { get; private set; }
 
    public Memento(string isbn, string title, string author, DateTime lastEdited)
    {
        ISBN = isbn;
        Title = title;
        Author = author;
        LastEdited = lastEdited;
    }
	}
 
 
	public class Caretaker
	{
	    public Memento Memento { get; set; }
	}

## Особености ##

- шаблонът енкапсулира данните за моментното състояние на обекта, които съхранява, което го прави удачен в случаи, в които тази информация не трябва да е достъпна за външния свят

- в зависимост от обема на съхраняваната информация и сложността на логиката, с която CareTaker класа управлява управлява мементо-обекта може да се забави работата на приложението.


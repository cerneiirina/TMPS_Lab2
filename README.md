Лабораторная №2

*Подготовила - Черней Ирина, группа TI-164*

*Целью данной лабораторной работы было имплементировать 5 структурных (можно было использовать и поведенческие) шабловнов*

Были выбраны 5 шаблонов:

## Decorator
![Decorator](https://refactoring.guru/images/patterns/cards/decorator-mini-2x.png)

Декоратор — это структурный паттерн проектирования, который позволяет динамически добавлять объектам новую функциональность, оборачивая их в полезные «обёртки».
Декоратор имеет альтернативное название — обёртка. Оно более точно описывает суть паттерна: вы помещаете целевой объект в другой объект-обёртку, который запускает базовое поведение объекта, а затем добавляет к результату что-то своё.

Оба объекта имеют общий интерфейс, поэтому для пользователя нет никакой разницы, с каким объектом работать — чистым или обёрнутым. Вы можете использовать несколько разных обёрток одновременно — результат будет иметь объединённое поведение всех обёрток сразу.

Функция Draw включает в себя вызов нескольних внутренних методов
```
Draw(){
        this.Context.beginPath();
        this.Context.arc(this.X,this.Y,this.D,0,2*Math.PI);
        this.Context.stroke();
        this.Context.fillStyle = new ColorAdapter(this.Color).Hex;
        this.Context.fill();
    }
```
## Bridge
![Bridge](https://refactoring.guru/images/patterns/cards/bridge-mini-2x.png)

Мост — это структурный паттерн проектирования, который разделяет один или несколько классов на две отдельные иерархии — абстракцию и реализацию, позволяя изменять их независимо друг от друга.

Пример проблемы при которой можно использовать этот паттерн :

У вас есть класс геометрических Фигур, который имеет подклассы Круг и  Квадрат. Вы хотите расширить иерархию фигур по цвету, то есть иметь Красные и Синие фигуры. Но чтобы всё это объединить, вам придётся создать 4 комбинации подклассов, вроде СиниеКруги и КрасныеКвадраты.

![Bridge](https://refactoring.guru/images/patterns/diagrams/bridge/problem-2x.png)

При добавлении новых видов фигур и цветов количество комбинаций будет расти в геометрической прогрессии. Например, чтобы ввести в программу фигуры треугольников, придётся создать сразу два новых подкласса треугольников под каждый цвет. После этого новый цвет потребует создания уже трёх классов для всех видов фигур. Чем дальше, тем хуже.


Класс Circle включает в себя параметры ColorAdapter и Color, которые являются классами идентифицирующие цвет

```
class Circle {
    constructor(X, Y, D, Color, Context) {
        this.History = [this];
        this.X = X;
        this.Y = Y;
        this.D = D;
        this.Color = Color;
        this.Adapter = new ColorAdapter(this.Color);
        this.Context = Context;
    }
}
```

## Iterator
![Iterator](https://refactoring.guru/images/patterns/cards/iterator-mini-2x.png)

Итератор — это поведенческий паттерн проектирования, который даёт возможность последовательно обходить элементы составных объектов, не раскрывая их внутреннего представления.

Идея паттерна Итератор состоит в том, чтобы вынести поведение обхода коллекции из самой коллекции в отдельный класс.

![Iterator](https://refactoring.guru/images/patterns/diagrams/iterator/solution1-2x.png)

Данный паттерн реализуется следующим интерфейсов:

```
interface IIterator{
    next();
    hasMore();
}

export default IIterator
```

А интерфейс реализуется классом

```
import IITerator from './IITerator'

class Iterator implements IITerator{
    private currentPos = 0;
    private Collection:Array<any> = new Array();

    Iterator(){ }

    next():void{
        if(this.currentPos < this.Collection.length){
            return this.Collection[this.currentPos++]
        }
        else{
            this.currentPos = 0
        }
    }
    push(item:any):void{
        this.Collection.push(item)
    }
    pop():void{
        this.Collection.pop()
    }
    hasMore():Boolean{
        if(this.currentPos < this.Collection.length){
            return true;
        }
        else{
            return false;
        }
    }
}

export default Iterator
```

## Memento
![Iterator](https://refactoring.guru/images/patterns/cards/memento-mini-2x.png)

Снимок — это поведенческий паттерн проектирования, который позволяет сохранять и восстанавливать прошлые состояния объектов, не раскрывая подробностей их реализации.

Предположим, что вы пишете программу текстового редактора. Помимо обычного редактирования, ваш редактор позволяет менять форматирование текста, вставлять картинки и прочее.

В какой-то момент вы решили сделать все эти действия отменяемыми. Для этого вам нужно сохранять текущее состояние редактора перед тем, как выполнить любое действие. Если потом пользователь решит отменить своё действие, вы достанете копию состояния из истории и восстановите старое состояние редактора.

![Iterator](https://refactoring.guru/images/patterns/diagrams/memento/problem1-ru-2x.png)

Данный паттерн реализуется интерфейсом

```
interface IMementoCircle{
    getName():string;
    getSnapshot(version:number)
}

export default IMementoCircle;
```

А интерфейс реализуется
```
class Circle implements IMementoCircle{
    ...
    getName(){
        return "Circle";
    }

    getSnapshot(version:number){
        if(this.History.length > version){
            return this.History[version];
        }
        else{
            return this;
        }
    }
}
```

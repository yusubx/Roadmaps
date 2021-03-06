### Resource for creational patterns by [Kamran Ahmed](https://github.com/kamranahmedse/design-patterns-for-humans#creational-design-patterns)

> ## You can tap Patterns' names to open [dartpad.dev](dartpad.dev) to run code

# [1. Singleton](https://dartpad.dev/cd419b3d4b46cae00ac4fef7ccd5bf8e?)
**Singleton** is a creational design pattern that lets you ensure that a class has only one instance, while providing a global access point to this instance.

**Reading Resources**
- [Flutter Design Patterns: 1 — Singleton by Mangirdas Kazlauskas](https://medium.com/flutter-community/flutter-design-patterns-1-singleton-437f04e923ce)
- [Singleton by Refactoring Guru](https://refactoring.guru/design-patterns/singleton)

```dart
void main() {
  final s1 = Singleton();
  print('Singleton: ${s1.hashCode}');

  final s2 = Singleton();
  print('Singleton: ${s2.hashCode}\n');

  final ls1 = LazySingleton();
  print('Lazy Singleton: ${ls1.hashCode}');

  final ls2 = LazySingleton();
  print('Lazy Singleton: ${ls2.hashCode}');
}

class Singleton {
  Singleton._();

  static final _instance = Singleton._();

  factory Singleton() {
    return _instance;
  }
}

class LazySingleton {
  LazySingleton._();

  static LazySingleton _instance;

  factory LazySingleton() {
    if (_instance == null) {
      _instance = LazySingleton._();
    }

    return _instance;
  }
}
```

# 2. [Factory Method (Virtual Constructor)](https://dartpad.dev/cd4b2a0772554d7947c0f6347a9bbf19?)
**Factory Method** is a creational design pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created.

**Reading Resources**
  - [Factory Method by Refactoring Guru](https://refactoring.guru/design-patterns/factory-method)
  - [Flutter Design Patterns: 10 — Factory Method by Mangirdas Kazlauskas](https://mkobuolys.medium.com/flutter-design-patterns-10-factory-method-c53ad11d863f)


```dart
void main() {
  Shape shape = ShapeFactory.buildShape(ShapeType.circle);
  shape.draw();

  shape = ShapeFactory.buildShape(ShapeType.square);
  shape.draw();
}

enum ShapeType { circle, square }

class ShapeFactory {
  static Shape buildShape(ShapeType shapeType) {
    switch (shapeType) {
      case ShapeType.circle:
        return Circle();
      case ShapeType.square:
        return Square();
      default:
        throw FormatException('This Shape not implemented');
    }
  }
}

abstract class Shape {
  void draw();
}

class Circle implements Shape {
  @override
  void draw() => print('drawing Circle');
}

class Square implements Shape {
  @override
  void draw() => print('drawing Square');
}
```

# [3. Abstract Factory (Kit)](https://dartpad.dev/d1db00700c73c8c95eae2bd40ee0589e?)
**Abstract Factory** is a creational design pattern that lets you produce families of related objects without specifying their concrete classes.

**Reading Resources**
  - [Abstract Factory by Refactoring Guru](https://refactoring.guru/design-patterns/abstract-factory)
  - [Flutter Design Patterns: 11 — Abstract Factory by Mangirdas Kazlauskas](https://medium.com/flutter-community/flutter-design-patterns-11-abstract-factory-7098112925d8)

```dart
void main() {
  final materialFactory = MaterialFactory();
  final materialButton = materialFactory.makeButton();
  final materialCheckBox = materialFactory.makeCheckBox();

  materialButton.onTap();
  materialCheckBox.onTap();
  materialCheckBox.onTap();

  print('\n');

  final cupertionFactory = CupertinoFactory();
  final cupertionButton = cupertionFactory.makeButton();
  final cupertionCheckBox = cupertionFactory.makeCheckBox();

  cupertionButton.onTap();
  cupertionCheckBox.onTap();
  cupertionCheckBox.onTap();
}

/// Material Factory for Material Widgets
class MaterialFactory implements WidgetsFactory {
  @override
  Button makeButton() => MaterialButton();

  @override
  CheckBox makeCheckBox() => MaterialCheckBox();
}

/// Cupertino Factory for Cupertino Widgets
class CupertinoFactory implements WidgetsFactory {
  @override
  Button makeButton() => CupertinoButton();

  @override
  CheckBox makeCheckBox() => CupertinoCheckBox();
}

abstract class WidgetsFactory {
  Button makeButton();

  CheckBox makeCheckBox();
}

/// For Button
abstract class Button {
  String get title;

  void onTap() => print('$title tapped!');
}

class MaterialButton extends Button {
  @override
  String get title => 'Material Button';
}

class CupertinoButton extends Button {
  @override
  String get title => 'Cupertino Button';
}

///For CheckBox
abstract class CheckBox {
  String get title;

  bool checked = false;

  void onTap() {
    checked = !checked;
    print('$title checked: $checked');
  }
}

class MaterialCheckBox extends CheckBox {
  @override
  String get title => 'Material CheckBox';
}

class CupertinoCheckBox extends CheckBox {
  @override
  String get title => 'Cupertino CheckBox';
}
```

# [4. Prototype (Clone)](https://dartpad.dev/374b2282611c21415e94513def6d8995?)
**Prototype** is a creational design pattern that lets you copy existing objects without making your code dependent on their classes.

**Reading Resources**
  - [Prototype by Refactoring Guru](https://refactoring.guru/design-patterns/prototype)
  - [Flutter Design Patterns: 14 — Prototype by Mangirdas Kazlauskas](https://medium.com/flutter-community/flutter-design-patterns-14-prototype-7d7d18bcf643)

```dart
void main() {
  final c1 = Circle('black', 10);
  final c2 = c1.clone() as Circle;

  print('c2 radius: ${c2.radius}');
  print('c2 color: ${c2.color}');
  
  print('\n');

  final r1 = Rectangle('red', 10, 15);
  final r2 = r1.clone() as Rectangle;

  print('r2 color: ${r2.color}');
  print('r2 height: ${r2.height}');
  print('r2 width: ${r2.width}');
}

abstract class Shape {
  String color;

  Shape(this.color);

  Shape.clone(Shape source) {
    color = source.color;
  }

  Shape clone();
}

class Circle extends Shape {
  double radius;

  Circle(String color, this.radius) : super(color);

  /// returns new object
  Circle.clone(Circle source) : super.clone(source) {
    radius = source.radius;
  }

  @override
  clone() => Circle.clone(this);
}

class Rectangle extends Shape {
  double height;
  double width;

  Rectangle(String color, this.height, this.width) : super(color);

  /// returns new object
  Rectangle.clone(Rectangle source) : super.clone(source) {
    height = source.height;
    width = source.width;
  }

  @override
  clone() => Rectangle.clone(this);
}
```

# [5. Builder](https://dartpad.dev/3fcdd83b843dcf614b378875897e13f1?)
**Builder** is a creational design pattern that lets you construct complex objects step by step. The pattern allows you to produce different types and representations of an object using the same construction code.


**Reading Resources**
  - [Builder by Refactoring Guru](https://refactoring.guru/design-patterns/builder)
  - [Flutter Design Patterns: 18 — Builder by Mangirdas Kazlauskas](https://medium.com/flutter-community/flutter-design-patterns-18-builder-cdc90b222724)

```dart
void main() {
  final burgerBuilder1 = SmallBurgerBuilder();
  final burgerBuilder2 = BigBurgerBuilder();

  final burgerMaker = BurgerMaker(burgerBuilder1);
  burgerMaker.prepareBurger();

  final burger1 = burgerMaker.getBurger();

  print('Burger 1');
  burger1.printFormattedIngredients();
  print('price: ${burger1.getFormattedPrice()}');
  

  print('\n');
  
  
  burgerMaker.changeBurgerBuilder(burgerBuilder2);
  burgerMaker.prepareBurger();

  final burger2 = burgerMaker.getBurger();
  
  print('Burger 2');
  burger2.printFormattedIngredients();
  print('price: ${burger2.getFormattedPrice()}');
}

// Burger Maker
class BurgerMaker {
  BurgerMaker(this.burgerBuilder);

  IBurgerBuilder burgerBuilder;

  void changeBurgerBuilder(IBurgerBuilder burgerBuilder) {
    this.burgerBuilder = burgerBuilder;
  }

  Burger getBurger() => burgerBuilder.getBurger();

  void prepareBurger() {
    burgerBuilder.createBurger();
    burgerBuilder.setBurgerPrice();

    burgerBuilder.addBeef();
    burgerBuilder.addMayonnaise();
    burgerBuilder.addOnion();
    burgerBuilder.addCheese();
    burgerBuilder.addKetchup();
  }
}

/// Concrete Builders
class BigBurgerBuilder extends IBurgerBuilder {
  BigBurgerBuilder() {
    price = 4.99;
  }

  void addCheese() => burger.addIngredient(Cheese());

  void addBeef() => burger.addIngredient(Beef());

  void addKetchup() => burger.addIngredient(Ketchup());

  void addMayonnaise() => burger.addIngredient(Mayonnaise());

  void addOnion() => burger.addIngredient(Onion());
}

class SmallBurgerBuilder extends IBurgerBuilder {
  SmallBurgerBuilder() {
    price = 2.99;
  }

  void addCheese() => burger.addIngredient(Cheese());

  void addBeef() => burger.addIngredient(Beef());

  void addKetchup() {
    // Not needed
  }

  void addMayonnaise() => burger.addIngredient(Mayonnaise());

  void addOnion() => burger.addIngredient(Onion());
}

/// Burger Builder Base
abstract class IBurgerBuilder {
  Burger burger;
  double price;

  void createBurger() {
    burger = Burger();
  }

  Burger getBurger() => burger;

  void setBurgerPrice() => burger.setPrice(price);

  void addCheese();
  void addBeef();
  void addKetchup();
  void addMayonnaise();
  void addOnion();
}

/// Burger
class Burger {
  final List<Ingredient> _ingredients = [];
  double _price;

  void addIngredient(Ingredient ingredient) => _ingredients.add(ingredient);

  void printFormattedIngredients() =>
      print(_ingredients.map((i) => i.name).join(','));

  void setPrice(double price) {
    _price = price;
  }

  String getFormattedPrice() => '\$${_price.toStringAsFixed(2)}';
}

/// Ingredients
abstract class Ingredient {
  final String name;

  Ingredient(this.name);
}

class Cheese extends Ingredient {
  Cheese() : super('Cheese');
}

class Beef extends Ingredient {
  Beef() : super('Beef');
}

class Ketchup extends Ingredient {
  Ketchup() : super('Ketchup');
}

class Mayonnaise extends Ingredient {
  Mayonnaise() : super('Mayonnaise');
}

class Onion extends Ingredient {
  Onion() : super('Onion');
}
```


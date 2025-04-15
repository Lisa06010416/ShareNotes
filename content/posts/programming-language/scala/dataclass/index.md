---
title: "Dataclass in Java/Scala/Python"
date: 2024-12-28
description: "Dataclass in Java/Scala/Python"
draft: True
categories: ["Engineering", "Programming language"]
tags: ["Java", "Scala", "Python"]
author: "Lisa Lin"

showToc: true
TocOpen: false
draft: true
hidemeta: false
comments: false
description: ""
canonicalURL: "https://lisa06010416.github.io/ShareNotes/posts/programming-language/scala/scala-for-data-scientists/"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "<image path/url>" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
editPost:
    URL: "https://github.com/Lisa06010416/HugoEngineer/tree/master/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---



# Dataclass in Python

Before Python 3.7, we need to use `__init__` to initialize the class.
```python
class Article:
    def __init__(self, title:str, clicks:int, views:int):
        self.title = title
        self.clicks = clicks
        self.views = views
        self.CTR = float(clicks) / float(views)
        
```

After Python 3.7, we can use `dataclass` to initialize the class. It automatically generates special methods like __init__(), __repr__(), __eq__(), and others based on class attributes.
```python
from dataclasses import dataclass

@dataclass
class Article:
    title: str
    clicks: 0 # default value
    views: int
    CTR: float
    TAG: list[str] = field(default_factory=list) # Using default_factory=list ensures that each instance gets a new list instead of sharing the same list.

    def __post_init__(self): # Define additional initialization logic using __post_init__()
        self.CTR = float(self.clicks) / float(self.views)

    def count_tag(self): # Dataclasses can also include regular methods
        return len(self.TAG)
```


How to make the class immutable?
@dataclass(frozen=True)

Compare two objects
```
@dataclass(order=True)
class Product:
    price: float
    name: str

p1 = Product(20.5, "Apple")
p2 = Product(15.0, "Banana")

print(p1 > p2)  # True, since 20.5 > 15.0
```


Inferent
```
@dataclass
class Animal:
    species: str

@dataclass
class Dog(Animal):
    breed: str
    age: int

dog = Dog(species="Canine", breed="Labrador", age=5)
print(dog)  # Dog(species='Canine', breed='Labrador', age=5)
```


| Parameter      | Default | Description |
|---------------|---------|-------------|
| `init`        | `True`  | Generates the `__init__` method automatically. |
| `repr`        | `True`  | Includes fields in the string representation (`__repr__`). |
| `eq`          | `True`  | Generates the equality method (`__eq__`). |
| `order`       | `False` | Enables comparison methods (`<`, `>`, `<=`, `>=`). |
| `unsafe_hash` | `False` | Generates a `__hash__` method (useful for hashable objects). |
| `frozen`      | `False` | Makes the dataclass immutable (prevents modification after creation). |


default 與 default_factory 兩個參數只能擇一使用，不能同時在 field() 出現
| Parameter          | Default           | Description |
|-------------------|------------------|-------------|
| `default`        | `MISSING`         | Specifies a default value for the field. |
| `default_factory` | `MISSING`         | A factory function to generate default values (useful for mutable types like lists). |
| `init`           | `True`            | If `False`, the field is excluded from the `__init__` method. |
| `repr`           | `True`            | If `False`, the field is excluded from `__repr__`. |
| `compare`        | `True`            | If `False`, the field is excluded from comparison methods. |
| `hash`           | `None`            | Controls whether the field is included in `__hash__` (auto-determined if `None`). |
| `metadata`       | `{}`              | Stores additional metadata for the field (e.g., annotations, validation info). |


dict 是可以在程式執行階段（runtime）增減內容元素的，所以需要的記憶體自然較高。《Python 神乎其技》 書中建議，如果你不需要在程式執行階段增減物件屬性，就適合使用 slots，它會事先定義物件屬性需要多少記憶體空間，執行時就可以讓佔用的記憶體「瘦身」、並且加快存取速度。通常 slots 的存取所需時間會比 dict 減少至少 20%！
```
@dataclass()
class T:
    # slots 內列出物件要擁有的屬性
    __slots__ = ['a', 'b', 'c']
    a: int
    b: int
    c: int
 
t1 = T(1, 2, 3)
print(t1.__slots__)
print(t1.b) # 取得資料的方法依舊
 
## exectitive result：
## $ python3 test.py
## ['a', 'b', 'c']
## 2
```

# Dataclass in Java

1. Enum
2. Class with Lombok (/ˈlɒm.bɒk/）)




```import lombok.Data;
import lombok.Builder;

@Data
@Builder
public class User {
    private String name;
    private int age;
}
```


Lombok annotation:
| Annotation               | Functionality                                              |
|--------------------------|----------------------------------------------------------|
| `@Getter`               | Automatically generates getter methods                   |
| `@Setter`               | Automatically generates setter methods                   |
| `@ToString`             | Automatically generates `toString()`                     |
| `@EqualsAndHashCode`    | Automatically generates `equals()` and `hashCode()`      |
| `@Data`                 | Combines `@Getter`, `@Setter`, `@ToString`, `@EqualsAndHashCode`, and `@RequiredArgsConstructor` |
| `@NoArgsConstructor`     | Generates a no-argument constructor                     |
| `@AllArgsConstructor`    | Generates a constructor with all fields                 |
| `@RequiredArgsConstructor` | Generates a constructor only for `final` or `@NonNull` fields |
| `@Value`                | Makes the class immutable (similar to `@Data`, but all fields are `final`) |
| `@Builder`              | Supports the builder pattern                            |
| `@Log`                  | Quickly generates a Logger object                       |


✅ 適合使用 enum 的場景
固定常數集合（如狀態、類型、設定值）。
需要 switch 語句處理時（switch(status)）。
不希望有額外的實例被創建（enum 具有內建的單例特性）。
希望使用 values() 方法來遍歷所有值。
🔹 適用範例

```java
複製
編輯
public enum PaymentMethod {
    CASH, CREDIT_CARD, PAYPAL;
}
```

✅ 適合使用 Lombok 的場景
普通 Java 類別 需要減少樣板程式碼（如 DTO、VO、Entity）。
物件是可變的（mutable），需要 @Getter 和 @Setter。
希望使用建構子模式（Builder），可以用 @Builder。
🔹 適用範例

```java
複製
編輯
import lombok.Data;
import lombok.Builder;

@Data
@Builder
public class User {
    private String name;
    private int age;
}
```

# Dataclass in Scala
* case class 在 Scala 中是一种特殊的类，默认具备数据类的特性，比如：

  自动生成 toString 方法
  自动生成 equals 和 hashCode 方法
  支持解构（模式匹配）
  默认是不可变（val 修饰）
  提供 copy 方法

* 在 Scala 的推荐做法 中，我们通常保持 case class 不可变，并使用 copy 方法创建新实例
  ```scala
  case class User(name: String, age: Int)
  val user1 = User("Alice", 30)
  val user2 = user1.copy(age = 31)
  ```


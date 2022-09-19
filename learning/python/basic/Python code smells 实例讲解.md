# Python code smells 实例讲解

<a id="profileBt"></a><a id="js_name"></a>Python编程时光 *2022-03-09 09:02*

<a id="js_article-tag-card__left"></a>收录于话题 #Python 案例分享 <a id="js_article-tag-card__right"></a>4个

**code smells** 可以理解为代码中让人感觉到不舒服的地方。可能是代码规范问题，也可能是设计上的缺陷。

很多时候一段代码符合基本逻辑，能够正常运行，并不代表它是不“丑”的。代码中可能会存在诸如可读性差、结构混乱、重复代码太多、不够健壮等问题。

#### 示例代码

```
`"""
Very advanced Employee management system.
"""
from dataclasses import dataclass
from typing import List
# The fixed number of vacation days that can be paid out.
FIXED_VACATION_DAYS_PAYOUT = 5
@dataclass
class Employee:
    """Basic representation of an employee at the company"""
    name: str
    role: str
    vacation_days: int = 25
    def take_a_holiday(self, payout: bool) -> None:
        """Let the employee take a single holiday, or pay out 5 holidays."""
        if payout:
            if self.vacation_days < FIXED_VACATION_DAYS_PAYOUT:
                raise ValueError(
                    f"You don't have enough holidays left over for a payout. \
                            Remaining holidays: {self.vacation_days}"
                )
            try:
                self.vacation_days -= FIXED_VACATION_DAYS_PAYOUT
                print(
                    f"Paying out a holiday. Holidays left: {self.vacation_days}")
            except Exception:
                pass
        else:
            if self.vacation_days < 1:
                raise ValueError(
                    "You don't have any holidays left. Now back to work, you!"
                )
            self.vacation_days -= 1
            print("Have fun on your holiday. Don't forget to check your emails!")
@dataclass
class HourlyEmployee(Employee):
    """ Employee that's paid based on number of worked hours."""
    hourly_rate: float = 50
    amount: int = 10
@dataclass
class SalariedEmployee(Employee):
    """Employee that's paid based on a fixed monthly salary."""
    monthly_salary: float = 5000
class Company:
    """Represents a company with employees."""
    def __init__(self) -> None:
        self.employees: List[Employee] = []
    def add_employee(self, employee: Employee) -> None:
        self.employees.append(employee)
    def find_managers(self) -> List[Employee]:
        managers = []
        for employee in self.employees:
            if employee.role == "manager":
                managers.append(employee)
        return managers
    def find_vice_presidents(self) -> List[Employee]:
        vice_presidents = []
        for employee in self.employees:
            if employee.role == "president":
                vice_presidents.append(employee)
        return vice_presidents
    def find_interns(self) -> List[Employee]:
        interns = []
        for employee in self.employees:
            if employee.role == "intern":
                interns.append(employee)
        return interns
    def pay_employee(self, employee: Employee) -> None:
        if isinstance(employee, SalariedEmployee):
            print(
                f"Paying employee {employee.name} a monthly salary of ${employee.monthly_salary}"
            )
        elif isinstance(employee, HourlyEmployee):
            print(
                f"Paying employee {employee.name} a hourly rate of \
                            ${employee.hourly_rate} for {employee.amount} hours."
            )
def main() -> None:
    company = Company()
    company.add_employee(SalariedEmployee(name="Louis", role="manager"))
    company.add_employee(HourlyEmployee(name="Brenda", role="president"))
    company.add_employee(HourlyEmployee(name="Tim", role="intern"))
    print(company.find_vice_presidents())
    print(company.find_managers())
    print(company.find_interns())
    company.pay_employee(company.employees[0])
    company.employees[0].take_a_holiday(False)
if __name__ == '__main__':
    main()
`
```

上述代码实现了一个简单的“员工管理系统”。

- Employee 类代表公司里的员工，有姓名、角色、假期等属性。可以请假（`take_a_holiday`），或者单独请一天，或者以 5 天为单位将假期兑换为报酬
    
- HourlyEmployee 和 MonthlyEmployee 分别代表以时薪或者月薪来计算工资的员工
    
- Company 类代表公司，可以招收员工（`add_employee`）、返回特定角色的员工列表（如 `find_managers`）、发放薪资等（`pay_employee`）
    

#### code smells

上面的代码中存在着很多可以改进的地方。

##### 用 Enum 类型替代 str 作为员工的 role 属性

上面的 `Employee` 类使用了 `str` 类型来存储 `role` 属性的值，比如用 `"manager"` 代表经理，用 `"intern"` 代表实习生。

```
`company.add_employee(SalariedEmployee(name="Louis", role="manager"))
company.add_employee(HourlyEmployee(name="Brenda", role="president"))
company.add_employee(HourlyEmployee(name="Tim", role="intern"))
`
```

实际上 String 过于灵活，可以拥有任何含义，用来表示角色属性时不具有足够清晰的指向性。不同的拼写规则和大小写习惯都会导致出现错误的指向，比如 `"Manager"` 和 `"manager"`，`"vice-president"` 和 `"vice_president"`。可以使用 Enum 替代 str。

```
`from enum import Enum, auto
class Role(Enum):
    """Employee roles."""
    PRESIDENT = auto()
    VICEPRESIDENT = auto()
    MANAGER = auto()
    LEAD = auto()
    WORKER = auto()
    INTERN = auto()
`
```

修改 `Employee` 类中 `role` 属性的定义：

```
`@dataclass
class Employee:
    name: str
    role: Role
    vacation_days: int = 25
`
```

`Company` 类中 `find_managers` 等方法也做相应的修改：

```
`def find_managers(self) -> List[Employee]:
    managers = []
    for employee in self.employees:
        if employee.role == Role.MANAGER:
            managers.append(employee)
    return managers
`
```

`main` 方法中使用新的 role 创建员工对象：

```
`company.add_employee(SalariedEmployee(name="Louis", role=Role.MANAGER))
company.add_employee(HourlyEmployee(name="Brenda", role=Role.VICEPRESIDENT))
company.add_employee(HourlyEmployee(name="Tim", role=Role.INTERN))
`
```

##### 消除重复代码

`Company` 类中有一个功能是返回特定角色的员工列表，即 `find_managers`、`find_vice_presidents`、`find_interns` 三个方法。

这三个方法实际上有着同样的逻辑，却分散在了三个不同的函数里。可以合并成一个方法来消除重复代码。

```
`def find_employees(self, role: Role) -> List[Employee]:
    """Find all employees with a particular role."""
    employees = []
    for employee in self.employees:
        if employee.role == role:
            employees.append(employee)
    return employees
`
```

同时将 `main` 函数中的 `find_managers`、`find_vice_presidents`、`find_interns` 都改为如下形式：

```
`print(company.find_employees(Role.VICEPRESIDENT))
print(company.find_employees(Role.MANAGER))
print(company.find_employees(Role.INTERN))
`
```

##### 尽量使用内置函数

上面版本中的 `find_employees` 方法，包含了一个 `for` 循环。实际上该部分逻辑可以使用 Python 内置的**列表推导**来实现。

合理的使用 Python 内置函数可以使代码更短、更直观，同时内置函数针对很多场景在性能上也做了一定的优化。

```
`def find_employees(self, role: Role) -> List[Employee]:    
    """Find all employees with a particular role."""    
    
    return [employee for employee in self.employees if employee.role is role]
`
```

##### 更清晰明确的变量名

旧版本：

```
`@dataclass
class HourlyEmployee(Employee):   
    """ Employee that's paid based on number of worked hours."""   
    
    hourly_rate: float = 50   
    amount: int = 10
`
```

新版本：

```
`@dataclass
class HourlyEmployee(Employee):   
    """ Employee that's paid based on number of worked hours."""  
    
    hourly_rate_dollars: float = 50  
    hours_worked: int = 10
`
```

##### isinstance

当你在代码的任何地方看到 `isinstance` 这个函数时，都需要特别地加以关注。它意味着代码中有可能存在某些有待提升的设计。

比如代码中的 `pay_employee` 函数：

```
`def pay_employee(self, employee: Employee) -> None:   
    if isinstance(employee, SalariedEmployee):     
       print(       
           f"Paying employee {employee.name} a monthly salary of ${employee.monthly_salary}"       
       )  
   elif isinstance(employee, HourlyEmployee):     
       print(     
           f"Paying employee {employee.name} a hourly rate of \                     
                       ${employee.hourly_rate_dollars} for {employee.hours_worked} hours."      
        )
`
```

这里 `isinstance` 的使用，实际上在 `pay_employee` 函数中引入了对 `Employee` 的子类的依赖。这种依赖导致各部分代码之间的职责划分不够清晰，耦合性变强。

`pay_employee` 方法需要与 `Employee` 的子类的具体实现保持同步。每新增一个新的员工类型（`Employee` 的子类），此方法中的 `if-else` 也就必须再新增一个分支。即需要同时改动不同位置的两部分代码。

可以将 `pay_employee` 的实现从 `Company` 类转移到具体的 `Employee` 子类中。即特定类型的员工拥有对应的报酬支付方法，公司在发薪时只需要调用对应员工的 `pay` 方法，无需实现自己的`pay_employee` 方法。由 `isinstance` 引入的依赖关系从而被移除。

```
`@dataclass
class HourlyEmployee(Employee):   
    """ Employee that's paid based on number of worked hours."""   
    
    hourly_rate_dollars: float = 50  
    hours_worked: int = 10  
    
    def pay(self):   
        print(        
            f"Paying employee {self.name} a hourly rate of \                   
                    ${self.hourly_rate_dollars} for {self.hours_worked} hours."       
         )
         
@dataclass
class SalariedEmployee(Employee):  
    """Employee that's paid based on a fixed monthly salary."""   
    
    monthly_salary: float = 5000  
    
    def pay(self):   
        print(        
            f"Paying employee {self.name} a monthly salary of ${self.monthly_salary}"     
        )
`
```

再把 `main` 函数中的 `company.pay_employee(company.employees[0])` 改为 `company.employees[0].pay()`。

由于每一个特定的 `Employee` 子类都需要实现 `pay` 方法，更好的方式是将 `Employee` 实现为虚拟基类，`pay` 成为子类必须实现的虚拟方法。

```
`from abc import ABC, abstractmethod
class Employee(ABC):  
     @abstractmethod   
     def pay() -> None:     
         """Method to call when paying an employee"""
`
```

##### Bool flag

`Employee` 类中的 `take_a_holiday` 方法有一个名为 `payout` 的参数。它是布尔类型，作为一个开关，来决定某个员工是请一天假，还是以 5 天为单位将假期兑换为报酬。

这个开关实际上导致了 `take_a_holiday` 方法包含了两种不同的职责，只通过一个布尔值来决定具体执行哪一个。

**函数原本的目的就是职责的分离**。使得同一个代码块中不会包含过多不同类型的任务。

因此 `take_a_holiday` 方法最好分割成两个不同的方法，分别应对不同的休假方式。

```
`class Employee(ABC):   
    def take_a_holiday(self) -> None:   
        """Let the employee take a single holiday."""   
        
        if self.vacation_days < 1:       
            raise ValueError(         
                "You don't have any holidays left. Now back to work, you!"         
            )      
        self.vacation_days -= 1    
        print("Have fun on your holiday. Don't forget to check your emails!")   
    def payout_a_holiday(self) -> None:    
        """Let the employee get paid for unused holidays."""       
        
        if self.vacation_days < FIXED_VACATION_DAYS_PAYOUT:          
           raise ValueError(             
               f"You don't have enough holidays left over for a payout. \                     
                       Remaining holidays: {self.vacation_days}"          
            )    
        try:         
            self.vacation_days -= FIXED_VACATION_DAYS_PAYOUT           
            print(          
                f"Paying out a holiday. Holidays left: {self.vacation_days}")      
         except Exception:      
             pass
`
```

##### Exceptions

`payout_a_holiday` 方法中有一步 `try-except` 代码。但该部分代码实际上对 Exception 没有做任何事。对于 Exception 而言：

**如果需要 catch Exception，就 catch 特定类型的某个 Exception，并对其进行处理；如果不会对该 Exception 做任何处理，就不要 catch 它**。

在此处使用 `try-except` 会阻止异常向外抛出，导致外部代码在调用 `payout_a_holiday` 时获取不到异常信息。此外，使用 `Exception` 而不是某个特定类型的异常，会导致所有的异常信息都被屏蔽掉，包括语法错误、键盘中断等。

因此，去掉上述代码中的 `try-except`。

##### 使用自定义 Exception 替代 ValueError

`ValueError` 是 Python 内置的在内部出现值错误时抛出的异常，并不适合用在自定义的场景中。最好在代码中定义自己的异常类型。

```
`class VacationDaysShortageError(Exception):  
    """Custom error that is raised when not enough vacation days available."""   
    
    def __init__(self, requested_days: int, remaining_days: int, message: str) -> None:     
        self.requested_days = requested_days   
        self.remaining_days = remaining_days    
        self.message = message    
        super().__init__(message)
`
```

```
`def payout_a_holiday(self) -> None: 
    """Let the employee get paid for unused holidays."""  
    
    if self.vacation_days < FIXED_VACATION_DAYS_PAYOUT:      
        raise VacationDaysShortageError(    
            requested_days=FIXED_VACATION_DAYS_PAYOUT,   
            remaining_days=self.vacation_days,     
            message="You don't have enough holidays left over for a payout.")  
    self.vacation_days -= FIXED_VACATION_DAYS_PAYOUT  
    print(f"Paying out a holiday. Holidays left: {self.vacation_days}")
`
```

#### 最终版本

```
`"""
Very advanced Employee management system.
"""
from dataclasses import dataclass
from typing import List
from enum import Enum, auto
from abc import ABC, abstractmethod
# The fixed number of vacation days that can be paid out.
FIXED_VACATION_DAYS_PAYOUT = 5
class VacationDaysShortageError(Exception):
    """Custom error that is raised when not enough vacation days available."""
    def __init__(self, requested_days: int, remaining_days: int, message: str) -> None:
        self.requested_days = requested_days
        self.remaining_days = remaining_days
        self.message = message
        super().__init__(message)
class Role(Enum):
    """Employee roles."""
    PRESIDENT = auto()
    VICEPRESIDENT = auto()
    MANAGER = auto()
    LEAD = auto()
    WORKER = auto()
    INTERN = auto()
@dataclass
class Employee(ABC):
    """Basic representation of an employee at the company"""
    name: str
    role: Role
    vacation_days: int = 25
    def take_a_holiday(self) -> None:
        """Let the employee take a single holiday."""
        if self.vacation_days < 1:
            raise VacationDaysShortageError(
                requested_days=1,
                remaining_days=self.vacation_days,
                message="You don't have any holidays left. Now back to work, you!")
        self.vacation_days -= 1
        print("Have fun on your holiday. Don't forget to check your emails!")
    def payout_a_holiday(self) -> None:
        """Let the employee get paid for unused holidays."""
        if self.vacation_days < FIXED_VACATION_DAYS_PAYOUT:
            raise VacationDaysShortageError(
                requested_days=FIXED_VACATION_DAYS_PAYOUT,
                remaining_days=self.vacation_days,
                message="You don't have enough holidays left over for a payout."
            )
        self.vacation_days -= FIXED_VACATION_DAYS_PAYOUT
        print(f"Paying out a holiday. Holidays left: {self.vacation_days}")
    @abstractmethod
    def pay() -> None:
        """Method to call when paying an employee"""
@dataclass
class HourlyEmployee(Employee):
    """ Employee that's paid based on number of worked hours."""
    hourly_rate_dollars: float = 50
    hours_worked: int = 10
    def pay(self):
        print(
            f"Paying employee {self.name} a hourly rate of \
                    ${self.hourly_rate_dollars} for {self.hours_worked} hours."
        )
@dataclass
class SalariedEmployee(Employee):
    """Employee that's paid based on a fixed monthly salary."""
    monthly_salary: float = 5000
    def pay(self):
        print(
            f"Paying employee {self.name} a monthly salary of ${self.monthly_salary}"
        )
class Company:
    """Represents a company with employees."""
    def __init__(self) -> None:
        self.employees: List[Employee] = []
    def add_employee(self, employee: Employee) -> None:
        self.employees.append(employee)
    def find_employees(self, role: Role) -> List[Employee]:
        """Find all employees with a particular role."""
        return [employee for employee in self.employees if employee.role is role]
def main() -> None:
    company = Company()
    company.add_employee(SalariedEmployee(name="Louis", role=Role.MANAGER))
    company.add_employee(HourlyEmployee(
        name="Brenda", role=Role.VICEPRESIDENT))
    company.add_employee(HourlyEmployee(name="Tim", role=Role.INTERN))
    print(company.find_employees(Role.VICEPRESIDENT))
    print(company.find_employees(Role.MANAGER))
    print(company.find_employees(Role.INTERN))
    company.employees[0].pay()
    company.employees[0].take_a_holiday()
if __name__ == '__main__':
    main()
`
```

#### 参考资料

7 Python Code Smells: Olfactory Offenses To Avoid At All Costs

> 来源：
> 
> https://www.starky.ltd/2021/12/06/7-python-code-smells-by-practical-example/

<img width="247" height="19" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_106952c4695e47a08.jpg"/>

[<img width="677" height="119" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__d8219e6007624b08a.png"/>](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzkzMjMxMTg2NQ==&action=getalbum&album_id=2225391633284562944&scene=173&subscene=90&sessionid=1642653201&enterid=1642653281&from_msgid=2247483819&from_itemidx=1&count=3&nolastread=1#wechat_redirect)

[<img width="677" height="117" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_47332f17f74b4db6b.jpg"/>](http://mp.weixin.qq.com/s?__biz=MzIzMzMzOTI3Nw==&mid=2247505072&idx=1&sn=3605fcc95b6c0c7b0b9ec5dd15480231&chksm=e885b452dff23d44649944d41a190d55955a9f8ea6987e1ed73363c10bf3711528f4f8524e8a&scene=21#wechat_redirect)

[<img width="677" height="121" src="../../../_resources/640_wx_fmt_png_wxfrom_5_wx_lazy__2e81bae5b47b40988.png"/>](http://mp.weixin.qq.com/s?__biz=MzIzMzMzOTI3Nw==&mid=2247505084&idx=1&sn=d3bc3a37cda2759c1a078ce9811fdec2&chksm=e885b45edff23d48744d42d9f6658cdddd2164af7042544815851a851dc710c70a1527a9b686&scene=21#wechat_redirect)

[<img width="677" height="115" src="../../../_resources/640_wx_fmt_jpeg_wxfrom_5_wx_lazy_d1b58c4232184c6f9.jpg"/>](http://mp.weixin.qq.com/s?__biz=MzIzMzMzOTI3Nw==&mid=2247505065&idx=1&sn=a8b98840693b8d57bc5422da6d68a25d&chksm=e885b44bdff23d5d93cd686803d091d4b280c6f8a33afcf9e38c3f92746a1c222d1b40a8ad9a&scene=21#wechat_redirect)

People who liked this content also liked

有了这篇 Docker 网络原理，彻底爱了~

高效运维

不看的原因

- 内容质量低
- 不看此公众号

前端竟然用Golang 动态生成图片？

前端技术江湖

不看的原因

- 内容质量低
- 不看此公众号

C函数指针别再停留在语法，得上升到软件设计~

嵌入式资讯精选

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___cfb8748e139f44969.bmp"/>

Scan to Follow
# Readability Counts

"Any fool can write code that a computer can understand. Good programmers write code that humans can understand." - [Martin Fowler](https://martinfowler.com/)

I have a friend who once told me that it's our jobs to write code that can be maintained. When you open up someone else's code, or even your own from a while back, the clock immediately starts ticking. The more time you have to think about what code is doing or what variable `p` stands for, the more time you're wasting and the more money your employer is spending.

Our jobs as engineers is to solve problems quickly, efficiently, and proficiently. Code is meant to be proficient yes, but so are we at our jobs. It's a lot simpler to declare explicit variables and have descriptive method names, than to use arbitrary names or abbreviations, or to solely rely on annotations to explain your intension. `p.first` isn't expressive, whereas `person.first_name` is immediately readable and understandable.

There's a fun game to play called [Code Golf](https://en.wikipedia.org/wiki/Code_golf). The point of the game is to refactor code making it shorter and smaller until you reach the least amount of characters possible to still produce the same outcome as before. While fun to play, it also demonstrates that there's a certain point that you reach where you start to sacrifice readability for performance while increasing cognitive load.

Being mindful of the amount of cognitive load your code adds to yourself or a colleague is one of the most important aspects of your job. There's a great quote that says: "Always code as if the guy who ends up maintaining your code will be a violent psychopath who knows where you live." While not the most cheerful saying, it's always good to be mindful of how your code will read to the next person, or to you in 6 months when you've moved on to another project.


---

> Author: <no value>  
> URL: http://localhost:1313/2018/09/18/readability-counts/  


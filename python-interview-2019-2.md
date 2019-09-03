## Python开发工程师笔试题

> 答题要求：将该项目从[地址1](<https://github.com/jackfrued/python-interview-2019>)或[地址2](<https://gitee.com/jackfrued/python-interview-2019>)fork到自己的[github]()或[gitee]()仓库并填写答案，完成之后将自己的仓库（public）地址发给面试官即可，答题时间120分钟。

1. 下面的Python代码会输出什么。

   ```Python
   print([(x, y) for x, y in zip('abcd', (1, 2, 3, 4, 5))])
   print({x: f'item{x ** 2}' for x in range(5) if x % 2})
   print(len({x for x in 'hello world' if x not in 'abcdefg'}))
   ```

   答案：

   ```
   1、
   2、{1:'item1', 3:'item9'}
   3、6
   ```

2. 下面的Python代码会输出什么。

   ```Python
   from functools import reduce
   
   items = [11, 12, 13, 14] 
   print(reduce(int.__mul__, map(lambda x: x // 2, filter(lambda x: x ** 2 > 150, items))))
   ```

   答案：

   ```
   42
   ```

3. 有一个通过网络获取数据的Python函数（可能会因为网络或其他原因出现异常），写一个装饰器让这个函数在出现异常时可以重新执行，但尝试重新执行的次数不得超过指定的最大次数。

   答案：

   ```Python
   def retry(max_try):
      def outer(func):
         @wraps(func)
         def wrapper(*args, **kwargs):
            for _ in range(max_try):
               try:
                  result = func(*args, **kwargs)
                  print('请求成功')
                  break
               except:
                  print('网络异常，正在重试')
            return func
         return wrapper
      return outer
   ```

4. 下面的字典中保存了某些公司今日的股票代码及价格，用一句Python代码从中找出价格最高的股票对应的股票代码，用一句Python代码创建股票价格大于100的股票组成的新字典。

   > 说明：美股的股票代码是指英文字母代码，如：AAPL、GOOG。

   ```Python
   prices = {
       'AAPL': 191.88,
       'GOOG': 1186.96,
       'IBM': 149.24,
       'ORCL': 48.44,
       'ACN': 166.89,
       'FB': 208.09,
       'SYMC': 21.29
   }
   ```

   答案：

   ```Python
   from collections import Counter
   print(Counter(prices).most_common(1)[0])
   dict(filter(lambda item:item[1] > 100, Counter(prices).items())  # 第二问用了pycharm，算错
   ```

5. 用生成式实现矩阵的转置操作。例如，用`[[1, 2], [3, 4], [5, 6]`表示矩阵$\begin{bmatrix}1 & 2\\\\3 &4\\\\5 & 6\end{bmatrix}$，写一个生成式将其转换成`[[1, 3, 5], [2, 4, 6]]`即$\begin{bmatrix}1 & 3 & 5\\\\2 & 4 & 6\end{bmatrix}$。

   答案：

   ```Python
   matrix_a = [[1, 2], [3, 4], [5, 6]]
   [list(item) for item in zip(*matrix_a)]
   ```

6. 写一个函数，传入的参数是一个列表（列表中的元素可能也是一个列表），返回该列表最大的嵌套深度，例如：

   > 参数：`[1, 2, 3]`
   >
   > 返回：`1`
   >
   > 参数：`[[1], [2, [3]]]`
   >
   > 返回：`3`

   答案：

   ```Python
   # 之前自己做过
   # 方法一：时间复杂度高
   from copy import deepcopy
   
   
   def nestification1(array):
      list1 = deepcopy(array)
      len_max = 0
      while True:
         if list1:
            list_cycle = []
         else:
            break
         len_max += 1
         for item in list1:
            if isinstance(item, list):
               list_cycle += item
         list1 = deepcopy(list_cycle)
      return len_max
            
   # 方法二：转化成字符串再处理，时间复杂度低
   import re
   
   
   def nestification2(array):
      pattern1 = re.compile(r'[\[\]]')
      list2 = pattern1.findall(array)
      count = 0
      len_set = set()
      for item in list2:
         if item == '[':
            count += 1
         elif item == ']':
            count -= 1
         len_set.add(count)
      return max(len_set)
   ```

7. 写一个函数，实现将输入的长链接转换成短链接，每个长链接对应的短链接必须是独一无二的且每个长链接只应该对应到一个短链接，假设短链接统一以`http://t.cn/`开头。

   > 参数：`http://jackfrued.xyz/api/users/10001`
   >
   > 返回：`http://t.cn/E6MUth1`

   答案：

   ```Python
   
   ```

8. 用5个线程，将1~100的整数累加到一个初始值为0的变量上，每次累加时将线程ID和本次累加后的结果打印出来。

    答案：

    ```Python
    from threading import current_thread
    from threading import Thread


    def func(iter_num):
       while True:
           global init_value
           try:
               init_value += next(iter_num)
               print('线程ID:', current_thread())
               print('本次累加结果为:', init_value)
           except:
               break


    if __name__ == '__main__':
       init_value = 0
       iter_num = (_ for _ in range(1, 101))
       t_list = []
       for _ in range(5):
           thread = Thread(target=func, args=(iter_num, ))
           thread.start()
           t_list.append(thread)
       for item in t_list:
           item.join()
       print('计算完成')
       print('最终累加结果:', init_value)
    ```

9. 请阐述Python是如何进行内存管理的。

    答案：

    ```
    python在定义变量或方法时会在内存中开辟空间(已存在的变量则不会再次开辟)，将开辟的这部分的内存地址指向定义的变量并且这部分内存空间的引用计数会增加1，如果有新的变量也指向这部分内存空间则引用计数会相应继续增加；如果销毁这个变量或者对变量重新赋值时，因为之前的内存空间已经不指向之前的变量，所以引用计数也会相应减少，当引用计数减少至0时，这部分内存空间将会被释放。
    ```

10. 在MySQL数据库中有名为`tb_result`的表如下所示，请写出能查询出如下所示结果的SQL。

  `tb_result`表：

  | rq         | shengfu |
  | ---------- | ------- |
  | 2017-04-09 | 胜      |
  | 2017-04-09 | 胜      |
  | 2017-04-09 | 负      |
  | 2017-04-09 | 负      |
  | 2017-04-10 | 胜      |
  | 2017-04-10 | 负      |
  | 2017-04-10 | 负      |

  查询结果：

  | rq         | 胜   | 负   |
  | ---------- | ---- | ---- |
  | 2017-04-09 | 2    | 2    |
  | 2017-04-10 | 1    | 2    |

  答案：

  ```SQL
  # 纵表转横表
  select 
  from 
  ```

11. 列举出你知道的HTTP请求头选项并说明其作用。

    答案：

    ```
    'User-Agent': 表示发送请求的浏览器的相关信息；
    'Cookie': 储存在用户本地的一些数据，一般用于保存用户信息、认证、安全等方面；
    'HOST'：所请求的服务器域名
    'referer': 表示当前请求时从哪个url连接过来的
    ```

12. 阐述JSON Web Token的工作原理和优点。

    答案：

    ```
    
    ```

13. 请阐述访问一个用Django或Flask开发的Web应用，从用户在浏览器中输入网址回车到浏览器收到Web页面的整个过程中，到底发生了哪些事情，越详细越好。

    答案：

    ```
    以Django为例：
    浏览器向服务器发起请求，请求先经过wsgi(web服务网关接口)，然后再到django的中间件，依次执行每一个中间件中的process_request，如果遇到异常则会进入process_exception,之后再到路由分发，按照urls中配置的依次查找，找到第一个就不会继续查找，映射到views中对应的方法或类，views中根据业务逻辑与数据库交互(增删改查)，再从templates中获取页面并进行渲染(后端渲染)，然后返回时再次经过中间件(与请求时顺序相反)，依次经过process_response方法，然后再次经过wsgi，最后浏览器收到Web页面。
    
    ```

14. 请阐述HTTPS的工作原理，并说明该协议与HTTP之间的区别。

    答案：

    ```
    
    ```

15. 简述如何检查数据库是不是系统的性能瓶颈以及你在工作中是如何优化数据库操作性能的。

    答案：

    ```
    判断是否为性能瓶颈：
    可以利用第三方包，例如django中的django-debug-toolbar等，或配置日志文件，又或者配置mysql慢查询，找到系统耗时过久的原因，判断是否有大量的时间消耗在数据库的操作上。
    优化数据库性能：
    1、配置缓存数据库，缓解数据库压力(缓存无数据时进行缓存预热)；
    2、适当的建立索引，加快查询效率；
    3、根据实际业务需求优化数据库结构，适当建立冗余字段，减少连表查询。
    
    ```

16. 在Linux系统中，假设Nginx的访问日志位于`/var/log/nginx/access.log`，该文件的每一行代表一条访问记录，每一行都由若干列（以制表键分隔）构成，其中第1列记录了访问者的IP地址。请用一条命令找出最近的100000次访问中，访问频率最高的IP地址及访问次数。

    答案：

    ```Shell
    
    ```


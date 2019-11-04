# Written for work





1. Use test data

   ```python
   import sys
   sys.stdin = open('text.txt', 'r')
   a = int(sys.stdin.readline().strip())
   t = list(sys.stdin.readline().strip().split())
   #sys.stdout = open('output.txt', 'w')
   print(a)
   print(t)
   ```

2. Simple Server

   - python2:

     ```terminal
     python -m SimpleHttpServer port
     ```

   - python3:

     ```terminal
     python -m http.server port	
     ```

     

   

3. Min/Max value

   ```
   float('inf')
   float('-inf')
   
   ---------
   import sys
   INT_MAX = sys.maxsize
   INT_MIN = -sys.minsize - 1
   ```

   

4. Sort

   ```python
   sorted(arr, key = len)
   sorted(arr, key = str.lower)
   
   #cmp_to_key
   def largestNumber(nums):
     from functools import cmp_to_key:
       def helper(x, y):
         if x + y > y + x:
           return -1
         elif x + y < y + x:
           return 1
         else:
           return 0
        sorted(arr, key = cmp_to_key(helper))
       
   #Use class magic method
   def largestNumber(nums):
     def __lt(self, other):
       return self + other > other + self
    	sorted(arr, key = large_num)
     
   #Sorted one by one
   sorted(a, key = lambda x : (x[0], -x[1]))
   ```

   

5. Counter

   ```python
   # A counter tool is provided to support convenient and rapid tallie
   from collection import Counter
   arr = Counter(input()).values()
   ```

6. Int to binary string

   ```python
   st =  "{0:032b}".format(n)
   st = "{0:{fill}{width}}".format(n, fill = '0', width = 32)
   ```

7. Find median number from array

   ```python
   #Method 1: Use quick sort method
   # -*- coding:utf-8 -*-
   class Solution:
       def GetMedian(self, tempArr):
           length = len(tempArr)
           if length % 2 == 1:
               ans = self.solve(length // 2, tempArr)
           else:
               left = self.solve((length - 1)// 2, tempArr) 
               right = self.solve(length //2 , tempArr) 
               ans =  ( left + right ) / 2
           # write code here
           print(ans)
           
       def solve(self, mid, tempArr):
           k = self.partition(0, len(tempArr) - 1, tempArr)
           while k != mid:
               if k > mid:
                   k = self.partition(0, k - 1, tempArr)
               else:
                   k = self.partition(k + 1, len(tempArr) - 1, tempArr)
           return tempArr[k]
   
       def partition(self, l, r, tempArr):
           i = l - 1
           pivot = tempArr[r]
           for j in range(l, r):
               if tempArr[j] <= pivot:
                   i += 1
                   tempArr[i], tempArr[j] = tempArr[j], tempArr[i]
           tempArr[i + 1], tempArr[r] = tempArr[r], tempArr[i + 1]
           return i + 1
           
   #Method 2: Use statistics
   import statistics
   res = statistics.median(arr)
   
   ```

8. Multiple  Processes

   ```python
   #Multiple process
   from multiprocessing import Process
   from time import sleep
   
   counter = 0
   
   
   def sub_task(string):
       global counter
       while counter < 10:
           print(string, end='', flush=True)
           counter += 1
           sleep(0.01)
   
           
   def main():
       Process(target=sub_task, args=('Ping', )).start()
       Process(target=sub_task, args=('Pong', )).start()
   
   
   if __name__ == '__main__':
       main()
   ```

9. Multiple threads and lock

   ```python
   from time import sleep
   from threading import Thread, Lock
   
   
   class Account(object):
   
       def __init__(self):
           self._balance = 0
           self._lock = Lock()
   
       def deposit(self, money):
           # 先获取锁才能执行后续的代码
           self._lock.acquire()
           try:
               new_balance = self._balance + money
               sleep(0.01)
               self._balance = new_balance
           finally:
               # 在finally中执行释放锁的操作保证正常异常锁都能释放
               self._lock.release()
   
       @property
       def balance(self):
           return self._balance
   
   
   class AddMoneyThread(Thread):
   
       def __init__(self, account, money):
           super().__init__()
           self._account = account
           self._money = money
   
       def run(self):
           self._account.deposit(self._money)
   
   
   def main():
       account = Account()
       threads = []
       for _ in range(100):
           t = AddMoneyThread(account, 1)
           threads.append(t)
           t.start()
       for t in threads:
           t.join()
       print('账户余额为: ￥%d元' % account.balance)
   
   
   if __name__ == '__main__':
       main()
   ```

10. Coroutine

   ```python
   from multiprocessing import Process, Queue
   from random import randint
   from time import time
   
   
   def task_handler(curr_list, result_queue):
       total = 0
       for number in curr_list:
           total += number
       result_queue.put(total)
   
   
   def main():
       processes = []
       number_list = [x for x in range(1, 100000001)]
       result_queue = Queue()
       index = 0
       # 启动8个进程将数据切片后进行运算
       for _ in range(8):
           p = Process(target=task_handler,
                       args=(number_list[index:index + 12500000], result_queue))
           index += 12500000
           processes.append(p)
           p.start()
       # 开始记录所有进程执行完成花费的时间
       start = time()
       for p in processes:
           p.join()
       # 合并执行结果
       total = 0
       while not result_queue.empty():
           total += result_queue.get()
       print(total)
       end = time()
       print('Execution time: ', (end - start), 's', sep='')
   
   
   if __name__ == '__main__':
       main()
   ```

11. Request

    ```python
    #Http protocal by using request module.
    from time import time
    from threading import Thread
    
    import requests
    
    
    # 继承Thread类创建自定义的线程类
    class DownloadHanlder(Thread):
    
        def __init__(self, url):
            super().__init__()
            self.url = url
    
        def run(self):
            filename = self.url[self.url.rfind('/') + 1:]
            resp = requests.get(self.url)
            with open('/Users/Hao/' + filename, 'wb') as f:
                f.write(resp.content)
    
    
    def main():
        # 通过requests模块的get函数获取网络资源
        # 下面的代码中使用了天行数据接口提供的网络API
        # 要使用该数据接口需要在天行数据的网站上注册
        # 然后用自己的Key替换掉下面代码的中APIKey即可
        resp = requests.get(
            'http://api.tianapi.com/meinv/?key=APIKey&num=10')
        # 将服务器返回的JSON格式的数据解析为字典
        data_model = resp.json()
        for mm_dict in data_model['newslist']:
            url = mm_dict['picUrl']
            # 通过多线程的方式实现图片下载
            DownloadHanlder(url).start()
    
    
    if __name__ == '__main__':
        main()
    
    ```

12. Socket

    - Tcp Socket:

      ```python
      from socket import socket, SOCK_STREAM, AF_INET
      from datetime import datetime
      
      
      def main():
          # 1.创建套接字对象并指定使用哪种传输服务
          # family=AF_INET - IPv4地址
          # family=AF_INET6 - IPv6地址
          # type=SOCK_STREAM - TCP套接字
          # type=SOCK_DGRAM - UDP套接字
          # type=SOCK_RAW - 原始套接字
          server = socket(family=AF_INET, type=SOCK_STREAM)
          # 2.绑定IP地址和端口(端口用于区分不同的服务)
          # 同一时间在同一个端口上只能绑定一个服务否则报错
          server.bind(('192.168.1.2', 6789))
          # 3.开启监听 - 监听客户端连接到服务器
          # 参数512可以理解为连接队列的大小
          server.listen(512)
          print('服务器启动开始监听...')
          while True:
              # 4.通过循环接收客户端的连接并作出相应的处理(提供服务)
              # accept方法是一个阻塞方法如果没有客户端连接到服务器代码不会向下执行
              # accept方法返回一个元组其中的第一个元素是客户端对象
              # 第二个元素是连接到服务器的客户端的地址(由IP和端口两部分构成)
              client, addr = server.accept()
              print(str(addr) + '连接到了服务器.')
              # 5.发送数据
              client.send(str(datetime.now()).encode('utf-8'))
              # 6.断开连接
              client.close()
      
      
      if __name__ == '__main__':
          main()
      ```

13. Load json file

    ```python
    import json
    d = json.dumps(file)
    ```

14. Use slice to get data

    ```python
    ######    0123456789012345678901234567890123456789012345678901234567890'
    record = '....................100 .......513.25 ..........'
    SHARES = slice(20, 23)
    PRICE = slice(31, 37)
    cost = int(record[SHARES]) * float(record[PRICE])
    ```

15. Use list generator

    ```python
    nums = [1, 2, 3, 4, 5]
    s = sum(x * x for x in nums)
    ```

    
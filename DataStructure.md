You can find the original article in https://python3-cookbook.readthedocs.io/zh_CN/latest/c01/p15_group_records_based_on_field.html

1. Set

   ```python
   a = set()
   b = set()
   a.intersection(b)
   ```
   
   
   
2. Deque

   ```python
   from collections import deque
   q = deque(maxsize = 5)
   deque.append(element1)
   deque.append(element2)
   #在队列两端插入或删除元素时间复杂度都是 O(1) ，区别于列表，在列表的开头插入或删除元素的时间复杂度为 O(N)
   #The time complexity insert or delete is O(1), however, the time cost of list is O(N).
   ```

   

3. Heapq

   ```python
   import Heapq
   heapq.nlargest(cnt, nums)
   heapq.nsmallest(cnt, nums)
   #You can also use lambda to process dic
   heapq.nlargest(3, dic, lambda x : x['property2'])
   #If you want to find n smallest numbers from set or list, you can use heapify
   heap = list(nums)
   heapq.heapify(heap)
   ```

4. Priority Queue

   ```python
   #Use heapq to build priority queue structure.
   import heapq
   class PriorityQueue:
       def __init__(self):
           self._queue = []
           self._index = 0
   
       def push(self, item, priority):
           heapq.heappush(self._queue, (-priority, self._index, item))
           self._index += 1
   
       def pop(self):
           return heapq.heappop(self._queue)[-1]
   ```

5. MultiDict

   ```python
   #Use defaultdict from collections is a good choice and it will auto assign key value.
   from collections import defaultdict
   d = defaultdict(list)
   d['a'].append(1)
   d['a'].append(2)
   d['b'].append(4)
   
   d = defaultdict(set)
   d['a'].add(1)
   d['a'].add(2)
   d['b'].add(4)
   
   d = defaultdict(list)
   for key, value in pairs:
     d[key].append(value)
   ```

6. OrderedDict

   ```python
   #If you want to keep the order that elements insert, you can use orderDict.
   from collections import OrderedDict
   d = OrderedDict()
   d['foo'] = 1
   d['bar'] = 2
   d['spam'] = 3
   d['grok'] = 4
   for key in d :
     print(key, d[key])
     
   #OrderedDict 内部维护着一个根据键插入顺序排序的双向链表。每次当一个新的元素插入进来的时候， 它会被放到链表的尾部。对于一个已经存在的键的重复赋值不会改变键的顺序
   #There exists a bi-direction list in orderedDict to keep the order that elements insert. Every time a new element insert, it will be put the tail position of this list and if it exists the order of key will not be changed.
   ```

7. Operation of dict

   - Use zip to find minimum and maximum value.

     ```python
     prices = {
         'ACME': 45.23,
         'AAPL': 612.78,
         'IBM': 205.55,
         'HPQ': 37.20,
         'FB': 10.75
     }
     
     min_price = min(zip(prices.values(), prices.keys()))
     # min_price is (10.75, 'FB')
     max_price = max(zip(prices.values(), prices.keys()))
     # max_price is (612.78, 'AAPL')
     #Note that zip only generate a single use iterator.
     #注意zip只能产生一次洗使用的迭代器
     ```

   - Use lambda expression

     ```python
     min(prices, key=lambda k: prices[k]) # Returns 'FB'
     max(prices, key=lambda k: prices[k]) # Returns 'AAPL'
     ```

   - Find common elements between two dict.

     ```python
     #Find keys in common
     a.keys() & b.keys()
     #Find keys in a that are not in b
     a.keys() - b.keys()
     #Find (key, value) pairs in common
     a.items & b.items()
     ```

8. Counter

   ```python
   from collections import Counter
   word_counts = Counter(words)
   top_three = word_counts.most_common(3)
   #Update word_counts
   word_counts.update(morewords)
   #Combine counts
   c = a + b
   d = a - b
   ```

9. Itemgetter

   ```python
   rows = [
       {'fname': 'Brian', 'lname': 'Jones', 'uid': 1003},
       {'fname': 'David', 'lname': 'Beazley', 'uid': 1002},
       {'fname': 'John', 'lname': 'Cleese', 'uid': 1001},
       {'fname': 'Big', 'lname': 'Jones', 'uid': 1004}
   ]
   #Itemgetter is little faster than lambda.
   from operator import itemgetter
   rows_by_fname = sorted(rows, key = itemgetter('fname'))
   rows_by_uid = sorted(row, key = itemgetter('uid'))
   rows_by_lfname = sorted(rows, key=itemgetter('lname','fname'))
   ```

10. Attrgetter

   ```python
   #Sort object by the attribute.
   from operator import attrgetter
   sorted(users, key=attrgetter('user_id'))
   ```

11. Itertools.groupby()

    ```python
    rows = [
        {'address': '5412 N CLARK', 'date': '07/01/2012'},
        {'address': '5148 N CLARK', 'date': '07/04/2012'},
        {'address': '5800 E 58TH', 'date': '07/02/2012'},
        {'address': '2122 N CLARK', 'date': '07/03/2012'},
        {'address': '5645 N RAVENSWOOD', 'date': '07/02/2012'},
        {'address': '1060 W ADDISON', 'date': '07/02/2012'},
        {'address': '4801 N BROADWAY', 'date': '07/01/2012'},
        {'address': '1039 W GRANVILLE', 'date': '07/04/2012'},
    ]
    
    from operator import itemgetter
    from itertools import groupby
    
    # Sort by the desired field first.
    rows.sort(key=itemgetter('date'))
    # Iterate in groups.
    for date, items in groupby(rows, key=itemgetter('date')):
        print(date)
        for i in items:
            print(' ', i)
    ```

12. Filter

    ```python
    values = ['1', '2', '-3', '-', '4', 'N/A', '5']
    def is_int(val):
        try:
            x = int(val)
            return True
        except ValueError:
            return False
    ivals = list(filter(is_int, values))
    ```

13. ChainMap

    ```python
      from collections import ChainMap
      a = {"x":1, "z":3}
      b = {"y":2, "z":4}
      c = ChainMap(a,b)
      print(c)
      print("x: {}, y: {}, z: {}".format(c["x"], c["y"], c["z"]))
      ChainMap({'x': 1, 'z': 3}, {'y': 2, 'z': 4})
      x: 1, y: 2, z: 3
    ```

    






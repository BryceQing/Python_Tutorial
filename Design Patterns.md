# Design Patterns

1. Single instance

   ```python
   class SingleCase(object):
   		_instance = None
       def __new__(cls, *args, **kwargs):
         	if cls._instance is None:
             	cls._instance = super().__new__(cls, *args, **kwargs)
            return cls._instance
   
   a = SingleCase()
   b = SingleCase()
   ```

   https://www.jianshu.com/p/ec6589e02e2f
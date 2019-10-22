1. split

   ```python
   #When you need process string by multiple sperator, you should use re.split()
   import re
   re.split(r'[;,\s]\s*, line')
   ```

2. Check String begin and end

   ```python
   str.startswith()
   str.endswith()
   str.find()
   #Note the parameter with multiple elements must be tuple.
   ```

3. Use regex expression

   ```python
   import re
   regex = re.compile(r'\d+/\d+/\d+')
   regex.match(string) #match from begin
   regex.findall(string) #match from any position
   >>> m = datepat.match('11/27/2012')
   >>> m
   <_sre.SRE_Match object at 0x1005d2750>
   >>> # Extract the contents of each group
   >>> m.group(0)
   '11/27/2012'
   >>> m.group(1)
   '11'
   >>> m.group(2)
   '27'
   >>> m.group(3)
   '2012'
   >>> m.groups()
   ('11', '27', '2012')
   >>> month, day, year = m.groups()
   >>>
   >>> # Find all matches (notice splitting into tuples)
   >>> text
   'Today is 11/27/2012. PyCon starts 3/13/2013.'
   >>> datepat.findall(text)
   [('11', '27', '2012'), ('3', '13', '2013')]
   >>> for month, day, year in datepat.findall(text):
   ... print('{}-{}-{}'.format(year, month, day))
   ...
   2012-11-27
   2013-3-13
   >>>
   ```

4. Replace

   ```python
   #Method 1
   str.replace(a, b)
   #Method 2
   >>> text = 'Today is 11/27/2012. PyCon starts 3/13/2013.'
   >>> import re
   >>> re.sub(r'(\d+)/(\d+)/(\d+)', r'\3-\1-\2', text)
   'Today is 2012-11-27. PyCon starts 2013-3-13.'
   ```

5. Case insensitive

   ```python
   re.findall('python', text, flags = re.IGNORECASE)
   ```

6. Delete character

   ```python
   #Whitespace strip
   s.strip()
   #Left strip
   s.lstrip()
   #Right strip
   s.rstrip()
   #Multiple characters strip
   s.strip('-=')
   #Strip internal whitespace
   s.replace(' ','')
   import re
   re.sub('\s+', ''m, str)
   ```

7. String align

   ```python
   text.ljust(20)
   text.rjust(20)
   text.center(20, '*')
   
   format(text, '>20')
   format(text, '<20')
   format(text, '^20')
   x = 1.2345
   format(x, '>10')
   '			1.2345'
   format(x, '^10.2f')
   '   1.23.  '
   ```

8. print

   ```python
   print(a + ':' + b + ':' + c) # Ugly
   print(':'.join([a, b, c])) # Still ugly
   print(a, b, c, sep = ':') # Better
   ```

   
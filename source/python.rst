######
python
######
*******
pycharm
*******

output tweaks
=============

selective run
-------------

- run code selection: `Alt + Shift + E`


********
packages
********

common
======

matplotlib
----------


.. code-block:: python

   plt.show()


numpy
-----

https://numpy.org/doc/stable/reference/generated/numpy.set_printoptions.html

.. code-block:: python

   desired_width = 640
   np.set_printoptions(linewidth=desired_width)

pandas
------

https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.set_option.html

.. code-block:: python

  pd.set_option('display.max_columns', 10)
  # >>> df.to_html('filename.html')
  pd.set_option('display.max_rows', 10)
  pd.set_option('display.width', desired_width)  # make output in console wider

ipynb
-----

convert them pesky ipynb files to markdown files 

.. code-block:: bash

   find -type f -iname '*.ipynb' -print0 | xargs -0 -n1 -P4 jupyter nbconvert --to markdown


*******
library
*******

toolkit
=======

first collection
----------------

initiating stuff

.. code-block:: python

   def initiate_numpy(console_width=640):
       # https://numpy.org/doc/stable/reference/generated/numpy.set_printoptions.html
       try:
           import numpy as np
           np.set_printoptions(linewidth=console_width)
           return np
       except ImportError:
           print("numpy not installed")

   def initiate_pandas(max_rows=10, max_cols=10, console_width=640):
       try:
           import pandas as pd
           pd.set_option('display.max_columns', max_cols)
           pd.set_option('display.max_rows', max_rows)
           pd.set_option('display.width', console_width)  # make output in console wider
           return pd
       except ImportError:
           print("pandas not installed")


.. code-block:: python

   from pathlib import Path
   import contextlib
   import os
   
   
   def file_exists(file_path):
       return Path(file_path).exists()
   
   
   def compress_to_bz2_file(source_file, output_file, compression_lvl=9):
       import bz2
       if 1 <= compression_lvl <= 9:
           with open(source_file, 'rb') as data:
               tar_bz2_contents = bz2.compress(data.read(), compresslevel=compression_lvl)
       else:
           raise ValueError("compression level has to be between 1 and 9")
   
   
   # concatenate_files_in_list(text_files_list, final_file, 'latin-1', True)
   def concatenate_files_in_list(file_list, output_file, encode_in='utf-8', is_small=False, use_shutil=False):
       # don't forget to specify target folder when opening files!
       # todo: check if file was created already
       encoding_format = encode_in
       print(f"target file -> {output_file}")
       readmode = ('rb' if use_shutil else 'r')
       writemode = ('wb' if use_shutil else 'w+')
       with open(output_file, writemode) as finaltext:
           for file_name in file_list:
               print(f"currently appending file: {file_name}")
               with open(file_name, readmode, encoding=encoding_format) as file_currently_opened:
                   if use_shutil:
                       import shutil
                       shutil.copyfileobj(file_name, finaltext)
                   elif is_small:
                       finaltext.write(file_currently_opened.read())
                   elif not is_small:
                       for line in file_currently_opened:
                           finaltext.write(line)
   
   
   def add_files_from_folder_to_list(filetype, folder_target='./'):
       folder = set_folder(folder_target)
       print(folder)
       print(f"getting files from {folder} of type {filetype} ...")
       all_files = []
       for path, dirs, files in os.walk(folder):
           for filename in files:
               if filename.endswith('.txt'):
                   full_path = os.path.join(path,filename)
                   all_files.append(full_path)
       with contextlib.suppress(Exception):
           raise RuntimeError('something went wrong')
       return all_files
   
   
   def set_folder(folder_dir):
       isdir = Path(folder_dir)
       if not isdir:
           raise Exception(f"folder you're trying to set is invalid -> {folder_dir}")
       absolute_path = isdir.resolve()
       return absolute_path
   
   
   def test_files_in_list(file_list: []):
       print("testing files in list... ")
       for file in file_list:
           if not Path(file).exists():
               print(f"oops: {file} doesn't seem to exist")

*****
conda
*****

urls
====
https://conda.io/projects/conda/en/latest/user-guide/tasks/index.html

install
=======

download anaconda from https://www.anaconda.com/products/individual#Downloads

.. code::

   wget https://repo.anaconda.com/archive/Anaconda3-2021.05-Linux-x86_64.sh

try anaconda navigator

.. code::

   anaconda-navigator

more info: https://docs.anaconda.com/anaconda/navigator/


environments
============

managing environments
---------------------

conda `env mgmt`_ documentation

.. _env mgmt: https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html

.. code::

   $ conda-env list
   # conda environments:
   #
   base                  *  /home/dadude/anaconda3
   helloworld               /home/dadude/anaconda3/envs/helloworld

backup environment
------------------

.. code::

   conda env export --from-history -n helloworld --no-builds | grep -v "prefix" > backup_env.yml

   cat backup_env.yml
   name: helloworld
   channels:
     - defaults
   dependencies:
     - sphinx
     - argcomplete


create environment
------------------

checkout python version you want to use

.. code::

   conda search python

.. code::

   conda create -n envname python=3.9

remove environment
------------------

.. code::

   conda env remove -n ENV_NAME

packages
========

search
------

.. code::

   conda search scipy

backup used packages
--------------------

.. code::

   conda list --export > conda_reqs.txt



install required packages
-------------------------

.. code::

   conda create --name envname --file conda_reqs.yml


***
pip
***

package mgmt
============

backup required packages
------------------------

.. code::

   pip3 freeze > pip_requirements.txt

install required packages
-------------------------

.. code::

   python3 -m venv envname
   source env/bin/activate
   pip3 install -r pip_requirements.txt

virtualenvwrapper
-----------------

documentation `virtualenvwrapper`_

.. _virtualenvwrapper: https://virtualenvwrapper.readthedocs.io/en/latest/command_ref.html

install
.. code::

   sudo apt-get install virtualenvwrapper


add to bashrc once virtualenvwrapper is installed

.. code::

   #Virtualenvwrapper settings:
   export WORKON_HOME=$HOME/venv
   VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
   source /usr/share/virtualenvwrapper/virtualenvwrapper.sh
   
   (base) dadude@dahost ~/gits/docuhub (main)$ source ~/.bashrc
   virtualenvwrapper.user_scripts creating /home/dadude/.venvs/premkproject
   virtualenvwrapper.user_scripts creating /home/dadude/.venvs/postmkproject
   virtualenvwrapper.user_scripts creating /home/dadude/.venvs/initialize
   virtualenvwrapper.user_scripts creating /home/dadude/.venvs/premkvirtualenv
   virtualenvwrapper.user_scripts creating /home/dadude/.venvs/postmkvirtualenv
   virtualenvwrapper.user_scripts creating /home/dadude/.venvs/prermvirtualenv
   virtualenvwrapper.user_scripts creating /home/dadude/.venvs/postrmvirtualenv
   virtualenvwrapper.user_scripts creating /home/dadude/.venvs/predeactivate
   virtualenvwrapper.user_scripts creating /home/dadude/.venvs/postdeactivate
   virtualenvwrapper.user_scripts creating /home/dadude/.venvs/preactivate
   virtualenvwrapper.user_scripts creating /home/dadude/.venvs/postactivate
   virtualenvwrapper.user_scripts creating /home/dadude/.venvs/get_env_details


workon will work now

.. code::

   (base) dadude@dahost ~/gits/docuhub (main)$ workon 
   code
   deleteme
   desktop
   docu
   docuhub
   jupyter
   networking
   otp_client
   portfolio
   pytest
   testing_mocks
   
   (docu) (helloworld) dadude@dahost ~/gits/docuhub (main)$ workon docuhub
   (docuhub) (helloworld) dadude@dahost ~/gits/docuhub (main)$ 

virtualenvwrapper commands
^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code::

   mkvirtualenv new_venv_name
   lsvirtualenv
   rmvirtualenv name_of_your_env
   cpvirtualenv old_virtual_env new_virtual_env


***********
documenting
***********

sphinx
======

setup sphinx
------------

.. code::
  
   [ ! -d "$HOME/reqfiles" ] && mkdir $HOME/reqfiles && echo -e "rstcheck\nSphinx\nsphinx-autobuild\nsphinx-rtd-theme\nsphinx-rtd-dark-mode" > $HOME/sphinx_reqs.txt

   python3 -m venv sphinx
   source ~/venv/sphinx/bin/activate

   python3 -m pip install --upgrade pip
   
   #(sphinx) (base) dadude@dahost:~/venv$
   pip3 install -r ~/reqfiles/sphinx_reqs.txt


   cd ~/gits;git clone git@github.com:brunswyck/docuhub.git;cd ~/gits/docuhub


pip3 requirements
-----------------

.. code::

   docutils
   rstcheck
   Sphinx
   sphinx-autobuild
   sphinx-rtd-theme

extensions
----------

graphviz
^^^^^^^^

http://graphviz.org/

::
  sudo apt-get install graphviz

revealjs
^^^^^^^^

https://sphinx-revealjs.readthedocs.io/en/latest/
https://pypi.org/project/sphinx-revealjs/

.. code-block:: python

   pip install sphinx-revealjs
   """
   extensions = [
     'sphinx_revealjs',
   ]
   """

matplotlib
^^^^^^^^^^

https://matplotlib.org/

::
  sudo apt-get install python3-matplotlib


pytest
^^^^^^

https://pypi.org/project/pytest-sphinx/

https://numpydoc.readthedocs.io/en/latest/format.html
https://numpydoc.readthedocs.io/en/latest/
https://pypi.org/project/numpydoc/

numpydoc
^^^^^^^^

::
  pip install numpydoc

https://ipython.readthedocs.io/en/stable/sphinxext.html

ipython
^^^^^^^

::
  pip install ipython

sphinx-gallery
^^^^^^^^^^^^^^

https://pypi.org/project/sphinx-gallery

https://sphinx-gallery.github.io/stable/getting_started.html

.. code-block:: bash

   pip install pillow
   pip install matplotlib
   pip install seaborn
   pip install sphinx-gallery # extension wanted

create these dirs & files

a py file with *plot_* prefix that generates the plot
a py file with just the code
a readme file

.. code-block:: bash

   (docuhub) ~/gits/docuhub (main)$ mkdir gallery_code
   (docuhub) ~/gits/docuhub (main)$ mkdir source/gallery
   (docuhub) ~/gits/docuhub (main)$ touch gallery_code/README.rst gallery_code/example.py gallery_code/plot_example.py 

content for examples

structure examples/example.py

https://sphinx-gallery.github.io/stable/syntax.html#python-script-syntax

::

  """
  "This" is my example-script
  ===========================
  
  This example doesn't do much, it just makes a simple plot
  """

  # Code source: Óscar Nájera
  # License: BSD 3 clause
  
  import numpy as np
  import matplotlib.pyplot as plt
  
  x = np.linspace(0, 2 * np.pi, 100)
  y = np.sin(x)
  
  plt.plot(x, y)
  plt.xlabel(r'$x$')
  plt.ylabel(r'$\sin(x)$')
  # To avoid matplotlib text output
  plt.show()

add to source/conf.py

.. code-block:: bash

   extensions = [
       ...
       'sphinx_gallery.gen_gallery',
       ]

   sphinx_gallery_conf = {
        'examples_dirs': '../gallery_code',   # code here
        'gallery_dirs': 'gallery',  # gallery generated output
   }

   html_static_path = ['_static', 'gallery']

disable download files

::

  Disable joint download of all gallery scripts
  By default Sphinx-Gallery prepares zip files of all python scripts and all Jupyter notebooks for each gallery section and makes them available for download at the end of each section. To disable this behavior add to the configuration dictionary in your conf.py file
  
  sphinx_gallery_conf = {
      'download_all_examples'  : False}

to check on generated images

.. code-block::

   (docuhub) ~/gits/docuhub (main)$ ls docs/_images/sphx*
   docs/_images/sphx_glr_plot_example_001.png
   docs/_images/sphx_glr_plot_example_thumb1.png
   docs/_images/sphx_glr_plot_example_thumb.png
   docs/_images/sphx_glr_sin_demo_thumb.png


cross referencing to gallery code

https://sphinx-gallery.github.io/stable/advanced.html#cross-referencing

.. code-block::

   :ref:`sphx_glr_auto_examples_plot_example.py`


The examples directory is also copied to doc/gallery by sphinx-gallery, so plots from the examples directory can be included using

.. include:: ../gallery_code/sin_demo.py

to add the code from gallery_code/plot_example.py file

::

  .. literalinclude:: ../../gallery_code/plot_example.py

to add the generated image

::

  .. image:: ../../_images/sphx_glr_plot_example_001.png

ignore with `sphinx-autobuild --ignore "*.pickle"`

************
fundamentals
************
code examples
=============

for x in range(number+1)
------------------------

.. code-block:: python

   for count_25 in range(4+1):  # note the +1 to indicate 4 is not included
       print(count_25)
   for count_25 in range(4+1):
       print(count_25)
   0
   1
   2
   3
   4

.. code-block:: python

   # loop over all possible counts for each coin (summing up to <= 1$);
   # if the total amount is exactly $1, add current counts to "combinations"
   
   combinations = []
   
   for count_100 in range(1+1):
       for count_50 in range(2+1):
           for count_25 in range(4+1):
               for count_10 in range(10+1):
                   for count_5 in range(20+1):
                       for count_1 in range(100+1):
                           if 100*count_100 + 50*count_50 + 25*count_25 + 10*count_10 + 5*count_5 + count_1 == 100:
                               combinations.append([count_100, count_50, count_25, count_10, count_5, count_1])

   combinations
   [[0, 0, 0, 0, 0, 100],
    [0, 0, 0, 0, 1, 95],
    [0, 0, 0, 0, 2, 90],
    [0, 0, 0, 0, 3, 85],
    [0, 0, 0, 0, 4, 80],
    [0, 0, 0, 0, 5, 75],
    [0, 0, 0, 0, 6, 70],
    [0, 0, 0, 0, 7, 65],
    [0, 0, 0, 0, 8, 60],
    [0, 0, 0, 0, 9, 55], # cut here ..
    [0, 1, 1, 0, 2, 15],
    [0, 1, 1, 0, 3, 10],
    [0, 1, 1, 0, 4, 5],
    [0, 1, 1, 0, 5, 0],
    [0, 1, 1, 1, 0, 15],
    [0, 1, 1, 1, 1, 10],
    [0, 1, 1, 1, 2, 5],
    [0, 1, 1, 1, 3, 0],
    [0, 1, 1, 2, 0, 5],
    [0, 1, 1, 2, 1, 0],
    [0, 1, 2, 0, 0, 0],
    [0, 2, 0, 0, 0, 0],
    [1, 0, 0, 0, 0, 0]]
    len(combinations)
    293

summary
=======

underscore
----------

* Underscore _ is considered as "I don't Care" or "Throwaway" variable in Python
* ipython stores the last expression value to the special variable called _
* underscore _ is also used for ignoring the specific values. If the values are not used, just assign the values to underscore
* particular function implementation doesn't need all of the parameters

.. code-block:: python

   10
   10

   _
   10

   _ * 3
   30

   # Ignore a value when unpacking

   x, _, y = (1, 2, 3)

   x
   1

   y
   3

   # Ignore the index

   for _ in range(10):
       do_something()

   # not all params in def/lambda are needed

   def callback(_):
       return True

   lambda _: 1.0  # don't require argument


list indices
------------

.. code-block:: python

   x = ["first", "second", "third", "fourth"]
   y = [1, 2, 3, 4]

.. list-table:: indices
   :widths: 20 20 20 20 20
   :header-rows: 1

   * - x = [
     - "first",
     - "second",
     - "third",
     - "fourth"     ]
   * - positive index
     - 0
     - 1
     - 2
     - 3
   * - negative index
     - -4
     - -3
     - -2
     - -1

.. code-block:: python

   x[1:-1]
   ["second", "third"]

   x[0:3]
   ["first", "second", "third"]  # last index not included!

   x[:3]
   ["first", "second", "third"]

   x[2:]
   ["third", "fourth"]

   # append list to end of list
   x[len(x):] = [5, 6, 7]

   # make copy you can modify
   y = x[:]

   # add list to front of list (prepend)
   x[:0] # up to first element
   x[:0] = [-1, 0]
   x
   [-1, 0, 1, 2, 3, 4, 5, 6, 7]

   # delete part of list
   x[1:-1] = []  # 2nd element up to last (0 -> 7) not including 7
   x
   [-1, 7]

   # glue one list to another
   x.extend(y)

   # x.append(y) will add y list as an element, not extend
   [1, 2, 3, 4, [5, 6, 7]]

   del x[4]
   [1, 2, 3, 4]

   del x[:3]
   [4]

   z = [None] * 4
   z
   [None, None, None, None]


.. warning:: x.remove(3) doesn't remove 4th element, it removes the first 3 it finds in the list

list operations
---------------

.. list-table:: list operations
   :widths: 25 75 25
   :header-rows: 1

   * - List operation
     - Explanation
     - Example
   * - []
     - Creates an empty list 
     - x = []
   * - len
     - Returns the length of a list
     - len(x)
   * - append
     - Adds a single element to the end of a list
     - x.append('y')
   * - extend
     - Adds another list to the end of the list
     - x.extend(['a', 'b'])
   * - insert
     - Inserts a new element at a given position in the list
     - x.insert(0, 'y')
   * - del
     - Removes a list element or slice
     - del(x[0])
   * - remove
     - Searches for and removes a given value from a list
     - x.remove('y')
   * - reverse
     - Reverses a list in place
     - x.reverse()
   * - sort
     - Sorts a list in place
     - x.sort()
   * - +
     - Adds two lists together
     - x1 + x2
   * - *
     - Replicates a list
     - x = ['y'] * 3
   * - min
     - Returns the smallest element in a list
     - min(x)
   * - max
     - Returns the largest element in a list
     - max(x)
   * - index
     - Returns the position of a value in a list
     - x.index['y']
   * - count
     - Counts the number of times a value occurs in a list
     - x.count('y')
   * - sum
     - Sums the items (if they can be summed)
     - sum(x)
   * - in
     - Returns whether an item is in a list
     - 'y' in x


nested lists
------------

.. code-block:: python

   m = [[0, 1, 2], [10, 11, 12], [20, 21, 22]]
   m[0]
   # [0, 1, 2]
   m[0][1]
   # 1
   m[2]
   # [20, 21, 22]
   m[2][2]
   # 22

lists are mutable so can change inside a variable, watch out

.. code-block:: python

   nested = [0]
   original = [nested, 1]
   original
   [[0], 1]
   nested[0] = 'zero'
   original
   [['zero'], 1]
   original[0][0] = 0
   nested
   [0]
   original
   [[0], 1]
   # the following makes nested point to a different list
   nested = [2]
   # so original no longer points to the same list but another one
   original
   [[0], 1]

deep copy
---------

if your list has other lists nested in it you may want a **deep copy**

.. code-block:: python

   original = [[0], 1] <-----
   # shallow copy just refer|ences to original list
   shallow = original[:] ----
   import copy
   # deep copy doesn't reference but creates new list
   deep = copy.deepcopy(original)


tuples
------

.. note::
   add a comma to force a tuple!


.. code-block:: python

   x = 3
   y = 4
   (x+y)
   7
   type(x)
   # <class 'int'>
   type(x+y)
   # <class 'int'>
   # z is not a tuple!
   z = (x+y)
   type(z)
   # <class 'int'>
   # adding a comma it now is a tuple!
   z = (x+y,)
   type(z)
   # <class 'tuple'>
   # create empty tuple
   x = ()
   type(x)
   # <class 'tuple'>


packing unpacking
^^^^^^^^^^^^^^^^^

.. code-block:: python

   (one, two, three, four) = (1, 2, 3, 4)
   type(one)
   # <class 'int'>
   one
   1

variable swapping
^^^^^^^^^^^^^^^^^

.. code-block:: python

   var1, var2 = var2, var1

extended unpacking
^^^^^^^^^^^^^^^^^^

.. code-block:: python

   x = (1, 2, 3, 4)
   a, b, *c = x
   a, b, c
   (1, 2, [3, 4])

   a, *b, c = x
   a, b, c
   (1, [2, 3], 4)

   *a, b, c = x
   a, b, c
   ([1, 2], 3, 4)

   a, b, c, d, *e = x
   a, b, c, d, e
   (1, 2, 3, 4, [])


.. note::

   element with * absorbs all elements not matching the others as a list
   if there are no surplus items it receives an empty list


dictionaries
------------

get function
^^^^^^^^^^^^

if the dictionary doesn't contain the key, it returns a second argument

.. code-block:: python

   print(en_to_nl.get('blue', 'no translation')
   blauw
   print(en_to_nl.get('purp', 'no translation')
   no translation

.. note:: 2nd arg is optional, if not present it returns None

setdefault
^^^^^^^^^^

.. note:: set a key value if there is no value set! awesome

.. code-block:: python

   x = {1: 'one', 2: 'two', 3: '', 4: 'four'}
   x.get(2)
   'two'
   x.get(2, 'pump it')
   'two'
   # no key 5, so it will return 'pump it'
   x.get(5, 'pump it')
   'pump it'
   x.setdefault(3, 'three')
   ''
   # no key 5, so the message 'five' is returned but key not created
   x.get(5, 'five')
   'five'
   x
   {1: 'one', 2: 'two', 3: '', 4: 'four'}
   # no key 5, so the key is created along with 'five' value
   x.setdefault(5, 'five')
   'five'
   x
   {1: 'one', 2: 'two', 3: '', 4: 'four', 5: 'five'}


.. list-table:: values that can be used as dictionary keys
   :widths: 25 25 25 25
   :header-rows: 1

   * - type
     - immutable?
     - hashable?
     - dictionary key?
   * - int
     - yes
     - yes
     - yes
   * - float
     - yes
     - yes
     - yes
   * - boolean
     - yes
     - yes
     - yes
   * - complex
     - yes
     - yes
     - yes
   * - str
     - yes
     - yes
     - yes
   * - bytes
     - yes
     - yes
     - yes
   * - bytearray
     - no
     - no
     - no
   * - list
     - no
     - no
     - no
   * - tuple
     - yes
     - sometimes
     - sometimes
   * - set
     - no
     - no
     - no
   * - frozenset
     - yes
     - yes
     - yes
   * - dictionary
     - no
     - no
     - no

dictionary as cache
^^^^^^^^^^^^^^^^^^^

.. code-block:: python

   calc_cache = {}

   def calculation(x, y, z):
       if (x, y, z) in calc_cache:
           return calc_cache[(x, y, z)]
       else:
           # some time-consuming calculation
           calc_cache[(x, y, z)] = result
           return result



for loop tuple
--------------

.. code-block:: python

   somelist = [(1, 2), (3, 4), (5, 6)]
   result = 0
   # this isn't a dictionary but tuple unpacking
   for x, y in somelist:
       result = result + (x * y)

   result
   44

.. note:: the enumerate function returns (index, item) tuples

combining iterables to for loop over
------------------------------------

.. code-block:: python

   x = [1, 2, 3, 4]
   y = ['a', 'b', 'c']
   z = zip(x, y)
   for element in z:
       print(element)
   (1, 'a')
   (2, 'b')
   (3, 'c')

.. code-block:: python

   
.. code-block:: python

   
.. code-block:: python

   
.. code-block:: python

   
.. code-block:: python

   
modularity
==========

main function & cli arguments
-----------------------------

.. code-block:: python

   import sys

   def main(url):
       print(url)

   if __name__ == '__main__': # the module is being executed directly not imported
       main(sys.argv[1]) # the 0th arg is the module/script filename 
   

more on command-line parsing module: https://docs.python.org/3/howto/argparse.html
an alternative is docopt: https://github.com/docopt/docopt

docstrings
----------

.. code-block:: python

   def some_funtion(items):
       """Print items one per line

       Args:
           An iterable series of printable items
       """
       for item in items:
           print(items)

module docstrings
^^^^^^^^^^^^^^^^^

.. code-block:: python

   """ retrieve and print items from a list

   Usage:

       python3 list.py <list>
   """

   import sys

.. note:: when you now call help on the module you'll get a lot of information


objects
-------

.. code-block:: python

   # id() returns a unique identifier for an object
   a = 300
   id(a)
   4298473345
   b = 1500
   id(b)
   4298222345
   b = a
   id(b)
   4298473345 # same as a
   id(a) == id(b)
   True
   a is b
   True

function default values for arguments
-------------------------------------

.. code:: python

   def banner(message, border='-'): # place after args without defaults
    line = border * len(message)
    print(line)
    print(message)
    print(line)

   banner("Norwegian Blue")
   banner("Sun, Moon and Stars", "*")
   banner("Sun, Moon and Stars", border="*")
   banner(border=".", message="Hello from Earth")

.. warning:: always use immutable objects like ints and strings as default values eg you dont want a function time to always show the time of first function call

collections
===========

tuple
-----

.. code-block:: python

   k = (391,) # use the trailing comma, otherwise it will be an int 
   
   def minmax(items):
       return min(items), max(items) # tuples are useful for multiple return values

   minmax([83, 33, 82, 39, 88, 31, 86])

   # tuple unpacking allows us to destructure into named references
   lower, upper = minmax([83, 33, 82, 39, 88, 31, 86])
   lower
   31
   upper
   86
   # idiomatic python swap
   a = 'jelly'
   b = 'bean'
   a, b = b, a
   a
   'bean'
   # use the tuple() constructor to create tuples from other iterable objects
   tuple([561, 1105, 1219, 2466])
   (561, 1105, 1219, 2466)
   tuple("Patrick")
   ('P', 'a', 't', 'r', 'i', 'c', 'k')
   5 in (2, 3, 5, 18, 22, 55)
   True
   4 not in (2, 3, 5, 18, 22, 55)
   True


string
------

format
^^^^^^

.. code-block:: python

   import unittest

   name = "Alan Turing"
   age = 42
   person = [name, age, "mathematician"]
   text = "Hello, my name is {} and I am {} years old and I am a {}.".format(*person)
   # Hello, my name is Alan Turing and I am 42 years old an I am a mathematician
   typeAge = type(age)


   class TestNotebook(unittest.TestCase):

       def test_name(self):
           self.assertEqual(name, "Alan Turing")

       def test_age(self):
           self.assertEqual(age, 42)

       def test_person(self):
           self.assertEqual(person,["Alan Turing", 42, "mathematician"])

       def test_text(self):
           self.assertEqual(text,"Hello, my name is Alan Turing and I am 42 years old and I am a mathematician.")

       def test_type(self):
           self.assertEqual(typeAge,type(int()))


   unittest.main(argv=[''], verbosity=2, exit=False)


.. code-block:: python
   
   "New" + "found" + "land"
   'Newfoundland'
   # use join
   colors = ';'.join(['#45ff12', '#2323fa', '#1234a2', '#4313ff'])
   '#45ff12;#2323fa;#1234a2;#4313ff'
   # split them up again
   colors.split(';')
   ['#45ff12', '#2323fa', '#1234a2', '#4313ff']
   # using the empty string as a seperator to join
   ''.join(['high', 'way', 'man'])
   'highwayman'
   # partion() divides a string -> prefix,seperator,suffix
   "unforgetable".partition("forget")
   ('un', 'forget', 'able')
   # usage with tuples
   departure, seperator, arrival = "London:Edinburgh".partition(':')
   arrival
   Edinburgh
   # underscore dummy value
   origin, _, destination = "Seattle-Boston".partition('-')
   # use format() to insert values into strings
   "The age of {0} is {1}".format('patrick', 35)
   "The age of {0} is {1}. {0}'s birthday is on {2}".format('Patrick', 35, 'June 1')
   # they can be ommitted when in the exact order and used only once
   "The age of {} is {}".format('patrick', 35)
   # use named fields
   "Current position {latitude} {longitude}".format(latitude="60N",
                                                    longitude="5E")
   'Current position 60N 5E'
   # access values through keys or indexes with []
   pos = (65.2, 23.1, 82.2)
   "Galactic position x={pos[0]} y={pos[1] z={pos[2]}".format(pos=pos)
   'Galactic position x=65.2 y=23.1 z=82.2'
   # replacement field mini-language provides many value n alignment formatting options
   import math
   "Math constants: pi={m.pi:.3f}, e={m.e:.3f}".format(m=math)
   'Math constants: pi=3.142, e=2.718'
   help(str)




range
-----

.. code-block:: python
   
   list(range(5, 10)) # stop is not included
   [5, 6, 7, 8, 9]
   # with step argument
   list(range(0, 10, 2))
   [0, 2, 4, 6, 8]
   # avoid range for iterating over lists
   for item in list:
       print(item)
   # not
   for item in range(len(list)):
       print(item)
   
   # prefer enumerate() for counters
   # enumerate() yields (index, value) tuples
   t = [5, 333, 8888, 144444, 2012011]
   for p in enumerate(t):
       print(p)

   (0, 6)
   (1, 333)
   (2, 8888)
   (3, 144444)
   (4, 2012011)
   # improvement with tuple unpacking
   for i, v in enumerate(t):
       print("i = {}, v = {}".format(i, v))
   
::

   i = 0, v = 6 
   i = 1, v = 333
   i = 2, v = 8888
   i = 3, v = 144444
   i = 4, v = 2012011




list
----

.. code-block:: python
   
   # SHALLOW copying a list (contain same object references as the source list!!)
   copy_list = sequence[:] # full slice
   copy_list = other_list.copy() # use copy method
   copy_list = list(some_other_list) # use the list constructor


shallow copies
--------------

.. code-block:: python
   
   a = [ [1, 2], [3, 4] ]
   b = a[:] # create a shallow copy
   a[0] = [8, 9] # create new list in a at 0
   a[0]
   [8, 9]
   b[0] # b still references to the old list object b4 a created a new one
   [1, 2]
   a[1].append(5) # just appending to the list in a also affects the list for b 
   b
   [[1, 2], [3, 4, 5]] # they both still refer to the same object

list repetition
---------------

.. code-block:: python

   # repeat lists using the * operator
   c = [21, 37]
   d = c * 4
   d
   [21, 37, 21, 37, 21, 37, 21, 37] # this method is often used to zero a list of fixed length
   s = [constant] * size
   [0] * 9 # same principle as with list copying so beware, repetition = shallow
   # repetition repeats the reference without copying the value
   [0, 0, 0, 0, 0, 0, 0, 0, 0]

   s = [ [-1, +1] ] * 5
   [[-1, +1], [-1, +1], [-1, +1], [-1, +1], [-1, +1]]
   s[3].append(7) # because all innerlists refer to same object they are all modified
   s
   [[-1, 1, 7], [-1, 1, 7], [-1, 1, 7], [-1, 1, 7], [-1, 1, 7]]




more on list
------------

.. code::

   w = "the quick brown fox jumps over the lazy dog".split()
   ['the', 'quick', 'brown', 'fox', 'jumps', 'over', 'the', 'lazy', 'dog']
   i = w.index('fox')
   i
   3
   w[i]
   'fox'
   w.index('unicorn')
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   ValueError: 'unicorn' is not in list
   w.count('the')
   2
   # just test membership
   37 in [1, 78, 9, 37, 34, 53]
   True
   78 not in [1, 78, 9, 37, 34, 53]
   True
   u = "jackdaws love my big sphinx of quartz".split()
   u
   ['jackdaws', 'love', 'my', 'big', 'sphinx', 'of', 'quartz']
   del u[3]
   ['jackdaws', 'love', 'my', 'sphinx', 'of', 'quartz']    
   # remove elements by value rather than position
   u.remove('jackdaws')
   u
   ['love', 'my', 'sphinx', 'of', 'quartz']
   # the equivalent to remove using the index:
   del u[u.index('quartz')]
   u
   ['love', 'my', 'sphinx', 'of']
   u.remove('pyramid')
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   ValueError: list.remove(x): x not in list

   # insertion seq.insert(index, item)
   a = "I accidentally the whole universe".split()
   a.insert(2, "destroyed")
   a
   ['I', 'accidentally', 'destroyed', 'the', 'whole', 'universe']
   # use join method on a space seperator
   ' '.join(a)
   'I accidentally destroyed the whole universe'


growing lists
-------------

.. code-block:: python

   # concatenate lists
   m = [2, 1, 3]
   n = [4, 7, 11]
   k = m + n
   k
   [2, 1, 3, 4, 7, 11]
   # using assignment operator
   k += [18, 29, 47]
   [2, 1, 3, 4, 7, 11, 18, 29, 47]
   # or use the extend method
   k.extend([76, 129, 199])
   k
   [2, 1, 3, 4, 7, 11, 18, 29, 47, 76, 129, 199]
   # these techniques work with any iterable series on the righthand side


reversing and sorting lists
---------------------------

.. code-block:: python

   >>> g = [1, 11, 21, 1211, 112111]
   >>> g.reverse()
   >>> g
   [112111, 1211, 21, 11, 1]
   >>> d = [5, 17, 41, 29, 71, 149, 3299, 7, 13, 67]
   >>> d.sort()
   >>> d
   [5, 7, 13, 17, 29, 41, 67, 71, 149, 3299]
   >>> d.sort(reverse=True)
   >>> d
   [3299, 149, 71, 67, 41, 29, 17, 13, 7, 5]
   # key argument to sort() method accepts a function for producing a sort key from an item
   # e.g. sort words in order of length
   >>> h = 'not perplexing do handwriting family where I illegibly know doctors'.split()
   >>> h
   ['not', 'perplexing', 'do', 'handwriting', 'family', 'where', 'I', 'illegibly', 'know', 'doctors']
   >>> h.sort(key=len)
   >>> h
   ['I', 'do', 'not', 'know', 'where', 'family', 'doctors', 'illegibly', 'perplexing', 'handwriting']
   >>> ' '.join(h)
   'I do not know where family doctors illegibly perplexing handwriting'
   # sorted() built-in function sorts any iterable series and returns a list
   >>> x = [4, 9, 2, 1]
   >>> y = sorted(x)
   >>> y
   [1, 2, 4, 9]
   >>> x
   [4, 9, 2, 1]
   # reversed() built-in function reverses any iterable series
   >>> p = [9, 3, 1, 0]
   >>> q = reversed(p)
   >>> q
   <list_reverseiterator object at 0x7ffa18086e48>
   >>> list(q)
   [0, 1, 3, 9]
   # reversed returns an iterator so you need to convert it to a list (explained later)
   # these two sort methods will work on any finite iterable source object


dictionary
----------

.. code::

   # key objects must be immutable! e.g. Str, numbers and tuples NOT lists!
   # value objects can be mutable
   # the order in dictionaries are random, don't rely on it
   # dict() constructor accepts:
   # iterable series of key-vlaue 2-tuples
   # keyword arguments - requires keys are valid Python identifiers
   # a mapping such as another dict
   names_and_ages=[('Alice', 32), ('Bob', 48), ('Charlie', 28), ('Daniel', 33)]
   >>> d = dict(names_and_ages)
   >>> d
   {'Charlie': 28, 'Daniel': 33, 'Alice': 32, 'Bob': 48}
   >>> phonetic = dict(a='alfa', b='bravo', c='charlie', d='delta', e='echo', f='foxtrot')
   >>> phonetic
   {'b': 'bravo', 'a': 'alfa', 'c': 'charlie', 'f': 'foxtrot', 'd': 'delta', 'e': 'echo'}
   # copying dictionaries is also shallow
   e = d.copy()
   e = dict(d) 
   # Extend a dictionary with update()
   f.update(g)
   for key in colors:
       print("{key} = > {value}".format(key=key, value=colors[key]))
   # if you want to iterate only over the values use values()
   for value in colors.values():
       print(value)
   # there is no efficient way to get the key corresponding to a value!!
   for key in colors.keys(): # iterable view onto keys - not often needed
       print(key)
   # adding to a dictionary and using prettyprint
   >>> m = {'H': [1, 2, 3],
   ...      'He': [3, 5],
   ...      'Li': [6, 7],
   ...      'Be': [7, 9, 10],
   ...      'B': [10, 11],
   ...      'C': [11, 12, 13, 14]}
   >>> m['H'] += [4, 5, 6, 7]
   >>> m
   {'B': [10, 11], 'C': [11, 12, 13, 14], 'Be': [7, 9, 10], 'He': [3, 5], 'Li': [6, 7], 'H': [1, 2, 3, 4, 5, 6, 7]}
   >>> m['H'] = [13, 14, 15]
   >>> m['N'] = [13, 14, 15]
   >>> m
   {'B': [10, 11], 'C': [11, 12, 13, 14], 'N': [13, 14, 15], 'Be': [7, 9, 10], 'He': [3, 5], 'Li': [6, 7], 'H': [13, 14, 15]}
   >>> from pprint import pprint as pp
   >>> pp(m)
   {'B': [10, 11],
    'Be': [7, 9, 10],
    'C': [11, 12, 13, 14],
    'H': [13, 14, 15],
    'He': [3, 5],
    'Li': [6, 7],
    'N': [13, 14, 15]}
   
   

set
---

set is an unordered collection of unique, immutable objects
looks like a dict but each item is a single object

.. note:: empty {} create a dict so for empty set use the set() constructor

.. code::
   
   e = set()
   e
   set()
   >>> s = set([2, 4, 16, 64, 4096, 65536, 262144])
   >>> s
   {4096, 64, 2, 65536, 4, 262144, 16}
   >>> for x in s: # order is arbitrary
   ...     print(x)
   ... 
   4096
   64
   2
   65536
   4
   262144
   16
   >>> 
   # duplicates are discarded!
   # so often used to remove duplicates - not order preserving
   >>> t = [1, 4, 2, 1, 7, 9, 9]
   >>> set(t)
   {1, 2, 4, 9, 7}
   >>> for x in s:
   ...     print(x)
   ... 
   4096
   64
   2
   65536
   4
   262144
   16
   >>> 3 in s
   False
   >>> 3 not in s
   True
   >>> s.add(3)
   >>> s
   {4096, 64, 2, 65536, 4, 262144, 3, 16}
   >>> s.add(4)
   >>> s
   {4096, 64, 2, 65536, 4, 262144, 3, 16}
   >>> s.update([5, 55, 555])
   >>> s
   {4096, 64, 2, 65536, 4, 262144, 3, 5, 555, 16, 55}
   >>> s.remove(2)
   >>> s
   {4096, 64, 65536, 4, 262144, 3, 5, 555, 16, 55}
   >>> s.remove(2)
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   KeyError: 2
   # discard doesn't throw an error if item not present and so always succeeds
   >>> k.discard(2)
   # shallow copies	   
   .copy()
   >>> n
   {4096, 64, 65536, 262144, 4, 3, 5, 555, 16, 55}
   >>> m = set(n)
   >>> m
   {4096, 64, 65536, 262144, 4, 3, 5, 555, 16, 55}

   # set algebra showcase
   >>> blue_eyes = {'Olivia', 'Harry', 'Lily', 'Jack', 'Amelia'}
   >>> blond_hair = {'Harry', 'Jack', 'Amelia', 'Mia', 'Joshua'}
   >>> can_smell_hidrogencyanide = {'Harry', 'Amelia'}
   >>> taste_ptc = {'Harry' , 'Lily', 'Amelia', 'Lola'}
   >>> o_blood = {'Mia', 'Josjua', 'Lily', 'Olivia'}
   >>> b_blood = {'Amelia', 'Jack'}
   >>> a_blood = {'Harry'}
   >>> ab_blood = {'Joshua', 'Lola'}
   # find people with blond hair, blue eyes or both -> use union method
   # is commutative
   >>> blue_eyes.union(blond_hair)
   {'Amelia', 'Mia', 'Harry', 'Olivia', 'Joshua', 'Jack', 'Lily'}
   >>> blue_eyes.union(blond_hair) == blond_hair.union(blue_eyes)
   True
   # find people with blond hair AND blue eyes -> use intersection method
   >>> blue_eyes.intersection(blond_hair)
   {'Harry', 'Jack', 'Amelia'}
   >>> blue_eyes.intersection(blond_hair) == blond_hair.intersection(blue_eyes)
   True
   # find people with blond hair who don't have blue eyes -> use difference method
   # non-commutative because people who have blue eyes but not blond hair is not the same
   >>> blond_hair.difference(blue_eyes)
   {'Joshua', 'Mia'}
   >>> blond_hair.difference(blue_eyes) == blue_eyes.difference(blond_hair)
   False
   # exclusively blond hair OR blue_eyes but not both -> symmetric_difference() method
   # commutative
   >>> blond_hair.symmetric_difference(blue_eyes)
   {'Mia', 'Olivia', 'Joshua', 'Lily'}
   >>> blond_hair.symmetric_difference(blue_eyes) == blue_eyes.symmetric_difference(blond_hair)
   True
   # 3 predicates to tell us about the relationship between sets
   # check is one set is a subset of a set -> issubset() method
   >>> can_smell_hidrogencyanide.issubset(blond_hair)
   True
   # test if people who can taste ptc can also taste hydrogencyanide -> use issuperset() method
   >>> taste_ptc.issuperset(can_smell_hidrogencyanide)
   True
   # test that sets have no members in common -> isdisjoint() method
   >>> a_blood.isdisjoint(o_blood)
   True
   

collection protocols
--------------------

+------------------+-------------------------------------------+
| Protocol         | Implementing Collections                  |
+==================+===========================================+
| Container        | str, list, range, tuple, set, bytes, dict |
+------------------+-------------------------------------------+
| Sized            | str, list, range, tuple, set, bytes, dict |
+------------------+-------------------------------------------+
| Iterable         | str, list, range, tuple, set, bytes, dict |
+------------------+-------------------------------------------+
| Sequence         | str, list, range, tuple, set, bytes       |
+------------------+-------------------------------------------+
| Mutable Sequence | list                                      |
+------------------+-------------------------------------------+
| Mutable Set      | set                                       |
+------------------+-------------------------------------------+
| Mutable Mapping  | dict                                      |
+------------------+-------------------------------------------+

Protocols
 • To implement a protocol, objects must support certain operations.
 • Most collections implement container , sized and iterable.
 • All except dict are sequences.

Container Protocol
 • Membership testing using in and not in

Sized Protocol
 • Determine number of elements with len(s)

Iterable Protocol
 • Can produce and iterator with iter(s)

   .. code-block:: python
     
      for item in iterable:
          do_something(item)

Sequence Protocol

 • Retrieve elements by index
   `item = seq[index]`

 • Find items by value
   `index = seq.index(item)`
 
 • Count items
   `num = seq.count(item)`

 • Produce a reversed sequence
   `r = reversed(seq)`

* Collections Summary
  - Tuples are immutable sequence types
  - Literal syntax: optional parentheses around a comma separated list
  - Single element tuples must use trailing comma
* Tuple unpacking - return values and idiomatic swap
  - Strings are immutable sequence types of Unicode codepoints
  - String concatenation is most efficiently performed with join() on an empty separator
  - The partition() method is a useful and elegant string parsing tool.
  - The format() method provides a powerful way of replacing placeholders with values.
* Ranges represent integer sequences with regular intervals
  - Ranges are arithmetic progressions
  - The enumerate() function is often a superior alternative to range()
* Lists are heterogeneous mutable sequence types
  - Negative indexes work backwards from the end.
  - Slicing allows us to copy all or part of a list.
  - The full slice is a common idiom for copying lists, although the copy() method and list() constructor are less obscure.
  - List (and other collection) copies are shallow.
  - List repetition is shallow.
* Dictionaries map immutable keys to mutable values
  - Iteration and membership testing is done with respect to the keys.
  - Order is arbitrary
  - The keys(), values() and items() methods provide views onto different aspects of a dictionary, allowing convenient iteration.
* Sets store an unordered collection of unique elements
  - Sets support powerful and expressive set algebra operations and predicates.
* Protocols such as iterable, sequence and container characterise the collections.

handling exceptions
===================

**Raise** an exception to interrupt program flow
**Handle** an exception to resume control
**Unhandled exceptions** will terminate the program
**exception objects** contain information about the exceptional event

exception and control flow
--------------------------

.. code-block:: python
 
   '''A module for demonstrating exceptions.'''
   
   def convert(s):
       '''Convert to an integer.'''
       x = int(s) # int contstructor will raise an exception upon entering a string
       return x
   # type of exception object
   # ValueError: invalid literal for int() with base 10: 'somestring'


handling raised exceptions
--------------------------

.. code-block:: python

   def convert(s):
       '''Convert a string to an integer.'''
       try: # try block contains code that can raise an exception
           x = int(s) # print("Conversion succeeded! x = ", x) #  the print won't execute when the exception is raised
       except ValueError: # except block performs error handling in the event an exception is raised
           # print("conversion failed!")
           x = -1
       return x

Trying to convert a list with numbers will raise another exception
`convert([4, 5, 6])`
TypeError: int() argument must be a string, a bytes-like object or a number, not 'list'

.. note:: the except statement excepts a tuple of exception types


.. code-block:: python

   def convert(s):
       '''Convert a string to an integer.'''
       x = -1
       try:
           x = int(s)
           print("Conversion succeeded! x =", x)
       except (ValueError, TypeError):
           print("Conversion failed!")
       return x


programmer errors
-----------------

IndentationError, SyntaxError and NameError are programmer errors and should be corrected during development

you can use pass when you have an empty except block

.. code-block:: python

   def convert(s):
       '''Convert a string to an integer.'''
       x = -1
       try:
           x = int(s)
       except (ValueError, TypeError):
           pass
       return x

when you want to interrogate the exception object for more detail use as

.. code-block:: python

   import sys
   
   def convert(s):
       '''Convert a string to an integer.'''
       try:
           return int(s)
       except (ValueError, TypeError) as e:
           print("Conversion error: {}".format(str(e)), file=sys.stderr)
           return -1


re-raising exceptions
---------------------

.. code-block:: python

   from math import log
   import sys
   
   
   def convert(s):
       '''Convert a string to an integer.'''
       try:
           return int(s)
       except (ValueError, TypeError) as e:
           print("Conversion error: {}".format(str(e)), file=sys.stderr)
           raise # instead of returning an unpythonic error code emit the error msg and reraise the exception object
   
   
   def string_log(s):
       v = convert(s)
       return log(v)

exceptions as APIs
------------------

Callers need to know what exceptions to expect and when

.. code-block:: python

   def sqrt(x):
       '''Compute square roots using the method of Heron of Alexandria.
   
       Args:
           x: The number for which the square root is to be computed.
   
       Returns:
           The square root of x.
   
       Raises:
           ValueError: If x is negative.
       '''
   
       if x < 0:
           raise ValueError("Cannot compute square root of negative number {}".format(x))
   
       guess = x
       i = 0
       while guess * guess != x and i < 20:
           guess = (guess + x / guess) / 2.0
           i += 1
       return guess
   
   
   def main():
       try:
           print(sqrt(9))
           print(sqrt(2))
           print(sqrt(-1))
           print("This is never printed.")
       except ValueError as e:
           print(e, file=sys.stderr)
   
       print("Program execution continues normally here.")
   
   
   if __name__ == '__main__':
       main()

.. note:: it's usually not worth checking types, this can limit your functions unnecessarily

.. note:: it's Easier to Ask Forgiveness than Permission (example 2)

example1

.. code-block:: python

   import os
   
   p = '/path/to/datafile.dat'
   # only check for existence, what if file is garbage, what if path refers to dir instead of file
   
   if os.path.exists(p):
       process_file(p)
   else:
       print('No such file as {}'.format(p))
   
example2

.. code-block:: python
   
   p = '/path/to/datafile.dat'
   
   # no checks in advance 
   try:
       process_file(f)
   except OSError as e:
       print('Could not process file because {}'.format(str(e)))

Error codes require interspersed, local handling
Error codes are silent by default
Exceptions allow centralized, non-local handling
Exceptions require explicit handling

so we work with exceptions in python


clean-up actions
----------------

try...finally lets you clean up whether an exception occurs or not

.. code-block:: python

   import os
   
   def make_at(path, dir_name):
       original_path = os.getcwd() 
       os.chdir(path)
       os.mkdir(dir_name) # if this fails then
       os.chdir(original_path) # this won't happen

.. note:: finally-block is executed no matter how the try-block exits

.. code-block:: python

   import os
   import sys
   
   def make_at(path, dir_name):
       original_path = os.getcwd()
       try:
           os.chdir(path)
           os.mkdir(dir_name)
       except OSError as e:
           print(e, file=sys.stderr)
           raise
       finally:
           os.chdir(original_path) # runs even if OSError is thrown and handled

platform-specific code
----------------------

.. code-block:: python

   """keypress - A module for detecting a single keypress."""
   
   try:
       import msvcrt
   
       def getkey():
           """Wait for a keypress and return a single character string."""
           return msvcrt.getch()
   
   except ImportError: # when importing the windos module fails, the error is silenced explicitely, if this fails it will not be silenced
   
       import sys
       import tty
       import termios
   
       def getkey():
           """Wait for a keypress and return a single character string."""
           fd = sys.stdin.fileno()
           original_attributes = termios.tcgetattr(fd)
           try:
               tty.setraw(sys.stdin.fileno())
               ch = sys.stdin.read(1)
           finally:
               termios.tcsetattr(fd, termios.TCSADRAIN, original_attributes)
           return ch
   
       # If either of the Unix-specific tty or termios are not found,
       # we allow the ImportError to propagate from here

Comprehensions, iterables and generators
========================================

list comprehensions
-------------------

[ expr(item) for item in iterable ]
for each item in the iterable object on the right evaluate the expression on the left

.. code-block:: python

   ['Why', 'sometimes', 'I', 'have', 'believed', 'as', 'many', 'as', 'six', 'possible', 'things', 'before', 'breakfast']
   [len(word) for word in words]
   [3, 9, 1, 4, 8, 2, 4, 2, 3, 8, 6, 6, 9]
   from math import factorial
   f = [ len(str(factorial(x))) for x in range(20) ]
   f
   [1, 1, 1, 1, 2, 3, 3, 4, 5, 6, 7, 8, 9, 10, 11, 13, 14, 15, 16, 18]
   type(f)
   #<class 'list'>


set comprehensions
------------------

{ expr(item) for item in iterable }

.. code-block:: python

   { len(str(factorial(x))) for x in range(20) }
   {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 13, 14, 15, 16, 18}


dictionary comprehensions
-------------------------

{ key_expr:value_expr for item in iterable }

If we want both the keys and the values we should use the items() method and then use tuple unpacking country, capital

.. code-block:: python

   country_to_capital = {'United Kingdom': 'London',
                         'Brazil': 'Brazilia',
                         'Morocco': 'Rabat',
                         'Sweden': 'Stockholm'}
   from pprint import pprint as pp
   capital_to_country = { capital: country for country, capital in country_to_capital.items() }
   pp(capital_to_country)
   {'Brazilia': 'Brazil',
    'London': 'United Kingdom',
    'Rabat': 'Morocco',
    'Stockholm': 'Sweden'}

.. note:: duplicates: later keys overwrite earlier keys

e.g. only key  that begins with first letter x[0] store that word, duplicates will be gone

.. code-block:: python

   words = ["hi", "hello", "foxtrot", "hotel"]
   { x[0]: x for x in words }
   {'h': 'hotel', 'f': 'foxtrot'}


filtering predicates
--------------------

there is an optional filtering clause

[ expr(item) for item in iterable if predicate(item) ]

.. code-block:: python

   from math import sqrt
   
   def is_prime(x):
       if x < 2:
           return False
       for i in range(2, int(sqrt(x)) + 1):
           if x % i == 0:
               return False
       return True
   
   [x for x in range(101) if is_prime(x)]
   [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97]

   # numbers with only 3 divisors

   from pprint import pprint as pp
   prime_square_divisors = { x*x:(1, x, x*x) for x in range(101) if is_prime(x)}
   pp(prime_square_divisors)
   {4: (1, 2, 4),
    9: (1, 3, 9),
    25: (1, 5, 25),
    49: (1, 7, 49),
    121: (1, 11, 121),
    169: (1, 13, 169),
    289: (1, 17, 289),
    361: (1, 19, 361),
    529: (1, 23, 529),
    841: (1, 29, 841),
    961: (1, 31, 961),
    1369: (1, 37, 1369),
    1681: (1, 41, 1681),
    1849: (1, 43, 1849),
    2209: (1, 47, 2209),
    2809: (1, 53, 2809),
    3481: (1, 59, 3481),
    3721: (1, 61, 3721),
    4489: (1, 67, 4489),
    5041: (1, 71, 5041),
    5329: (1, 73, 5329),
    6241: (1, 79, 6241),
    6889: (1, 83, 6889),
    7921: (1, 89, 7921),
    9409: (1, 97, 9409)}


iteration protocols
-------------------


**iterable protocol**
iterable objects can be passed to the built-in `iter()` function to get an iterator 

iterator = iter(iterable)                                                                                                                                               
**iterator protocol**
Iterator objects can be passed to the built-in `next()` function to fetch the next item.
item = next(iterator)


.. code-block:: python

   iterable = ['Spring', 'Summer', 'Autumn', 'Winter']
   iterator = iter(iterable)
   next(iterator)
   'Spring'
   next(iterator)
   'Summer'
   next(iterator)
   'Autumn'
   next(iterator)
   'Winter'
   next(iterator)
   # Traceback (most recent call last):
   #  File "<stdin>", line 1, in <module>
   #  StopIteration


   def first(iterable):
       iterator = iter(iterable)
       try:
           return next(iterator)
       except StopIteration:
           raise ValueError("iterable is empty")
   
   first(["1st", "2nd", "3rd"])
   '1st'
   first(["1st", "2nd", "3rd"])
   '1st'
   first(set())
   #Traceback (most recent call last):
   #  File "<stdin>", line 4, in first
   #StopIteration
   #
   #During handling of the above exception, another exception occurred:
   #
   #Traceback (most recent call last):
   #  File "<stdin>", line 1, in <module>
   #  File "<stdin>", line 6, in first
   #ValueError: iterable is empty

generators
----------

specify iterable sequences
 - all generators are iterators

are lazily evaluated
 - they only computate the next value on demand

can model infinite sequences
 - such as data streams from a sensor with no definite end

are composable into pipelines
 - for natural stream processing


.. code-block:: python

   def gen123():
       yield 1
       yield 2
       yield 3
       # return
       # implicit return at the end

   g = gen123()
   g
   # <generator object gen123 at 0x7f3f662e8fc0>
   # pass the generator to the function
   next(g)
   1
   next(g)
   2
   next(g)
   3
   # now StopIteration exception
   #
   # Traceback (most recent call last):
   #   File "<stdin>", line 1, in <module>
   #   StopIteration
   for v in gen123(): 
       print(v)
   
   1
   2
   3
   # remember each call to function creates a new generator object
   h = gen123()
   i = gen123()
   h
   #generator object gen123 at 0x7f3f66302048>
   i
   #generator object gen123 at 0x7f3f663020a0>

.. code-block:: python

   def gen246():
       print("about to yield 2")
       yield 2
       print("about to yield 4")
       yield 4
       print("about to yield 6")
       yield 6
   
   g = gen246() # generator = created and returned but non of the code has yet been executed
   next(g)
   "about to yield 2"
   2
   next(g) # generator resumes at the point we left off
   "about to yield 4"                                                                                    
   2              
   next(g)
   "about to yield 6"
   6

stateful generators
-------------------

- Generators resume execution
- Can maintain state in local variables
- Complex control flow
- Lazy evaluation


take generator

.. code-block:: python

   def take(count, iterable):
       """Take items from the front of an iterable.
   
       Args:
           count: maximum number of items to retrieve
           iterable: the source series
   
       Yields:
            at most 'count' items from 'iterable'
       """
   
       counter = 0
       for item in iterable:
           if counter == count:
               return # end sequence when we reach specified count
               # return raises StopIteration which is caught internally by the for loop in run_take()
           counter += 1 # how many items have been yielded so far
           yield item # contains a generator bc it has at least one yield statement
   
   
   def run_take(): # generators are lazy and only generate values on request
       items = [2, 4, 6, 8, 10]
       for item in take(3, items):  # take(count, iterable) # return raises StopIteration which is caught by
           print(item)
   
   
   if __name__ == "__main__":
           run_take()


distinct generator

.. code-block:: python

   # 2nd generator
   def distinct(iterable):
       """Return unique items by eliminating duplicates
   
       Args:
           iterable: source series
   
       Yields:
           Unique elements in order from 'iterable'
       """
   
       seen = set()
       for item in iterable:
           if item in seen:
               continue # finishes current iteration of the loop an begins next iteration immediately!
               # execution will be transferred back to the for statement to begin next iteration
           yield item
           seen.add(item) # next call from for loop in run_distinct() will resume here with remembered
           # item 5 just yielded
           # and then moves up to beginning of for loop again where item is reassigned to second value 7
   
   def run_distinct():
       items = [5, 7, 7, 6, 5, 5]
       for item in distinct(items):
           print(item)
   
   if __name__ == '__main__':
       run_distinct()

both combined in pipeline

.. code-block:: python

   def take(count, iterable):
       counter = 0
       for item in iterable:
           if counter == count:
               return # end sequence when we reach specified count
               # return raises StopIteration which is caught internally by the for loop in run_take()
           counter += 1 # how many items have been yielded so far
           yield item # contains a generator bc it has at least one yield statement
   
   
   def run_take(): # generators are lazy and only generate values on request
       items = [2, 4, 6, 8, 10]
       for item in take(3, items):  # take(count, iterable) # return raises StopIteration which is caught by
           print(item)
   
   # 2nd generator
   def distinct(iterable):
       seen = set()
       for item in iterable:
           if item in seen:
               continue # finishes current iteration of the loop an begins next iteration immediately!
               # execution will be transferred back to the for statement to begin next iteration
           yield item
           seen.add(item) # next call from for loop in run_distinct() will resume here with first item 5
           # and then moves up to beginning of for loop again with second item 7
   
   def run_distinct():
       items = [5, 7, 7, 6, 5, 5]
       for item in distinct(items):
           print(item)
   
   def run_pipeline():
       items = [5, 7, 7, 6, 5, 5]
       for item in take(3, distinct(items)): # distinct must run first to produce the iterator object argument for take
           print(item)
   
   if __name__ == '__main__':
       run_pipeline()


laziness and infinite
^^^^^^^^^^^^^^^^^^^^^

• Just in Time Computation
• Infinite (or large) sequences
  • sensor readings
  • mathematical series
  • massive files

.. code-block:: python

   def lucas():
       yield 2
       a = 2
       b = 1
       while True: # infinite while loop
           yield b
           a, b = b, a + b
   
   for x in lucas():
       print(x)
   # this will run forever until you pc runs out of memory but demonstrates nicely

generator comprehensions
------------------------

- similar syntax to list comprehensions
- create a generator object
- concise
- lazy evaluation

(expr(item) for item in iterable)

.. code-block:: python

   million_squares = (x*x for x in range(1, 1000001)) # creates a generator object
   list(million_squares) # force evaluation of the generator by using it to create a list
   list(million_squares)
   [] # repeating it = empty, generators are single use objects!
   # each time we call a generator function, we create a generator object
   sum(x*x for x in range(1, 1000001)) # sum of first 10mil squares, using a list comprehencsion would take 400MB!
   333333833333500000

   sum(x*x for x in range(1, 1000001) if x % 17) # with optional if condition
   313726019609411764


.. note:: we didn't have to use extra parentheses to put a generator comprehension wihtin sum(), this improves readability

using itertools
---------------

list of iteration tools in python: https://docs.python.org/3/library/itertools.html 

using islice and count

.. code-block:: python

   from itertools import islice, count
   from math import sqrt
   
   
   def is_prime(x):
       if x < 2:
           return False
       for i in range(2, int(sqrt(x)) + 1):
           if x % i == 0:
               return False
       return True
   
   # do this thousand_primes = islice(all_primes, 1000) but how to generate all primes
   # ranges must always be finite, we need an open ended version of range and that is what count() does
   # thousand_primes = islice((x for x in count() if is_prime(x)), 1000) # with islice() like with lists
   
   sum(islice((x for x in count() if is_prime(x)), 1000))
   3682913


using any(or) and all(and) for iterable series of bool values

.. code-block:: python

   any([False, False, True])
   True
   all([False, False, True])
   False
   
   # Are there any prime numbers between 1328 and 1361?
   any(is_prime(x) for x in range(1328, 1361))
   False
   
   # title() converts first character to uppercase
   # check if all city names have capital letters
   all(name == name.title() for name in ['London', 'New York', 'Sydney'])
   True
   
   # syncronize iterations over 2 iterable series
   # eg two  series of temperature data
   sunday = [12, 14, 15, 15, 17, 21, 22, 22, 23, 22, 20, 18]
   monday = [13, 14, 14, 14, 16, 20, 21, 22, 22, 21, 19, 17]
   # bind them in pairs of corresponding readings
   for item in zip(sunday, monday):
       print(item)
   
   (12, 13)
   (14, 14)
   (15, 14)
   (15, 14)
   (17, 16)
   (21, 20)
   (22, 21)
   (22, 22)
   (23, 22)
   (22, 21)
   (20, 19)
   (18, 17)
   
   # zip yields tuples when iterated
   # we can take advantage of this with tuple unpacking in the for loop
   for sun, mon in zip(sunday, monday):
       print("average =", (sun + mon) / 2)
   
   average = 12.5
   average = 14.0
   average = 14.5
   average = 14.5
   average = 16.5
   average = 20.5
   average = 21.5
   average = 22.0
   average = 22.5
   average = 21.5
   average = 19.5
   average = 17.5
   
   tuesday = [2, 2, 3, 7, 9, 10, 9, 8, 8]
   
   for temps in zip(sunday, monday, tuesday):
       print("min={:4.1f}, max={:4.1f}, average={:4.1f}".format(min(temps), max(temps), sum(temps) / len(temps)))
   """ 
   min= 2.0, max=13.0, average= 9.0
   min= 2.0, max=14.0, average=10.0
   min= 3.0, max=15.0, average=10.7
   min= 7.0, max=15.0, average=12.0
   min= 9.0, max=17.0, average=14.0
   min=10.0, max=21.0, average=17.0
   min= 9.0, max=22.0, average=17.3
   min= 8.0, max=22.0, average=17.3
   min= 8.0, max=23.0, average=17.7
   """
   # now we want one long temperature series for sunday monday and thuesday 
   # we can then lazily concatenate iterables using itertools chain
   # this is different from simply concatenating 3 lists into a new list
   # we have no memory impact of data duplication
   from itertools import chain
   temperatures = chain(sunday, monday, tuesday)

   all(t > 0 for t in temperatures)
   temperatures = chain(sunday, monday, tuesday)
   True

   # following shows generator functions, generator expressions, predicate functions and for loops
   def lucas():
       yield 2
       a = 2
       b = 1
       while True: # infinite while loop
           yield b
           a, b = b, a + b

   for x in (p for p in lucas() if is_prime(p)):
       print(x)

   2
   3
   7
   11
   29
   47
   199
   521
   2207
   3571
   9349
   3010349
   54018521
   370248451
   6643838879
   119218851371
   5600748293801
   688846502588399
   32361122672259149


   """ itertools.chain(*iterables)
   Make an iterator that returns elements from the first iterable until it is exhausted, then proceeds to the next iterable, until all of the iterables are exhausted. Used for treating consecutive sequences as a single sequence
   """


https://docs.python.org/3/library/itertools.html#itertools.chain

summary comprehensions generators
---------------------------------

- Comprehensions
  - Comprehensions are a concise syntax for describing lists, sets and dictionaries.
  - Comprehensions operate on an iterable source object and apply an optional predicate filter and a mandatory expression, both of which are usually in terms of the current item.
  - Iterables are objects over which we can iterate item by item.
  - We retrieve an iterator from an iterable object using the built-in iter() function.
  - Iterators produce items one-by-one-from the underlying iterable series each time they are passed to the built-in next() function 

- Generators
  - Generator functions allow us to describe series using imperative code.
  - Generator functions contain at least one use of the yield keyword.
  - Generators are iterators. When advanced with next() the generator starts or resumes execution up to and including the next yield.
  - Each call to a generator function creates a new generator object.
  - Generators can maintain explicit state in local variables between iterations.
  - Generators are lazy, and so can model infinite series of data.
  - Generator expressions have a similar syntactic form to list comprehensions and allow for a more declarative and concise way of creating generator objects.

list of iteration tools in python: https://docs.python.org/3/library/itertools.html


classes
=======

self: the first argument to all instance methods
__init__() instance method for initializing new objects

.. warning:: __init__() is an initializer, not a constructor, self is similar to this. __init__() is to configure an object that already exists by the time it is called

why self._number?
 
 1. avoid name clash with number()
 2. implementation details of objects start with _

.. note:: using objects of different types through a common interface = polymorphism

.. code-block:: python

   """Model for aircraft flights."""

   class Flight:
       """A flight with a particular passenger aircraft."""

       def __init__(self, number, aircraft):
           if not number[:2].isalpha():
               raise ValueError("No airline code in '{}'".format(number))

           if not number[:2].isupper():
               raise ValueError("Invalid airline code '{}'".format(number))

           if not (number[2:].isdigit() and int(number[2:]) <= 9999):
               raise ValueError("Invalid route number '{}'".format(number))

           self._number = number
           self._aircraft = aircraft

           rows, seats = self._aircraft.seating_plan()
           self._seating = [None] + [ {letter:None for letter in seats} for _ in rows ]

       def number(self):
           return self._number

       def airline(self):
           return self._number[:2]

       def aircraft_model(self):
           return self._aircraft.model()

       def allocate_seat(self, seat, passenger):
           """Allocate a seat to a passenger.

           Args:
               seat: A seat designator such as '12C' or '21F'.
               passenger: The passenger name.

           Raises:
               ValueError: If the seat is unavailable.
           """
           rows, seat_letters = self._aircraft.seating_plan()

           letter = seat[-1]
           if letter not in seat_letters:
               raise ValueError("Invalid seat letter {}".format(letter))

           row_text = seat[:1]
           try:
               row = int(row_text)
           except ValueError:
               raise ValueError("Invalid seat row {}".format(row_text))

           if row not in rows:
               raise ValueError("Invalid row number {}".format(row))

           if self._seating[row][letter] is not None:
               raise ValueError("Seat {} is already occupied.".format(seat))

           self._seating[row][letter] = passenger

       def _parse_seat(self, seat):
           """Parse a seat designator into a valid row and letter.

           Args:
               seat: A seat designator such as 12F

           Returns:
               A tuple containing an integer and a string for row and seat.
           """
           row_numbers, seat_letters = self._aircraft.seating_plan()

           letter = seat[-1]
           if letter not in seat_letters:
               raise ValueError("Invalid seat letter {}".format(letter))

           row_text = seat[:-1]
           try:
               row = int(row_text)
           except ValueError:
               raise ValueError("Invalid seat row {}".format(row_text))

           if row not in row_numbers:
               raise ValueError("Invalid row number {}".format(row))

           return row, letter

       def allocate_seat(self, seat, passenger):
           """Allocate a seat to a passenger.

           Args:
               seat: A seat designator such as '12C' or '21F'.
               passenger: The passenger name.

           Raises:
               ValueError: If the seat is unavailable.
           """
           row, letter = self._parse_seat(seat)

           if self._seating[row][letter] is not None:
               raise ValueError("Seat {} already occupied".format(seat))

           self._seating[row][letter] = passenger

       def relocate_passenger(self, from_seat, to_seat):
           """Relocate a passenger to a different seat.

           Args:
               from_seat: The existing seat designator for the
                          passenger to be moved.

               to_seat: The new seat designator.
           """

           from_row, from_letter = self._parse_seat(from_seat)
           if self._seating[from_row][from_letter] is None:
               raise ValueError("No passenger to relocate in seat {}".format(from_seat))

           to_row, to_letter = self._parse_seat(to_seat)
           if self._seating[to_row][to_letter] is not None:
               raise ValueError("Seat {} already occupied".format(to_seat))

           self._seating[to_row][to_letter] = self._seating[from_row][from_letter]
           self._seating[from_row][from_letter] = None

       def num_available_seats(self):
           return sum( sum(1 for s in row.values() if s is None)
                       for row in self._seating
                       if row is not None)

       def make_boarding_cards(self, card_printer):
           for passenger, seat in sorted(self._passenger_seats()):
               card_printer(passenger, seat, self.number(), self.aircraft_model())

       def _passenger_seats(self):
           """An iterable series of passenger seating allocations."""
           row_numbers, seat_letters = self._aircraft.seating_plan()
           for row in row_numbers:
               for letter in seat_letters:
                   passenger = self._seating[row][letter]
                   if passenger is not None:
                       yield (passenger, "{}{}".format(row, letter))


   class Aircraft:
         def __init__(self, registration):
             self._registration = registration

         def registration(self):
             return self._registration

         def num_seats(self):
             rows, row_seats = self.seating_plan()
             return len(rows) * len(row_seats)


   class AirbusA319(Aircraft):
         def model(self):
             return "Airbus A319"

         def seating_plan(self):
             return range(1, 23), "ABCDEF"


   class Boeing777(Aircraft):
       def model(self):
           return "Boeing 777"

       def seating_plan(self):
           # For simplicity's sake, we ignore complex
           # seating arrangement for first-class
           return range(1, 56), "ABCDEGHJK"


   def make_flights():
         f = Flight("BA758", AirbusA319("G-EUPT"))
         f.allocate_seat('12A', 'Guido van Rossum')
         f.allocate_seat('15F', 'Bjarne Stroustrup')
         f.allocate_seat('15E', 'Anders Hejlsberg')
         f.allocate_seat('1C', 'John McCarthy')
         f.allocate_seat('1D', 'Richard Hickey')

         g = Flight("AF72", Boeing777("F-GSPS"))
         g.allocate_seat('55K', 'Larry Wall')
         g.allocate_seat('33G', 'Yukihiro Matsumoto')
         g.allocate_seat('4B', 'Brian Kernighan')
         g.allocate_seat('4A', 'Dennis Ritchie')

         return f, g


   def console_card_printer(passenger, seat, flight_number, aircraft):
         output = "| Name: {0}"     \
                  "  Flight: {1}"   \
                  "  Seat: {2}"     \
                  "  Aircraft: {3}" \
                  " |".format(passenger, flight_number, seat, aircraft)
         banner = '+' + '-' * (len(output) - 2) + '+'
         border = '|' + ' ' * (len(output) - 2) + '|'
         lines = [banner, border, output, border, banner]
         card = '\n'.join(lines)
         print(card)
         print()


summary classes
---------------

- All types in Python have a 'class'
- Classes define the structure and behavior of an object
- Class is determined when object is created
  - normally fixed for the lifetime
- Classes are the key support for Object-Oriented Programming in Python
- Classes defined using the class keyword followed by CamelCase name
- Class instances created by calling the class as if it were a function
- Instance methods are functions defined inside the class
  - Should accept an object instance called self as the first parameter
- Methods are called using instance.method()
  - Syntactic sugar for passing self instance to method
- The optional __init__() method initialized new instances
  - If present, the constructor calls __init__()
  - __init__() is not the constructor
- Arguments passed to the constructor are forwarded to the initializer

- Instance attributes are created simply by assigning to them
- Implementation details are denoted by a leading underscore
  - There are no public, protected or private access modifiers in Python
- Accessing implementation details can be very useful
  - Especially during development and debugging
- Class invariants should be established in the initializer
  - If the invariants can't be established raise exceptions to signal failure
- Methods can have docstrings, just like regular functions
- Classes can have docstrings
- Even within an object method calls must be preceded with self
- You can have as many classes and functions in a module as you wish
  - Related classes and global functions are usually grouped together this way
- Polymorphism in Python is achieved through duck typing
- Polymorphism in Python does not use shared base classes or interfaces
- Class inheritance is primarily useful for sharing implementation
- All methods are inherited, including special methods like the initializer

- Strings support slicing, because they implement the sequence protocol
- Following the Law of Demeter can reduce coupling
- We can nest comprehensions
- It can sometimes be useful to discard the current item in a comprehension
- When dealing with one-based collections it's often easier just to waste one
- list entry.
- Don't feel compelled to use classes when a simple function will suffice
- Comprehensions or generator expression can be split over multiple lines
- Statements can be split over multiple lines using backslash
  - Use this feature sparingly and only when it improves readability
- Use “Ask! Don’t tell.” to avoid tight coupling between objects

properties
==========

https://www.learnbyexample.org/python-properties/

.. code-block:: python

   class Person():
       def __init__(self, value):
           self.hidden_name = value

       @property
       def name(self):
           print('Getting name:')
           return self.hidden_name

       @name.setter
       def name(self, value):
           print('Setting name to', value)
           self.hidden_name = value

       @name.deleter
       def name(self):
           print('Deleting name')
           del self.hidden_name


.. code-block:: python

   class Person:
       def __init__(self, value):
           self.name = value
   
       @property
       def name(self):
           return self._name
   
       @name.setter
       def name(self, value):
           if not isinstance(value, str):
               raise TypeError('Expected a string')
           self._name = value
   
       @name.deleter
       def name(self):
           raise AttributeError("Can't delete attribute")
   
   p = Person(42)      # Triggers TypeError: Expected a string
   p = Person('Bob')
   print(p.name)       # Prints Bob
   p.name = 42         # Triggers TypeError: Expected a string
   del p.name          # Triggers AttributeError: Can't delete attribute

computed attributes
-------------------

.. code-block:: python

   class Rectangle(object):
       def __init__(self, width, height):
           self.width = width
           self.height = height

       @property
       def area(self):
           return self.width * self.height

extend property functionality
-----------------------------

.. code-block:: python

   class Person():
       def __init__(self, value):
           self.hidden_name = value
   
       @property
       def name(self):
           print('Getting name:')
           return self.hidden_name
   
       @name.setter
       def name(self, value):
           print('Setting name to', value)
           self.hidden_name = value
   
       @name.deleter
       def name(self):
           print('Deleting name')
           del self.hidden_name
   
   class SubPerson(Person):
       @property
       def name(self):
           print('Inside subperson getter')
           return super().name
   
       @name.setter
       def name(self, value):
           print('Inside subperson setter')
           super(SubPerson, SubPerson).name.__set__(self, value)
   
       @name.deleter
       def name(self):
           print('Inside subperson deleter')
           super(SubPerson, SubPerson).name.__delete__(self)


.. code-block:: python

   s = SubPerson('Bob')
   
   # calls the getter
   print(s.name)
   # Prints Inside subperson getter
   # Prints Getting name: Bob
   
   # calls the setter
   s.name = 'Sam'
   # Prints Inside subperson setter
   # Prints Setting name to Sam
   
   # calls the deleter
   del s.name
   # Prints Inside subperson deleter
   # Prints Deleting name


If you only want to redefine one of the methods, it’s not enough to use @property by itself, use code such as the following:

.. code-block:: python

   class SubPerson(Person):
       @Person.name.getter
       def name(self):
           print('Inside subperson getter')
           return super().name


If you just want to redefine the setter, use this code:

.. code-block:: python

   class SubPerson(Person):
       @Person.name.setter
       def name(self, value):
           print('Inside subperson setter')
           super(SubPerson, SubPerson).name.__set__(self, value)

recursion
=========


Solve problems that can be broken up into sub-problems of the same type

- 5! (5 factorial) is 5 * 4 * 3 * 2 * 1 (120)
- `n! = n * (n-1)!`
- **BASE CASE: 1! = 1** -> can be calculated without performing any more factorial function calls

.. note:: The base case acts as the exit condition of the recursion

.. code-block:: python

   def factorial(x):
       # base case
       if x == 1:
           return 1
       else:
           return x * factorial(x-1)


   print(factorial(5))  # 120


.. code-block:: python

   """fibonacci"""
   def fibo(n):
       if n <= 1:
           return n  # returns 0 & 1's
       else:
           return fibo(n-1) + fibo(n-2)


   number = 6
   for i in range(6):
       print(fibo(i))


.. code-block:: python

   def power(x, y):
       if y == 0:
           return 1
       else:
           return x * power(x, y-1)


   print(power(2, 3))  # 8


.. note:: Recursion can also be indirect. One function can call a second, which calls the first, which calls the second, and so on. This can occur with any number of functions


.. code-block:: python

   def is_even(x):
       if x == 0:
           return True
       else:
           return is_odd(x-1)


   def is_odd(x):
       return not is_even(x)  # not! else will also return True when odd


   is_even(9)  # False
   is_even(12) # True
   is_odd(17)  # True


files and resource management
=============================

open(file, mode, encoding)
 - file: path to file (required)
 - mode: read/write/append, binary/text
 - encoding: text encoding

https://docs.python.org/3/library/functions.html#open

+-----------+-----------------------------------------------------------------+
| Character | Meaning                                                         |
+===========+=================================================================+
| 'r'       | open for reading (default)                                      |
+-----------+-----------------------------------------------------------------+
| 'w'       | open for writing, truncating the file first                     |
+-----------+-----------------------------------------------------------------+
| 'x'       | open for exclusive creation, failing if the file already exists |
+-----------+-----------------------------------------------------------------+
| 'a'       | open for writing, appending to the end of the file if it exists |
+-----------+-----------------------------------------------------------------+
| 'b'       | binary mode                                                     |
+-----------+-----------------------------------------------------------------+
| 't'       | text mode (default)                                             |
+-----------+-----------------------------------------------------------------+
| '+'       | open a disk file for updating (reading and writing)             |
+-----------+-----------------------------------------------------------------+
| 'U'       | universal newlines mode (deprecated)                            |
+-----------+-----------------------------------------------------------------+

write() returns the number of codepoints, not the number of characters

.. code-block:: python

   import sys

   def main(filename):
       f = open(filename, mode='rt', encoding='utf-8')
       for line in f:
           print(line)
       f.close()

   if __name__ == '__main__':
       main(sys.argv[1])


zip files
---------

read zip as panda df
^^^^^^^^^^^^^^^^^^^^

.. code:: python

   # import required modules
   import zipfile
   import pandas as pd
     
   # read the dataset using the compression zip
   df = pd.read_csv('test.zip',compression='zip')
     
   # display dataset
   print(df.head())


   import zipfile
   import pandas as pd
     
   # oppen zipped dataset
   with zipfile.ZipFile("test.zip") as z:
      # open the csv file in the dataset
      with z.open("test.csv") as f:
           
         # read the dataset
         train = pd.read_csv(f)
           
         # display dataset
         print(train.head())

json
----

read
^^^^

.. code-block:: python

   # Python program to read JSON
   # from a file
     
   import json
     
   # Opening JSON file
   with open('sample.json', 'r') as openfile:
     
       # Reading from json file
       json_object = json.load(openfile)
     
   print(json_object)
   print(type(json_object))


write
^^^^^

.. code-block:: python

   # Python program to write JSON
   # to a file
   
   
   import json
   
   # Data to be written
   dictionary ={
       "name" : "sathiyajith",
       "rollno" : 56,
       "cgpa" : 8.6,
       "phonenumber" : "9976770500"
   }
   
   # Serializing json
   json_object = json.dumps(dictionary, indent = 4)
   
   # Writing to sample.json
   with open("sample.json", "w") as outfile:
       outfile.write(json_object)

   with open("sample.json", "w") as outfile:
       json.dump(dictionary, outfile)


update
^^^^^^

.. code-block:: python

   # Python program to update
   # JSON
   import json
   
   # JSON data:
   x =  '{ "organization":"GeeksForGeeks",
           "city":"Noida",
           "country":"India"}'
   
   # python object to be appended
   y = {"pin":110096}
   
   # parsing JSON string:
   z = json.loads(x)
   
   # appending the data
   z.update(y)
   
   # the result is a JSON string:
   print(json.dumps(z))
   

   # Python program to update
   # JSON
   import json
    
    
   # function to add to JSON
   def write_json(new_data, filename='data.json'):
       with open(filename,'r+') as file:
             # First we load existing data into a dict.
           file_data = json.load(file)
           # Join new_dat3a with file_data
           file_data.update(new_data)
           # Sets file's current position at offset.
           file.seek(0)
           # convert back to json.
           json.dump(file_data, file, indent = 4)
    
       # python object to be appended
   y = {"emp_name":'Nikhil',
        "email": "nikhil@geeksforgeeks.org",
        "job_profile": "Full Time"
       }
        
   write_json(y)


csv
---

.. code-block:: python

   # importing the csv module
   import csv
     
   # field names
   fields = ['Name', 'Branch', 'Year', 'CGPA']
     
   # data rows of csv file
   rows = [ ['Nikhil', 'COE', '2', '9.0'],
            ['Sanchit', 'COE', '2', '9.1'],
            ['Aditya', 'IT', '2', '9.3'],
            ['Sagar', 'SE', '1', '9.5'],
            ['Prateek', 'MCE', '3', '7.8'],
            ['Sahil', 'EP', '2', '9.1']]
     
   # name of csv file
   filename = "university_records.csv"
     
   # writing to csv file
   with open(filename, 'w') as csvfile:
       # creating a csv writer object
       csvwriter = csv.writer(csvfile)
         
       # writing the fields
       csvwriter.writerow(fields)
         
       # writing the data rows
       csvwriter.writerows(rows)


.. code-block:: python

   # importing the csv module
   import csv
     
   # my data rows as dictionary objects
   mydict =[{'branch': 'COE', 'cgpa': '9.0', 'name': 'Nikhil', 'year': '2'},
            {'branch': 'COE', 'cgpa': '9.1', 'name': 'Sanchit', 'year': '2'},
            {'branch': 'IT', 'cgpa': '9.3', 'name': 'Aditya', 'year': '2'},
            {'branch': 'SE', 'cgpa': '9.5', 'name': 'Sagar', 'year': '1'},
            {'branch': 'MCE', 'cgpa': '7.8', 'name': 'Prateek', 'year': '3'},
            {'branch': 'EP', 'cgpa': '9.1', 'name': 'Sahil', 'year': '2'}]
     
   # field names
   fields = ['name', 'branch', 'year', 'cgpa']
     
   # name of csv file
   filename = "university_records.csv"
     
   # writing to csv file
   with open(filename, 'w') as csvfile:
       # creating a csv dict writer object
       writer = csv.DictWriter(csvfile, fieldnames = fields)
         
       # writing headers (field names)
       writer.writeheader()
         
       # writing data rows
       writer.writerows(mydict)


.. note::

   In csv modules, an optional dialect parameter can be given which is used to define a set of parameters specific to a particular CSV format.
   By default, csv module uses excel dialect which makes them compatible with excel spreadsheets.
   You can define your own dialect using register_dialect method


.. code-block:: python

   csv.register_dialect(
       'mydialect',
       delimiter = ',',
       quotechar = '"',
       doublequote = True,
       skipinitialspace = True,
       lineterminator = '\r\n',
       quoting = csv.QUOTE_MINIMAL)

   csvreader = csv.reader(csvfile, dialect='mydialect')
   # change params
   csvreader = csv.reader(csvfile, delimiter = ';', lineterminator = '\n\n')

large csv files
^^^^^^^^^^^^^^^

.. code-block:: python

   import pandas as pd
   import numpy as np
   import time
     
   # time taken to read data
   s_time_chunk = time.time()
   chunk = pd.read_csv('gender_voice_dataset.csv', chunksize=1000)
   e_time_chunk = time.time()
     
   print("With chunks: ", (e_time_chunk-s_time_chunk), "sec")
   df = pd.concat(chunk)
     
   # data
   df.sample(10)

xml
---

load
^^^^
assets/data.xml

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <users>
       <user data-id="101">
           <nom>Zorro</nom>
           <metier>Danseur</metier>
       </user>
       <user data-id="102">
           <nom>Hulk</nom>
           <metier>Footballeur</metier>
       </user>
       <user data-id="103">
           <nom>Zidane</nom>
           <metier>Star</metier>
       </user>
       <user data-id="104">
           <nom>Beans</nom>
           <metier>Epicier</metier>
       </user>
       <user data-id="105">
           <nom>Batman</nom>
           <metier>Veterinaire</metier>
       </user>
       <user data-id="106">
           <nom>Spiderman</nom>
           <metier>Veterinaire</metier>
       </user>
   </users>


.. code-block:: python

   filename = "./assets/data.xml"
   file = open(filename, "r")
   print (file.read())
   file.close()

lxml
^^^^

.. code-block:: python

   import sys
   # !conda install --yes --prefix {sys.prefix} lxml
   # !{sys.executable} -m pip install numpy
   from lxml import etree
   # I define my source document
   filename = 'assets/data.xml'
   tree = etree.parse(filename)
   # so in tree.xpath("/users/user/name") there are the tags associated with our search
   for user in tree.xpath("/users/user/nom"):
       # I want to display only the content (.text) of these tags /users/user/name
       print(user.text)

   """
   Zorro
   Hulk
   Zidane
   Beans
   Batman
   Spiderman
   """

   tree.xpath("/users/user/nom")[0].text
   # 'Zorro'

   # You can display the attributes of the tags that store this information
   tree = etree.parse(filename)
   for user in tree.xpath("/users/user"):
       print(user.get("data-id"))
   """
   101
   102
   103
   104
   105
   106
   """
   
   # You can refine the display by proposing to display only users whose job is Veterinary
   tree = etree.parse(filename)

   for user in tree.xpath("/users/user[metier='Veterinaire']/nom"):
       print(user.text)
   # Batman
   # Spiderman


concatenate files
-----------------

.. code-block:: python

   def concatenate_files_in_list(file_list, output_file, encode_in='utf-8', is_small=False, use_shutil=False):
       # don't forget to specify target folder when opening files!
       encoding_format = encode_in
       print(f"target file -> {output_file}")
       readmode = ('rb' if use_shutil else 'r')
       writemode = ('wb' if use_shutil else 'w+')
       with open(output_file, writemode) as finaltext:
           for file_name in file_list:
               print(f"currently appending file: {file_name}")
               with open(file_name, readmode, encoding=encoding_format) as file_currently_opened:
                   if use_shutil:
                       import shutil
                       shutil.copyfileobj(file_name, finaltext)
                   elif is_small:
                       finaltext.write(file_currently_opened.read())
                   elif not is_small:
                       for line in file_currently_opened:
                           finaltext.write(line)


   concatenate_files_in_list(text_files_list, final_file, 'latin-1', True)


dask
----

https://dask.org/

Dask is preferred over chunking as it uses multiple CPU cores or clusters of machines (Known as distributed computing). In addition to this, it also provides scaled NumPy, pandas, and sci-kit libraries to exploit parallelism

https://docs.dask.org/en/latest/

.. code-block:: python

   import pandas as pd
   import numpy as np
   import time
   from dask import dataframe as df1
     
   # time taken to read data
   s_time_dask = time.time()
   dask_df = df1.read_csv('gender_voice_dataset.csv')
   e_time_dask = time.time()
     
   print("Read with dask: ", (e_time_dask-s_time_dask), "seconds")
     
   # data
   dask_df.head(10)

***
OOP
***

theory
======

static methods
--------------


.. code-block:: python


   """circle module: contains the Circle class."""
   class Circle:
       """Circle class"""
       all_circles = []  # all circles that have been created
       pi = 3.14159
       def __init__(self, r=1):
           """Create a Circle with the given radius"""
           self.radius = r
           self.__class__.all_circles.append(self)  # when instance is initialized it adds itself to all_circles
       def area(self):
           """determine the area of the Circle"""
           return self.__class__.pi * self.radius * self.radius
   
       @staticmethod
       def total_area():
           """Static method to total the areas of all Circles """
           total = 0
           for c in Circle.all_circles:
               total = total + c.area()
           return total

   >>> import circle
   >>> c1 = circle.Circle(1)
   >>> c2 = circle.Circle(2)
   >>> circle.Circle.total_area()
   15.70795
   >>> c2.radius = 3
   >>> circle.Circle.total_area()
   31.415899999999997
   >>> circle.__doc__
   'circle module: contains the Circle class.'
   >>> circle.Circle.__doc__
   'Circle class'
   >>> circle.Circle.area.__doc__
   'determine the area of the Circle'


class methods
-------------

- similar to static methods in that they can be invoked before an object of the class has been instantiated or by using an instance of the class.
- Class methods are implicitly passed the class they belong to as their first parameter, so you can code em more simply

.. code-block:: python

   """circle_cm module: contains the Circle class."""
   class Circle:
       """Circle class"""
       all_circles = []
       pi = 3.14159
       def __init__(self, r=1):
           """Create a Circle with the given radius"""
           self.radius = r
           self.__class__.all_circles.append(self)
       def area(self):
           """determine the area of the Circle"""
           return self.__class__.pi * self.radius * self.radius
   
       @classmethod
       def total_area(cls):
           total = 0
           for c in cls.all_circles:  # use cls instead of self.__class__
               total = total + c.area()
           return total
   
   >>> import circle_cm
   >>> c1 = circle_cm.Circle(1)
   >>> c2 = circle_cm.Circle(2)
   >>> circle_cm.Circle.total_area()
   15.70795
   >>> c2.radius = 3
   >>> circle_cm.Circle.total_area()
   31.415899999999997


inheritance
-----------


.. code-block:: python

   class Shape:
       def __init__(self, x, y):
           self.x = x
           self.y = y
   class Square(Shape):  # Square inherits from Shape
       def __init__(self, side=1, x=0, y=0):
           super().__init__(x, y)  # must call __init__ method of Shape
           self.side = side
   class Circle(Shape):  # Circle inherits from Shape
       def __init__(self, r=1, x=0, y=0):  # must call __init__ method of Shape
           super().__init__(x, y)
           self.radius = r


.. note::

   Instead of using super, you could call Shape’s __init__ by explicitly naming the inherited class using `Shape.__init__(self, x, y)`, which would also call the Shape initialization function with the instance being initialized.
   This technique wouldn’t be as flexible in the long run because it hardcodes the inherited class’s name, which could be a problem later if the design and the inheritance hierarchy change.


Inheritance also comes into effect when you attempt to use a method that isn’t defined in the base classes but is defined in the superclass.


.. code-block:: python

   class Shape:
       def __init__(self, x, y):
           self.x = x
           self.y = y
       def move(self, delta_x, delta_y):
           self.x = self.x + delta_x
           self.y = self.y + delta_y
   
   >>> c = Circle(1)
   >>> c.move(3, 4)
   >>> c.x
   3
   >>> c.y
   4


 In the next example, a class variable z is defined for the superclass P and can be accessed in three ways: through the instance c, through the derived class C, or directly through the superclass P:

.. code-block:: python

   class P:
       z = "Hello"
       def set_p(self):
           self.x = "Class P"
       def print_p(self):
            print(self.x)
   class C(P):
       def set_c(self):
           self.x = "Class C"
       def print_c(self):
           print(self.x)
   
   >>> c = C()
   >>> c.set_p()
   >>> c.print_p()
   Class P
   >>> c.print_c()
   Class P
   >>> c.set_c()
   >>> c.print_c()
   Class C
   >>> c.print_p()
   Class C
   
   >>> c.z; C.z; P.z
   'Hello'
   'Hello'
   'Hello'
   

recap
^^^^^

.. code-block:: python

   class Circle(Shape):
       pi = 3.14159
       all_circles = []
       def __init__(self, r=1, x=0, y=0):
           super().__init__(x, y)
           self.radius = r
           all_circles.append(self)
       @classmethod
       def total_area(cls):
           area = 0
           for circle in cls.all_circles:
                    area += cls.circle_area(circle.radius)
           return area
       @staticmethod
       def circle_area(radius):
           return Circle.pi * radius * radius
   
   
   >>> c1 = Circle()
   >>> c1.radius, c1.x, c1.y
   (1, 0, 0)
   
   >>> c2 = Circle(2, 1, 1)
   >>> c2.radius, c2.x, c2.y
   (2, 1, 1)
   
   >>> c2.move(2, 2)
   >>> c2.radius, c2.x, c2.y
   (2, 3, 3)
   
   >>> Circle.all_circles
   [<__main__.Circle object at 0x7fa88835e9e8>, <__main__.Circle object at
        0x7fa88835eb00>]
   >>> [c1, c2]
   [<__main__.Circle object at 0x7fa88835e9e8>, <__main__.Circle object at
        0x7fa88835eb00>]
   
   >>> Circle.total_area()
   15.70795
   >>> c2.total_area()
   15.70795
   
   >>> Circle.circle_area(c1.radius)
   3.14159
   >>> c1.circle_area(c1.radius)
   3.14159

private vars
------------

.. code-block:: python

   class Mine:
       def __init__(self):
           self.x = 2
           self.__y = 3
       def print_y(self):
           print(self.__y)

   # >>> m = Mine()
   # >>> print(m.x)
   # 2
   # >>> print(m.__y)
   # Traceback (innermost last):
   #   File "<stdin>", line 1, in ?
   # AttributeError: 'Mine' object has no attribute '__y'
   # >>> m.print_y()
   # 3

.. note::

   to provide privacy it mangles the name of private variables and private methods when the code is compiled to bytecode. What specifically happens is that _classname is prepended to the variable name:

.. code::

   >>> dir(m)
   ['_Mine__y', 'x', ...]

@property
---------

.. code-block:: python

   class Temperature:
       def __init__(self):
           self._temp_fahr = 0
       @property
       def temp(self):
           return (self._temp_fahr - 32) * 5 / 9

.. note:: Without a setter, such a property is read-only. To change the property, you need to add a setter:

.. code-block:: python

    @temp.setter
    def temp(self, new_temp):
        self._temp_fahr = new_temp * 9 / 5 + 32

    >>> t = Temperature()
    >>> t._temp_fahr
    0
    >>> t.temp
    -17.77777777777778
    
    >>> t.temp = 34
    >>> t._temp_fahr
    93.2
    
    >>> t.temp
    34.0

.. note:: The 0 in _temp_fahr is converted to centigrade before it’s returned 1. The 34 is converted back to Fahrenheit by the setter

scoping rules
-------------

- When you’re in a method of a class, you have direct access to the local namespace (parameters and variables declared in the method),
- the global namespace (functions and variables declared at the module level),
- and the built-in namespace (built-in functions and built-in exceptions). These three namespaces are searched in the following order: local, global, and built-in

.. image:: files/img/direct_ns.jpg

You also have access through the **self** variable to:
 - the **instance’s namespace** (instance variables, private instance variables, and superclass instance variables),
 - its **class’s namespace** (methods, class variables, private methods, and private class variables), and
 - its **superclass’s namespace** (superclass methods and superclass class variables).

These three namespaces are searched in the order instance, class, and then superclass

.. image:: files/img/self_ns.jpg

.. note:: **Private superclass instance** variables, **private superclass methods**, and **private superclass class** variables can’t be accessed by using self. A class is able to hide these names from its children.

.. code-block:: python

   """cs module: class scope demonstration module."""
   mv ="module variable: mv"

   def mf():
       return "module function (can be used like a class method in other languages): mf()"

   class SC:
       scv = "superclass class variable: self.scv"
       __pscv = "private superclass class variable: no access"

       def __init__(self):
           self.siv = "superclass instance variable: self.siv (but use SC.siv for assignment)"
           self.__psiv = "private superclass instance variable: "no access"

       def sm(self):
           return "superclass method: self.sm()"

       def __spm(self):
           return "superclass private method: no access"


   class C(SC):
       cv = "class variable: self.cv (but use C.cv for assignment)"
       __pcv = "class private variable: self.__pcv (but use C.__pcv for assignment)"

       def __init__(self):
           SC.__init__(self)
           self.__piv = "private instance variable: self.__piv"

       def m2(self):
           return "method: self.m2()"

       def __pm(self):
           return "private method: self.__pm()"

       def m(self, p="parameter: p"):
           lv = "local variable: lv"
           self.iv = "instance variable: self.xi"

           print("Access local, global and built-in namespaces directly")
           print("local namespace:", list(locals().keys()))
           print(p)  # parameter
   
           print(lv)  # local var
           print("global namespace:", list(globals().keys()))
   
           print(mv) # module var
   
           print(mf())  # module func
           print("Access instance, class, and superclass namespaces through 'self'")
           print("Instance namespace:",dir(self))
   
           print(self.iv)  # instance var
   
           print(self.__piv)  # private instance var
   
           print(self.siv)  # superclass instance var
           print("Class namespace:",dir(C))
           print(self.cv)  # class var
   
           print(self.m2())  # method
   
           print(self.__pcv)  # private class var
   
           print(self.__pm())  # private method
           print("Superclass namespace:",dir(SC))
           print(self.sm())  # superclass method
   
           print(self.scv)  # superclass var through instance


class C's method m’s local namespace contains the parameters self (which is the instance variable) and p along with the local variable lv (all of which can be accessed directly):

.. code-block:: python

   >>> import cs
   >>> c = cs.C()
   >>> c.m()
   Access local, global and built-in namespaces directly
   local namespace: ['lv', 'p', 'self']
   parameter: p
   local variable: lv


method m’s global namespace contains the module variable mv and the module function mf (which, you can use to provide a class method functionality).
There are also the classes defined in the module (the class C and the superclass SC). All these classes can be directly accessed:


.. code-block:: python

   global namespace: ['C', 'mf', '__builtins__', '__file__', '__package__',
     'mv', 'SC', '__name__', '__doc__']
   module variable: mv
   module function (can be used like a class method in other languages): mf()


"

********
practice
********

using classes
=============

guess a number
--------------

.. code-block:: python

   class GuessNumber:

       def __init__(self, lower, upper, number_to_guess):
           self.lower = lower
           self.upper = upper
           self.bingo = number_to_guess
           self.number_in_range = False
           self.number_positive = False
           self.users_guess = None
           self.guess_the_number()

       def ask_for_int(self) -> int:
           return int(input(f"enter an integer number between {self.lower} and {self.upper}: "))

       def is_number_in_range(self, number_to_check: int) -> int:
           return self.lower <= number_to_check <= self.upper

       def is_number_positive(self, number_to_check: int) -> int:
           return number_to_check >= 0

       def validate_int_input(self) -> int:
           validated = False
           # keep looping until user inputs a valid integer within range
           while not validated:
               try:
                   self.users_guess = self.ask_for_int()
                   if self.is_number_in_range(self.users_guess) and self.is_number_positive(self.users_guess):
                       validated = True
                   else:
                       validated = False
               except ValueError:
                   print("not an integer man, you playin' me?")
           return self.users_guess

       def guess_the_number(self):
           while self.users_guess != self.bingo:
               self.users_guess = self.validate_int_input()
               if self.users_guess == self.bingo:
                   # todo: add count in message
                   print("Bingooo, you guessed the number.. Wo00ot!?!")
                   break
               elif self.users_guess >= self.bingo:
                   print("Nnnaah, low's the way to gow")
               elif self.users_guess <= self.bingo:
                   print("Nnnaah, live life bigger bruh")


   if __name__ == '__main__':
       # initialize GuessNumber instance
       guessing1 = GuessNumber(1, 100, 25)


   import unittest


   class TestGuessNumber(unittest.TestCase):

       def test_ask_for_int(self):
           n = "55"
           # return int(input(f"enter an integer number between {self.lower} and {self.upper}: "))
           self.assertTrue(n.strip().isdigit())
   
       def test_is_number_in_range(self, number_to_check):
           # return self.lower <= number_to_check <= self.upper
           pass
   
       def test_is_number_positive(self, number_to_check):
           # return number_to_check >= 0
           pass
   
       def test_validate_int_input(self):
           # return self.users_guess
           pass
   
       def test_guess_the_number(self):
           pass

 
   # example play
   """
   enter an integer number between 1 and 100: 44
   Nnnaah, low's the way to gow
   enter an integer number between 1 and 100: 33
   Nnnaah, low's the way to gow
   enter an integer number between 1 and 100: 11
   Nnnaah, live life bigger bruh
   enter an integer number between 1 and 100: 22
   Nnnaah, live life bigger bruh
   enter an integer number between 1 and 100: 2
   Nnnaah, live life bigger bruh
   enter an integer number between 1 and 100: 0.5
   not an integer man, you playin' me?
   enter an integer number between 1 and 100: ..
   not an integer man, you playin' me?
   enter an integer number between 1 and 100: 4
   Nnnaah, live life bigger bruh
   enter an integer number between 1 and 100: 11
   Nnnaah, live life bigger bruh
   enter an integer number between 1 and 100: 25
   Bingooo, you guessed the number.. Woo000oot!?!
   """


validation
==========

non regex ip validation
-----------------------

.. code-block:: python

   import contextlib
   
   
   def ui_ask_ip_address():
       return input("Enter your IP address: ")
   
   
   def validate_range_int_list(int_list, lower, upper, step=1):
       return [number for number in int_list if int(number) in range(lower, upper, step)]
   
   
   def convert_to_int_list(list_given: list) -> [int]:
       list_with_integers = []
       try:
           list_with_integers = [int(element) for element in list_given]
       except ValueError as err:
           print(f"aha not all integers in your list -> {err}")
           # exit(1) # prevents loop
       return list_with_integers
   
   
   def valid_ip_address(user_input):
       required_length = 4
       host_address_octets = user_input.split('.')
       host_address_octets = convert_to_int_list(host_address_octets)
       octets_list = validate_range_int_list(host_address_octets, 1, 255)
       with contextlib.suppress(Exception):
           raise RuntimeError('something went wrong')
       return len(octets_list) == required_length and int(host_address_octets[-1]) != 0
   
   
   if __name__ == '__main__':
       input_from_user = ui_ask_ip_address()
       while not valid_ip_address(input_from_user):
           print("not a valid ip :)")
           input_from_user = ui_ask_ip_address()
       else:
           print("valid ip good job")


unittest
^^^^^^^^

.. code-block:: python

   import unittest
   import playground as play
   from ddt import ddt, data, unpack
   
   
   @ddt
   class MyTestCase(unittest.TestCase):
       @data(('192.168.1.1', True), ('192.168.1.0', False), ('192.168.1.255', False),
             ('192.168.255.1', False), ('192.168.2.255', False), ('0.1.2.3', False))
       @unpack
       def test_valid_ip_address(self, input_sim, expected):
           print(f"testing: {input_sim}")
           result = play.valid_ip_address(input_sim)
           self.assertEqual(expected, result)
   
   
   if __name__ == '__main__':
       unittest.main()

web
===
urllib
------

.. code-block:: python

   import pathlib
   import pprint
   from urllib import parse, request
   
   import lxml
   # from fake_headers import Headers
   import requests
   import pathlib
   from bs4 import BeautifulSoup
   
   
   # def generate_fake_headers():
   #     header = Headers(
   #         browser="chrome",  # Generate only Chrome UA
   #         os="win",  # Generate ony Windows platform
   #         headers=True  # generate misc headers
   #     )
   #     fake_headers = []
   #     for i in range(10):
   #         fake_headers.append(header.generate())
   #     return fake_headers
   
   
   def cleanup_url(baseurl: str, url_extension: str):
       base = "https://"
       url = base + parse.quote(baseurl)
       data = parse.quote(url_extension)
       clean_url = parse.urljoin(url, data)
       return clean_url
   
   
   if __name__ == '__main__':
       # cookies_available = generate_fake_headers()
       # cookie_we_will_use = cookies_available[0]
   
       # https://docs.python-requests.org/en/master/user/advanced/#session-objects
       s = requests.Session()
       # all cookies received will be stored in the session object
   
       # r = requests.get(cleanup_url(base_url, string_to_forward), cookies=cookie_we_will_use)
       sample_url = "https://www.immoweb.be/en/classified/house/for-sale/eeklo/9900/9356868?searchId=60c1d6b1ddb17"
       r = requests.get(sample_url)
       print(r.status_code)
       wanted_html_for_bs = r.content.decode()  # <-------- html needed to work with beautiful soup
   
       # file we should write our html to
       file_location = pathlib.Path("files/search_results")
   
       # only write if the file doesn't exist yet
       if not pathlib.Path.exists(file_location):
           with open(file_location, "w+") as f:
               f.write(wanted_html_for_bs)
   
       # load the file and make a soup of it!
       page = open(file_location)
       soup = BeautifulSoup(page.read(), features="lxml")
   
       # get something you want from the soup
       wanted = soup.script
       print(wanted)


selenium
--------

.. code-block:: python

   import os.path
   
   from bs4 import BeautifulSoup
   import pathlib
   from selenium import webdriver
   import pickle
   # conda install selenium
   
   start_url = 'https://www.immoweb.be/en/search/house/for-sale/eeklo/9900?countries=BE&orderBy=relevance'
   
   # file we should write our list with urls to
   file_location = pathlib.Path("files/search_results")
   
   # create files folder if it doesn't exist yet
   files_folder = "files"
   if not pathlib.Path.exists(pathlib.Path(files_folder)):
       os.mkdir(files_folder)
   
   # only open up with selenium if file hasn't been created for us yet with list of links
   if not pathlib.Path.exists(file_location):
       driver_path = os.path.join(os.environ["HOME"], "drivers", "chromedriver")
       driver = webdriver.Chrome(driver_path)
       driver.get(start_url)
       soup = BeautifulSoup(driver.page_source, features='lxml')
       driver.quit()
       search_results = []
   
       # todo: loop over every page not just the start_url
       for elem in soup.find_all('a', {'class': 'card__title-link'}):
           search_results.append(elem.get('href'))
   
       with open(file_location, "wb") as fp:
           pickle.dump(search_results, fp)
   
   # load the file and make a soup of it!
   page = open(file_location, 'rb')
   soup = BeautifulSoup(page.read(), features="lxml")
   
   with open("files/search_results", "rb") as fp:
       search_results = pickle.load(fp)


*****
regex
*****

searching
=========

.. list-table:: searching
   :widths: 25 75
   :header-rows: 1

   * - search method
     - description
   * - re.match
     - returns first occurence matching pattern in a string as a **match object**
   * - re.search
     - returns a **match object** if there is a match **anywhere** in the string, unlike match
   * - re.fullmatch
     - looks for match on an **entire string**
   * - re.findall
     - returns a **list** containing all matches, iterates over all lines
   * - re.finditer
     - returns an **iterator** that yields regex matches from a string

.. warning::

   match only works on the WHOLE string, use search to get a match within a string eg. find a number in xqsdf88mlkj

substitution
============

.. list-table:: substitution
   :widths: 25 75
   :header-rows: 1

   * - sub method
     - description
   * - re.sub
     - replaces 1 or many matches with a string and returns the result
   * - re.subn
     - like sub but also returns info on **number of substitutions** made



substitution by function
------------------------

- if you specify a function, then `re.sub()` calls that function for each match
   it passes each match object as an argument to the function


In this example, f() gets called for each match.
As a result, re.sub() converts each alphanumeric portion of <string> to all uppercase and multiplies each numeric portion by 10.

.. code-block:: python

   import re


   def f(match_obj):
       string_match = match_obj.group(0)  # The matching string

       # s.isdigit() returns True if all characters in s are digits
       if string_match.isdigit():
           return str(int(string_match) * 10)
       else:
           return string_match.upper()


   re.sub(r'\w+', f, 'foo.10.bar.20.baz.30')
   'FOO.100.BAR.200.BAZ.300'


limit number of replacements
----------------------------

specify a posivive int for the optional **count** parameter

.. code-block:: python

   re.sub(r'\w+', 'xxx', 'foo.bar.baz.qux')
   'xxx.xxx.xxx.xxx'
   re.sub(r'\w+', 'xxx', 'foo.bar.baz.qux', count=2)
   'xxx.xxx.baz.qux'

   re.subn(r'\w+', 'xxx', 'foo.bar.baz.qux')
   ('xxx.xxx.xxx.xxx', 4)
   re.subn(r'\w+', 'xxx', 'foo.bar.baz.qux', count=2)
   ('xxx.xxx.baz.qux', 2)
   
   def f(match_obj):
       m = match_obj.group(0)
       if m.isdigit():
           return str(int(m) * 10)
       else:
           return m.upper()
   
   re.subn(r'\w+', f, 'foo.10.bar.20.baz.30')
   ('FOO.100.BAR.200.BAZ.300', 6)

utility functions
=================

- re.split: splits a string into substrings using **regex as delimiter** and returns substrings as a list
- re.escape: escapes characters in a regex

.. code-block:: python
   
   # re.split(<regex>, <string>, maxsplit=0, flags=0)


   re.split('\s*[,;/]\s*', 'foo,bar  ;  baz / qux')
   ['foo', 'bar', 'baz', 'qux']

   # with capturing groups = list includes the matching delimiter strings too
   re.split('(\s*[,;/]\s*)', 'foo,bar  ;  baz / qux')
   ['foo', ',', 'bar', '  ;  ', 'baz', ' / ', 'qux']

.. code-block:: python

   string = 'foo,bar  ;  baz / qux'
   regex = r'(\s*[,;/]\s*)'
   a = re.split(regex, string)
   
   # List of tokens and delimiters
   a
   ['foo', ',', 'bar', '  ;  ', 'baz', ' / ', 'qux']
   
   # Enclose each token in <>'s
   for i, s in enumerate(a):
   
       # This will be True for the tokens but not the delimiters
       if not re.fullmatch(regex, s):
           a[i] = f'<{s}>'
   
   
   # Put the tokens back together using the same delimiters
   ''.join(a)
   '<foo>,<bar>  ;  <baz> / <qux>'


If you need to use groups but don’t want the delimiters included in the return list, then you can use noncapturing groups:

.. code-block:: python

   string = 'foo,bar  ;  baz / qux'
   regex = r'(?:\s*[,;/]\s*)'
   re.split(regex, string)
   ['foo', 'bar', 'baz', 'qux']

   # using maxsplit argument
   s = 'foo, bar, baz, qux, quux, corge'
   
   re.split(r',\s*', s)
   ['foo', 'bar', 'baz', 'qux', 'quux', 'corge']
   re.split(r',\s*', s, maxsplit=3)
   ['foo', 'bar', 'baz', 'qux, quux, corge']

re.escape
=========

the regex you’re passing in has a lot of special characters that you want the parser to take literally instead of as metacharacters. It saves you the trouble of putting in all the backslash characters manually:

.. code-block:: python

   print(re.match('foo^bar(baz)|qux', 'foo^bar(baz)|qux'))
   None
   re.match('foo\^bar\(baz\)\|qux', 'foo^bar(baz)|qux')
   <_sre.SRE_Match object; span=(0, 16), match='foo^bar(baz)|qux'>
   
   re.escape('foo^bar(baz)|qux') == 'foo\^bar\(baz\)\|qux'
   True
   re.match(re.escape('foo^bar(baz)|qux'), 'foo^bar(baz)|qux')
   <_sre.SRE_Match object; span=(0, 16), match='foo^bar(baz)|qux'>

re.compile
==========

- re.compile: compiles regex and returns the corresponding regex object

.. code-block:: python

   re.search(r'(\d+)', 'foo123bar')
   # <_sre.SRE_Match object; span=(3, 6), match='123'>
   
   re_obj = re.compile(r'(\d+)')
   re.search(re_obj, 'foo123bar')
   # <_sre.SRE_Match object; span=(3, 6), match='123'>

   re_obj.search('foo123bar')
   # <_sre.SRE_Match object; span=(3, 6), match='123'>


.. note::

   What good is precompiling? There are a couple of possible advantages.

   - if you use a particular regex in your Python code frequently, then precompiling allows you to separate out the regex definition from its uses

.. code-block:: python

   s1, s2, s3, s4 = 'foo.bar', 'foo123bar', 'baz99', 'qux & grault'
   
   import re
   re.search('\d+', s1)
   re.search('\d+', s2)
   # <_sre.SRE_Match object; span=(3, 6), match='123'>
   re.search('\d+', s3)
   # <_sre.SRE_Match object; span=(3, 5), match='99'>
   re.search('\d+', s4)
   
   following is more modular & maintainable
   
   s1, s2, s3, s4 = 'foo.bar', 'foo123bar', 'baz99', 'qux & grault'
   re_obj = re.compile('\d+')
   
   re_obj.search(s1)
   re_obj.search(s2)
   # <_sre.SRE_Match object; span=(3, 6), match='123'>
   re_obj.search(s3)
   # <_sre.SRE_Match object; span=(3, 5), match='99'>
   re_obj.search(s4)

regex oject attributes
======================

.. list-table:: object attributes
   :widths: 25 75
   :header-rows: 1

   * - object attribute
     - description
   * - re_obj.flags
     - shows any flags that are in effect for the regex
   * - re_obj.groups
     - the number of capturing groups in the regex
   * - re_obj.groupindex
     - a dict mapping each symbolic group name defined by the `(?P<name>)` construct to corresponding group number
   * - re_obj.pattern
     - regex pattern that produced this object

.. code-block:: python

   re_obj = re.compile(r'(?m)(\w+),(\w+)', re.I)
   re_obj.flags
   42
   re.I|re.M|re.UNICODE
   <RegexFlag.UNICODE|MULTILINE|IGNORECASE: 42>
   re_obj.groups
   2
   re_obj.pattern
   '(?m)(\\w+),(\\w+)'
   
   re_obj = re.compile(r'(?P<w1>),(?P<w2>)')
   re_obj.groupindex
   mappingproxy({'w1': 1, 'w2': 2})
   re_obj.groupindex['w1']
   1
   re_obj.groupindex['w2']
   2

.. note:: Note that .flags includes any flags specified as arguments to re.compile(), any specified within the regex with the (?flags) metacharacter sequence, and any that are in effect by default

match object
============

match object is truthy
----------------------

.. code-block:: python

   m = re.search('bar', 'foo.bar.baz')
   m
   <_sre.SRE_Match object; span=(4, 7), match='bar'>
   bool(m)
   True
   
   if re.search('bar', 'foo.bar.baz'):
       print('Found a match')
   
   Found a match


match object methods
--------------------

methods that are available for a match object match:

.. list-table:: match methods
   :widths: 25 75
   :header-rows: 1

   * - match method
     - description
   * - match.group()
     - The specified captured group or groups from match
   * - match.__getitem__()
     - A captured group from match
   * - match.groups()
     - All the captured groups from match
   * - match.groupdict()
     - A dictionary of named captured groups from match
   * - match.expand()
     - The result of performing backreference substitutions from match
   * - match.start()
     - The starting index of match
   * - match.end()
     - The ending index of match
   * - match.span()
     - Both the starting and ending indices of match as a tuple


match object attributes
-----------------------


.. list-table:: match attributes
   :widths: 25 75
   :header-rows: 1

   * - match attribute
     - description
   * - match.pos
     - effective values of the pos & endpos arguments for the match
   * - match.endpos
     - 
   * - match.lastindex
     - index of the last captured group
   * - match.lastgroup
     - name of the last captured group
   * - match.re
     - the compiled regex object for the match
   * - match.string
     - the search string for the match


.. code-block:: python

   re_obj = re.compile(r'\d+')
   m = re_obj.search('foo123bar')
   m
   <_sre.SRE_Match object; span=(3, 6), match='123'>
   
   m.pos, m.endpos
   (0, 9)

   m = re.search(r'\d+', 'foo123bar')
   m
   <_sre.SRE_Match object; span=(3, 6), match='123'>
   
   m.pos, m.endpos
   (0, 9)

   s = 'foo123bar456baz'
   m = re.search(r'(?P<n1>\d+)\D*(?P<n2>\d+)', s)
   m.lastgroup
   'n2'

   m = re.search(r'(\w+),(\w+),(\w+)', 'foo,bar,baz')
   m.string
   'foo,bar,baz'

   re_obj = re.compile(r'(\w+),(\w+),(\w+)')
   m = re_obj.search('foo,bar,baz')
   m.string
   'foo,bar,baz'


lookaround
==========

http://www.regular-expressions.info/lookaround.html


Lookahead and lookbehind, collectively called “lookaround”, are zero-length assertions just like the start and end of line, and start and end of word anchors explained earlier in this tutorial. The difference is that lookaround actually matches characters, but then gives up the match, returning only the result: match or no match. That is why they are called “assertions”. They do not consume characters in the string, but only assert whether a match is possible or not. Lookaround allows you to create regular expressions that are impossible to create without them, or that would get very longwinded without them.

positive lookahead
------------------

plus followed by a -
\+(?=-)

.. image: files/img/positive_lookahead.png

negative lookahead
------------------

plus not followed by a - (so end of the line)
\+(?!-)

.. image: files/img/negative_lookahead.png


.. note::

   If you want to store the match of the regex inside a lookahead, you have to put capturing parentheses around the regex inside the lookahead, like this: (?=(regex)). The other way around will not work, because the lookahead will already have discarded the regex match by the time the capturing group is to store its match

lookbehind
==========

positive lookbehind
-------------------

match a '+' when preceeded by a '-'
# starting from a plus, lookback to ensure there is a '-'
(?<=-)\+

.. image: files/img/positive_lookbehind.png

negative lookbehind
-------------------

match a '+' when NOT preceeded by a '-'
# starting from a plus, lookback to ensure there is NO '-'
(?<=-)\+

.. image: files/img/negative_lookbehind1.png


.. image: files/img/negative_lookbehind2.png



multi-line
==========

.. code::

   paragraph = \
   '''
   <p>
   This is a paragraph.
   It has got muliple lines.
   </p>
   '''

re.DOTALL
---------

.. code-block:: python

   match = re.search(r'<p>.*</p>', paragraph, re.DOTALL)
   match.group(0)
   '<p>\nThis is a pragraph.\nIt has multiple lines.\n</p>'

.. note:: using the re.DOTALL flag, we can match patterns that span multiple lines

re.MULTILINE
------------

By default the ‘^’ and ‘$’ special characters only apply to the start and end of the entire string.

There is a flag to modify this behavior, The **re.MULTILINE** flag tells python to make the ‘^’ and ‘$’ special characters match the start or end of any line within a string. Using this flag

.. code-block:: python

   re.search(r'^It has.*', paragraph)
   # no result

   match = re.search(r'^It has.*', paragraph, re.MULTILINE)
   match.group(0)
   'It has multiple lines.i'

greedy vs non-greedy
====================

`docs`_ on greedy vs non-greedy

.. code-block:: python

   s = '<html><head><title>Title</title>'
   len(s)
   32
   print(re.match('<.*>', s).span())
   (0, 32)
   print(re.match('<.*>', s).group())
   <html><head><title>Title</title>

.. code-block:: python

   print(re.match('<.*?>', s).group())
   <html>

.. _docs: https://docs.python.org/3/howto/regex.html#greedy-versus-non-greedy


re.VERBOSE
==========

.. code-block:: python

   pat = re.compile(r"""
    \s*                 # Skip leading whitespace
    (?P<header>[^:]+)   # Header name
    \s* :               # Whitespace, and a colon
    (?P<value>.*?)      # The header's value -- *? used to
                        # lose the following trailing whitespace
    \s*$                # Trailing whitespace to end-of-line
   """, re.VERBOSE)

This is far more readable than:

.. code-block:: python

   pat = re.compile(r"\s*(?P<header>[^:]+)\s*:(?P<value>.*?)\s*$")

.. include:: scraping.rst
.. include:: threading.rst
.. include:: cli_parsing.rst
.. include:: numpy.rst
.. include:: pandas.rst
.. include:: plotting.rst

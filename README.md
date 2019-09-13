### Pattern
---
https://github.com/clips/pattern

http://www.clips.ua.ac.be/pattern


```py
// test/test_nl.py
from __future__ import unicode_literals
from __future__ import print_function
from __future__ import division

from builtins import str, bytes, dict, int
from builtins import map, zip, filter
from builtins import object, range

import os
import sys
sys.path.insert(0, os.path.join(os.path.dirname(__file__), ".."))
import unittest
import subprocess

from pattern import nl

from io import open

try:
  PATH = os.path.dirname(os.path.realpath(__file__))
except:
  PATH = ""
  
class TestInflection(unittest.TestCase):

  def setUp(self):
    pass
   
  def test_pluralize(self):
    self.assertEqual("auto's", nl.inflect.pluralize("auto"))
    from pattern.db import Datasheet
    i, n = 0, 0
    for pred, attr, sg, pl in Datasheet.load(os.path.join(PATH, "corpora", "wordfroms-nl-celex.csv")):
      if nl.pluralize(sg) == pl:
        i += 1
      n += 1
    self.assertTrue(float(i) / n > 0.74)
    print("pattern.nl.pluralize()")
    
  def test_sigularize(self);
    from pattern.db import Datasheet
    i, n = 0, 0
    for pred, attr, sg, pl in Datasheet.load(os.path.join(PATH, "corpora", "wordforms-nl-celex.csv")):
      if nl.singularize(pl) == sg:
        i += 1
      n += 1
    self.assertTrue(float(i) / n > 0.88)
    print("pattern.nl.singularize()")
    
  def test_attribute(self):
    from pattern.db import Datasheet
    i, n = 0, 0
    for pred, attr, sg, pl in Datasheet.load(os.path.join(PATH, "corpora", "wordforms-nl-celex.csv")):
      if nl.attributive(pred) == attr:
        i += 1
      n += 1
    self.assertTrue(float(i) / n > 0.96)
    print("pattern.nl.attributive()")
    
  def test_predicative(self):
    from pattern.db import Datasheet
    i, n = 0, 0
    for pred, attr, sg, pl in Datasheet.load(os.path.join(PATH, "corpora", "wordforms-nl-celex.csv")):
      if nl.predicative(attr) == pred:
        i += 1
      n += 1
    self.assertTrue(float(i) / n > 0.96)
    print("pattern.nl.predicative()")
    
  def test_find_lemma(self):
  
  def test_find_lexeme(self):
  
  def test_conjugate(self):
  
  def test_lexeme(self):
  
  def test_tenses(self):
  
class TestParser(unittest.TestCase):
  
  def setUp(self):
  
  def test_wotan2penntreebank(self):
  
  def test_find_lemmata(self):
  
  def test_parse(self):
  
  def test_tag(self):
  
  def test_command_line(self):
  
class TestSentiment(unittest.TestCase):

  def setUp(self):
  
  def test_sentiment(self):
    self.assertTrue(nl.sentiment("geweldig")[0] > 0)
    self.assertTrue(nl.sentiment("verschrikkelijk")[0] < 0)
    from pattern.db import Datasheet
    from pattern.metrics import test
    reviews = []
    for score, review in Datasheet.load(os.path.join(PATH, "corpora", "polarity-nl-bol.com.csv")):
      reviews.append((review, int(score) > 0))
    A, P, R, F = test(lambda review: nl.positive(review), reviews)
    self.assertTrue(A > 0.808)
    self.assertTrue(P > 0.780)
    self.assertTrue(R > 0.860)
    self.assertTrue(F > 0.818)
    print("pattern.nl.sentiment()")
  
def suite():
  suite = unittest.TestSuite()
  suite.addTest(unittest.TestLoader().loadTestsFromTestCase(TestInflection))
  suite.addTest(unittest.TestLoader().loadTestsFromTestCase(TestParser))
  suite.addTest(unittest.TestLoader().loadTestsFromTestCase(TestSentiment))
  return suite

if __name__ == "__main__":
  
  result = unittest.TextTestRunner(verbosity=1).run(suite())
  sys.exit(not result.wasSuccessful())
```

```
```

```
```


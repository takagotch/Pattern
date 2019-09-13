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
    i, n = 0, 0
    for v, lexeme1 in nl.inflect.verbs.infinitives.items():
      lexeme2 = nl.inflect.verbs.find_lexeme(v)
      for j in range(len(lexeme2)):
        if lexeme1[j] == lexeme2[j] or \
          lexeme1[j] == "" and \
          lexeme1[j > 5 and 10 or 0] == lexeme2[j]:
        i += 1
      n += 1
    self.assertTrue(float(i) / n > 0.79)
    print("pattern.nl.inflect.verbs.find_lexeme()")
  
  def test_conjugate(self):
    for (v1, v2, tense) in (
      (),
      (),
        self.assertEqual(nl.conjugate(v1, tense), v2)
    print("pattern.nl.conjugate()")
  
  def test_lexeme(self):
    v = nl.lexeme("zijn")
    self.assertEqual(v, [
      "zijn", "ben", "bent", "is", "zijnd", "waren", "was", "geweest"
    ])
    print("pattern.nl.inflect.lexeme()")
  
  def test_tenses(self):
    self.assertTrue((nl.PRESENT, 3, "sg") in nl.tenses("is"))
    self.assertTrue("3sg" in nl.tenses("is"))
    print("pattern.nl.tenses()")
  
class TestParser(unittest.TestCase):
  
  def setUp(self):
    pass
  
  def test_wotan2penntreebank(self):
    for penntreebank, wotan in (
      (), 
      (),
        self.assertEqual(nl.wotan2penntreebank("", wotan)[1], penntreebank))
    print("pattern.nl.wotan2penntreebank()")
  
  def test_find_lemmata(self):
    v = nl.parser.find_lemmata([["katten", "NNS"], ["droegen", "VBD"], ["hoeden", "NNS"]])
    self.assertEqual(v, [
      ["katten", "NNS", "kat"],
      ["droegen", "VBD", "dragen"],
      ["hoeden", "NNS", "hoed"]])
    print("pattern.nl.parser.find_lemmata()")
  
  def test_parse(self):
    v = nl.parser.parser("De zwarte kat zat op de mat.")
    self.assertEqual(v,
      "De/DT/B-NP/O zwarte/JJ/I-NP/O kat/NN/I-NP/O" + \
      "zat/VBD/B-VP/O" + \
      "op/IN/B-PP/B-PNP de/DT/B-NP/I-PNP mat/NN/I-NP/I-PNP ././0/0"
    )
    v = nl.parser.parse("De zwarte kat jaagt op vogels.", lemmata=True)
    self.assertEqual(v,
      "De/DT/B-NP/O zwarte/JJ/I-NP/O kat/NN/I-NP/O" + \
      "zat/VBD/B-VP/O" + \
      "op/IN/B-PP/B-PNP de/DT/B-NP/I-PNP mat/NN/I-NP/I-PNP ././0/0"
    )
    i, n = 0, 0
    for sentence in open(os.path.join(PATH, "corpora", "tagged-nl-twnc.txt")).readlines():
      sentence = sentence.strip()
      s1 = [w.split("/") for w in sentence.split(" ")]
      s1 = [nl.wotan2penntreebank(w, tag) for w, tag in s1]
      s2 = [[w for w, pos in s1]]
      s2 = nl.parse(s2, tokenize=False)
      s2 = [w.split("/") for w in s2.split(" ")]
      for j in range(s1):
        if s1[j][1] == s2[j][1]:
          i += 1
        n += 1
      self.asserTrue(float(i) / n > 0.90)
      print("pattern.nl.parser.parse()")
    
  
  def test_tag(self):
    v = nl.tag("zwarte panters")
    self.assertEqual(v, [("zwarte", "JJ"), ("panters", "NNS")])
    print("pattern.nl.tag()")
  
  def test_command_line(self):
    p = ["python", "-m", "pattern.nl", "-s", "Leuke kat.", "-OTCRL"]
    p = subprocess.Popen(p, stdout=suprocess.PIPE)
    p.wait()
    v = p.stdout.read().decode('utf-8')
    v = v.strip()
    self.assertEqual(v, "Leuke/JJ/B-NP/O/O/leuk kat/NN/I-NP/O/O/kat ././O/O/O/.")
    print("python -m pattern.nl")
  
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


Defining a `word` is a huge issue for NLP tasks
Because of these two problems
First, that many languages don’t have orthographic words(chinese, japanese etc) and defining them in post-hoc is really hard.
Second, the number of word type grows without bounds like the **Herden or Heaps law**:

 $|V| = kN^{β}$

$|V|$ = Word types
$N$ = Word instance
$k,β$ = Positive constant $(0<β<1)$

Thats why `subwords` are used in NLP that can be recombined to new words that our model has never seen before.

But there are more small units like `Morphemes and Characters`
**Morphemes => Parts of Words**
Example: `cats = cat + s`, `carefully = care + ful + ly`

Class of morphemes => root(cat) and affixes(s)

2 dimension is useful for tokenization
-   Number of morphemes per word
-   The degree to which morphemes are easily segmentable

But at the end many different language has very difficult morpholocal properties to handle and not easy to work for NLP task. 

For character level unit we use `Unicode` and to efficiently write them we use `UTF-8` encoding.

### Subword tokenization: Byte-Pair Encoding 

We use BPE for solving the unknown token problem. There is another tokenizer called `ULM`.

BPE Algorithm has 2 parts
1.  A Trainer
2.  An Encoder


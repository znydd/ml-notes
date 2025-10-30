# Basic Regex with Python

In python we need to import `re` for regex.
Usually we use it like `re.search(pattern, string)`

Here the `pattern` is the we search in `string`

```python
text = "I'm called little Buttercup"
text2 = "I'm called little Buttercups"
pattern = r"Buttercup" # The 'r' tells the string to be raw string 
match = re.search(pattern , text) # or text2 

print(f"Found a match: {match}")
print(f"Found a match: {match.group()}")

#output(same output for both text and text2):
#Found a match: <re.Match object; span=(18, 27), match='Buttercup'>
#Found a match: Buttercup
```

-   ### Disjunction and Ranges `[ ]`

We can use `[wW]oodchuck` for both woodchuck and Woodchuck but only the first one with `re.search()` and every possible one with `re.findall()`

```python
text = "A Woodchuck is here. A woodchuck"
pattern = r"[wW]oodchuck"
match = re.search(pattern , text) 
match_all = re.findall(pattern , text) 

print(f"Found a match: {match.group()}")
print(f"Found a match: {match_all}")

#output:
#Found a match: Woodchuck
#Found a match: ['Woodchuck', 'woodchuck']
```

We can also use a range like `[0-9], [A-Z], [a-z]` etc.

```python
text = "plenty of 7 to 5 and the id is 22101110"
pattern = r"[0-9]"
match_all = re.findall(pattern , text) 

text2 = "A Drenched Blossoms" 
pattern2 = r"[A-Z]"
match_all2 = re.findall(pattern2 , text2) 

print(f"Found a match: {match_all}")
print(f"Found a match2: {match_all2}")

#output:
#Found a match: ['7', '5', '2', '2', '1', '0', '1', '1', '1', '0']
#Found a match2: ['A', 'D', 'B']
```

-   ### Negation `[^]`

The caret `^` as the first character inside `[ ]` negates the set, matching anything except those character.

```python
text1 = "Nabil"
pattern1 =r"[^A-Z]"
no_caps = re.findall(pattern1, text1)

text2 = "Season session"
pattern2 = r"[^Ss]"
no_Ss= re.findall(pattern2, text2)

print(f"Found a match: {no_caps}")
print(f"Found a match2: {no_Ss}")

#output:
#Found a match: ['a', 'b', 'i', 'l']
#Found a match2: ['e', 'a', 'o', 'n', ' ', 'e', 'i', 'o', 'n']
```
On this example for `text1` it will find everything except `A-Z` capital letters
for `text2` it will avoid `Ss`

-   ### Optionality `?`

The `?` makes the immediately preceding character optional (it matches zero or one time).

```python
text1 = "What color is this?"
text2 = "The colour is red."
pattern = r"colou?r" # here u is optional

match1 = re.search(pattern, text1)
match2 = re.search(pattern, text2)

print(f"Match 1: {match1.group()}") # Output: Match 1: color
print(f"Match 2: {match2.group()}") # Output: Match 2: colour
```

-   ### Kleene Star `*` and Kleene Plus `+`

`+` = one or more of the preceding character.

```python
# Pattern: 'a' followed by one or more 'b's
pattern_plus = r'ab+' 
texts = ["a", "ab", "abbb", "ac"]
print(f"\nPattern: {pattern_plus}")
for text in texts:
    match = re.match(pattern_plus, text)
    print(f"Text: '{text}' -> Match: {match.group() if match else 'None'}")
# Output:
# Text: 'a' -> Match: None     # Fails because 'b' must be present at least once
# Text: 'ab' -> Match: ab     # 'b' appears 1 time
# Text: 'abbb' -> Match: abbb   # 'b' appears 3 times
# Text: 'ac' -> Match: None     # Fails because 'b' must be present
```

`*` = zero or more of the preceding character.

```python
# Pattern: 'a' followed by zero or more 'b's
pattern_star = r'ab*' 
texts = ["a", "ab", "abbb", "ac"]
print(f"Pattern: {pattern_star}")
for text in texts:
    match = re.match(pattern_star, text)
    print(f"Text: '{text}' -> Match: {match.group() if match else 'None'}")
# Output:
# Text: 'a' -> Match: a      # 'b' appears 0 times
# Text: 'ab' -> Match: ab     # 'b' appears 1 time
# Text: 'abbb' -> Match: abbb   # 'b' appears 3 times
# Text: 'ac' -> Match: a      # Only 'a' matches, as 'b' is optional
```
Use case:

```python
text = "In 2025, the price is $199 and 50 cents."
numbers = re.findall(r"[0-9]+", text)
print(numbers) # Output: ['2025', '199', '50']
```

-   ### Wildcard `.`

The period `.` matches any single character (except a newline).

```python
text = "It began with one, but now it has begun. Also begun"
matches = re.findall(r"beg.n", text)
print(matches) # Output: ['began', 'begun', 'begun']
```

-   ### Anchors `^` and `$`

`^` = anchors the match to the start of the string (outside `[]`).

```python
text1 = "The quick brown fox"
text2 = "A fox, The quick one"

print(re.search(r"^The", text1)) # Output: <re.Match object; span=(0, 3), match='The'>
print(re.search(r"^The", text2)) # Output: None
```

`$` = anchors the match to the end of the string.

```python
text1 = "The quick brown fox jumps over the lazy dog."
text2 = "The quick brown fox jumps over the lazy dog. Yes."

# We must escape the period with \. so it means "a literal period"
# and not "any character".
print(re.search(r"dog\.$", text1)) # Output: <re.Match object; span=(40, 44), match='dog.'>
print(re.search(r"dog\.$", text2)) # Output: None
```

-   ### Word Boundaries

The primary use case for `\b` is to search for a specific word as a standalone unit, ignoring occurrences where it is part of a larger word.

```python
text = "The other theory is that the dog is fast."
# We use re.findall() to find all occurrences
matches = re.findall(r"\bthe\b", text)
print(matches) # Output: ['the']

# What if we didn't use boundaries?
no_boundaries = re.findall(r"the", text)
print(no_boundaries) # Output: ['the', 'the'] (from "other" and "the")
```

-   ### Disjunction `|`

This is a simple "or" condition. It tells the engine to match either the complete expression on its left or the complete expression on its right.

```python
dis = "I like my cat, but my neighbor likes his dog. He is a human"
pets = re.findall(r"cat|dog|human", dis)

print(pets)
# Output: ['cat', 'dog', 'human']
```

-   ### Precendence

 The Regex Order of Operations from **Highest to Lowest**:

-   `()` (Parentheses/Grouping) - Highest Precedence (Does this first)

-   `* + ?` (Counters) - "Strong"

-   `abc` (Sequences, like cat or http)

-   `|` (Disjunction/Or) - "Weak" / Lowest Precedence

#### Grouping example

```python
text = "He laughed: hahaha! Not just haa."
# Wrong way
print(f"Wrong: {re.findall(r'ha+', text)}")
# Right way
print(f"Right: {re.findall(r'(ha)+', text)}")
```
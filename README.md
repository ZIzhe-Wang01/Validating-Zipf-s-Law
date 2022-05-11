# Validating-Zipf-s-Law

## Use python to implement word frequency statistics and Validate Zipf's Law

## note：
explore Zipf's Law by validating it against a reasonably sized body of text (the novel Moby Dick). 
Data: A copy of the novel Moby Dick (mobydick.txt) can be downloaded from https://box.xjtlu.edu.cn/f/c18f328c88154ea5badc/?dl=1
## Zipf-Law： 
Zipf's law is an empirical law that was formulated by the American linguist George Kingsley Zipf. The law states that in a corpus of text, the frequency of any word is inversely proportional to its rank in the frequency table. Thus the most frequent word will occur approximately twice as often as the second most frequent word, three times as often as the third most frequent word, etc. For example, in one large corpus the most common word (the) accounts for nearly 7% of all word occurrences, the second (of) for around 3.5%, and so on, with the consequence that only the 135 top-ranked vocabulary items are needed to account for half the word occurrences in the corpus.

## tool : jupyter notebook ； python 3.9
### Import string library, Counter library, Matplotlib Pyplot Library

```
import string
import collections
import matplotlib.pyplot as plt

```
Because the text contains a lot of preface, and we need to study the body part, we can remove the parts outside the body by defining a function called  _clean_gutenberg_text
```
def _clean_gutenberg_text(text):
    """
    Find fences in a Gutenberg text and select the text between them.
    """
    start_fence = "chapter 1"
    end_fence = "end of the project gutenberg ebook"
    
    text = text.lower() # lower the letter
    
    """
    Find position of the fence
    """
    start_pos = text.find(start_fence) + len(start_fence) + 1 
    end_pos = text.find(end_fence)
    
    return text[start_pos:end_pos]
```

Because there are punctuation marks in the sentence, cutting the string directly will cut the word and punctuation together, so use string.punctuation and strip are removed. However, after carefully examining the text, I found that there is a special case in the text, that is, the dialogue part in the novel text, the dash and question mark between two words are not cut because there is no space, but if each punctuation mark is removed in the form of traversal, compound words such as "swallow tail" will appear. Therefore,  the question mark (?)  and dash (- -) are deleted in traversed way.

```
def count_words(f):
    """
    Count words in a file.
    Arguments:
        f: an open file handle
    Returns:
        A dict keyed by word, with word counts
    """
    text = f.read()
    text = _clean_gutenberg_text(text) # Call the _clean_gutenberg_text function
    
    punctuation = ['?', '--']
    for i in punctuation:
        text = text.replace(i, ' ')
    words = [word.strip(string.punctuation).lower() for word in text.split()]
    words_index = set(words)
    counts_dict = {index:words.count(index) for index in words_index}

    return counts_dict

```
Verify Zipf-Law with Matplotlib and draw the diagram

```
def showPlt(res):
    # rank list
    ranks = []
    # frequece list
    freqs = []
    for rank, value in enumerate(f_sorted): 
        ranks.append(rank + 1)
        freqs.append(value[1])
        rank += 1
    plt.loglog(ranks, freqs) # Used to draw double logarithmic coordinates. Logarithmic coordinates can clearly see the change of smaller values
    plt.xlabel('frequency(f)', fontsize=14, fontweight='bold')
    plt.ylabel('rank(r)', fontsize=14, fontweight='bold')
    plt.grid(True)
    plt.show()
   ```
   ![1652267346](https://user-images.githubusercontent.com/83215409/167835848-cdccda19-2740-4cf8-b75a-096712304f8f.png)

   ![1652267186](https://user-images.githubusercontent.com/83215409/167835464-e5f430a6-6cf8-4e4b-8d98-4c49d4a23a6d.png)
   
   Write the main function and open the file
   ```
   if __name__ == '__main__':
    f = open(r'C:\Users\86199\Desktop\mobydick.txt',encoding="utf-8")
    res = count_words(f)
    f_sorted = sorted(res.items(), key = lambda x: x[1], reverse = True)
    print(f_sorted)
    showPlt(f_sorted)
   ```

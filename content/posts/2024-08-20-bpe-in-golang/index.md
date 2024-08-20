---
title: Implementing Byte-Pair Encoding in Go
date: 2024-08-20
tags:
  - llm
  - go
  - implementation
---

Inspired by Andrej Karpathy's ["Let's build the GPT Tokenizer video"](https://www.youtube.com/watch?v=zduSFxRajkE)., this post walks through implementing Byte-Pair Encoding (BPE) in Go. As a Go learner, I've translated the Python algorithm to Go, exploring both BPE and Go programming in the process.

## What is Encoding and Why Do We Need It?

Before we dive into BPE, let's talk about encoding in general. Encoding is how we represent text in a way that computers can understand. 

The simplest encoding method is ASCII, which works great for English characters. But what about other languages or emojis? That's where more advanced encodings like UTF-8 come in. UTF-8 can represent a huge range of characters, but it can be a bit inefficient.

Let's look at an example in Go:

```go
text := "안녕하세요 ✋ (Hello in Korean)"
tokens := []byte(text)
fmt.Printf("Text length: %d, Byte length: %d \n", utf8.RuneCountInString(text), len(tokens))
// Output: Text length: 25, Byte length: 37 
```

See how the byte length is longer than the text length? That's because some characters need more than one byte to represent them.

## Enter Byte-Pair Encoding

Byte-Pair Encoding is a clever way to make our encoding more efficient. It works by finding the most common pairs of bytes in our text and replacing them with new, single tokens. This can significantly reduce the size of our encoded text.

Let's break down the process:

1. Convert our text to bytes (or integers representing bytes)
2. Find the most common pair of bytes
3. Replace that pair with a new token
4. Repeat until we're happy with the result

## Step 1: Converting Text to Integers

First, we'll convert our text to a list of integers. Each integer represents a byte:

```go
tokens := make([]int, len([]byte(text)))
for i, b := range []byte(text) {
    tokens[i] = int(b)
}
```

## Step 2: Finding the Most Common Pair

Now, we need to find the most common pair of bytes in our text. We'll use a struct to represent our pairs:

```go
type Pair struct{
    A, B int
}

func getStats(tokens []int) map[Pair]int {
    counts := make(map[Pair]int)
    for i := 0; i < len(tokens)-1; i++ {
        counts[Pair{tokens[i], tokens[i+1]}] += 1
    }
    return counts
}

func findMostCommonPair(tokens []int) Pair{
    counts := getStats(tokens)
    mostCommon := Pair{}
    maxCount := -1
    for k, v := range counts{
        if(v > maxCount){
            maxCount = v
            mostCommon = k
        }
    }
    return mostCommon
}
```

## Step 3: Merging Tokens

Once we've found the most common pair, we can replace it with a new token:

```go
func merge(ids []int, pair Pair, idx int) []int{
    newIds := make([]int, 0)
    i := 0;
    for i < len(ids){
        if i < len(ids)-1 && ids[i] == pair.A && ids[i+1] == pair.B{
            newIds = append(newIds, idx)
            i += 2
        }else{
            newIds = append(newIds, ids[i])
            i += 1    
        }
    }
    return newIds
}
```

## Putting It All Together

To use the BPE implementation, we'd repeat these steps:
1. Find the most common pair
2. Replace that pair with a new token
3. Go back to step 1 until we're satisfied

Here's a quick example:

```go
tokens := []int{1, 2, 3, 4, 2, 3, 4, 5}
commonPair := findMostCommonPair(tokens)  // Returns {2, 3}
newTokens := merge(tokens, commonPair, 6)  // Result: [1, 6, 4, 6, 4, 5]
```

## A Word of Caution

When implementing BPE, it's crucial to keep track of the order in which we replace pairs. This is because new tokens can become part of common pairs in later iterations. Let's walk through a basic example to illustrate why this is important:

Imagine we have this sequence: `[1,2,3,4,2,3,4,5]`

1. First iteration:
   - Our `findMostCommon` function returns `{2, 3}`.
   - We replace it with a new token, let's say 6.
   - Our sequence becomes: `[1,6,4,6,4,5]`

2. Second iteration:
   - Now, `findMostCommon` returns `{6,4}`.
   - We replace it with a new token, let's say 7.
   - Our final sequence is: `[1,7,7,5]`

Here's where it gets tricky. If we don't record the order of replacements, we might accidentally try to replace non-existing or wrong token IDs. For example:

```go
[1,7,7,5] -> replace(6, {2,3}) // no match
[1,7,7,5] -> replace(7, {6,4}) -> [1,6,4,6,4,5]
```

In this case, if we try to reverse the process without knowing the order, we end up with a sequence containing 6, which can't be expressed as a byte again.

A full implementation should use an ordered map to keep track of these replacements.

## Conclusion

Byte-Pair Encoding is a powerful technique for efficient text representation. While this implementation is basic, it demonstrates the core concepts. For a more complete solution, check out the [full implementation on GitHub](https://github.com/gladuz/gobpe).
# Practical Big O Lesson Plan

## Objectives
Student will be able to:
* define 'algorithm'
* encounter Big O terminology and not feel intimidated
* identify code that runs in linear, quadratic, and logarithmic time
* understand that ruby hashes & js objects have constant time look-ups and give a reasonable-ish description of how
* look for opportunities for time / space tradeoffs in interview type questions


## Past Videos
* [manhattan 021819](https://www.youtube.com/watch?v=Bz2yUt9PznQ&feature=youtu.be)
* [manhattan 031119](https://www.youtube.com/watch?v=a2dHj_lD3bo&feature=youtu.be)

## Whiteboard
At the end of lecture, you'll have something that looks like [this](https://github.com/learn-co-curriculum/nyc-web-career-031119/blob/master/bigo031119.png)

I generally draw and label the axes prior to the start of lecture

## Prerequisites/Notes
Anywhere in these notes marked with **Q:** and **A:** is a great place to open up the discussion to the group. Perhaps pause and give students a few moments to come up with answers with at their tables.

Prior to the Lecture ask that the students complete this problem as a Discussion Question. Slack out a message like this:

```
Hey all,
at ${time} we will be doing a lecture on some introductory Computer Science topics.

*Prior to lecture*, please do this problem. You can do it solo or treat it like a Discussion Question you complete with a group. It can be your project partner or someone else.

Write a function called
`hasTargetSum` that receives two arguments
  * an array of integers
  * a target integer
The function should return all pairs of numbers found in the array that add to the target number.

Ex:
```
```js
// Example 1:
hasTargetSum([-1, 3, 8, 12, 4, 11, 7], 10)

// should return
// [[-1, 11], [3, 7]]

// it is fine if you return repeats
// ex:
// [[-1, 11], [3, 7], [11, -1], [7, 3]]

// Example 2:
hasTargetSum([22, 19, 4, 6, 30, -6], 25)
// returns
// [[19, 6]]
// or [[19, 6], [6, 19]]
```

## Lecture Notes

### Step 1: Define Algorithm
Stress that Big O is a pretty intuitive concept, promise students that they already have an understanding of the concept. (Specifically, you can really hammer home what "linear time" means, everyone sort of already gets that even if they don't know it). Unfortunately, it is surrounded in a bunch of language and terminology that can be intimidating and obfuscatory.

Big O is a way to compare algorithms to each other. It doesn't make sense to compare, for example, the number of seconds some code takes to run because that would be different on different computers. It also would be different given different inputs. So we'll need some way to describe algorithms in relation to each other in a pretty abstract and high-level way.

But first! let's define the term.

**Q:** What does 'algorithm' mean?

**A:** A procedure; a set of instructions.


### Examples

**Cute Examples**

Give some goofy/easy examples, reinforcing terms like "procedure" and "instructions" for example the algorithm for washing your hair:

```
lather
rinse
repeat
```

Now obviously that's not the full algorithm because that's an infinite loop. The full procedure is like

```
while (hair != clean) {
  lather()
  rinse()
}
```

Next, these are useful algorithms to give as examples as they will come up later in the lecture.

**Finding a Word in a Dictionary Example**
```
open up the dictionary, is the word you're looking for alphabetically before the words on the page or after?

if (before) {
  open up the dictionary to somewhere before the current page
} else if (after)  {
  open up to some page after the current page
}

repeat
```

Next, step through the algorithm for a simple O(n) method like Ruby's `.find`. This will start transitioning you into the next part of the lecture.

## Step 2: Linear Time

**Linear Time `find` Example**

Make an array of 10 numbers, talk through in excruciating detail what happens each step of the way. Go slowly.

```ruby
arr = (1..10).to_a

arr.find do |n|
  n == 3
end
```
> Ex: "`find` is an enumerable method so we start iterating. The first element, `1` is yielded to the block. The block variable `n` is now bound to the value `1`. Did we meet the condition: is `1` equal to `3`? Nope, ok, next element. Now the second element `2` is yielded to the block, `n` is now bound to the value `2`. Is `2` equal to `3` -> no -> next elem. Now `3` is yielded to the block. Is `3 == 3` ok, great, stop and return the found element."

Go through a few more examples, what happens if the number you were trying to find was `7`, what if it was `1`.

**Q:** What's the worst case scenario?

**A:** people will usually say `10`, but what about: `11` or `1001` or `"whatever"`. In Big O we sort of only care about this worst case scenario because we only care about a high level summary of the algorithm. We don't care that sometimes it might find the element on the first try, or sometimes on the third try or whatever, or sometimes not at all; we just care about describing the process from a high level.

> "Ok, so if we had an array of 10 things the worst case scenario is our computer has to look at 10 numbers, do 10 units of work. If we had an array of 100 things, the worst case is the computer looks at 100 things. Ok, so if we had an array of _n_ things, how many elements would our computer have to look at in the worst case? _n_."

**Q:** What would this look like if we tried to draw this out on a graph

**A:** Do your best to draw a _perfectly straight line_. This is, like, the hardest part of the lecture.

Conclude that this is called "big o of n" aka `O(n)` aka "linear time".

Why "linear time"? Because of what we just drew out on the graph. As the size of the input grows the algorithm will take more time directly linearly proportionally.

## Step 3: Drop the constant factors
### in Big O we just care about a high level summary

It's worth taking a break to make this point. It can be confusing. Ask students to think through writing out a few methods.

The task is to take the average of an array of numbers and then return `true` or `false` if the array contains the average number

```ruby
def average(arr)
  # complete this code together
  # ...
end

def contains_avg?(arr)
  # complete this code together
  avg = average(arr)

  arr.includes { |n| n == avg }
end

a = [2,3,4,5,6]
b = [2,3,4,5,100]

avg(a)
# => returns `true`
avg(b)
# => returns `false`
```

**Q**: What's the Big O of this?

**(Tentative) A:** Well... ok, the `.includes` part is O(n), it's the exact same algorithm as .find, we look at the first elem, is it what we are looking for, if not, look at the next, etc...

What about the `average` part? The "get the sum of all the numbers part" seems also to be O(n), which makes sense as a bigger input array would mean my computer has to do more stuff...

Then we have to always divide by the length at the end so that's like, maybe + 1 thing the computer has to do...

```ruby
def contains_avg?(arr)
  avg = average(arr) # so this is O(n) + 1 maybe?

  arr.includes { |n| n == avg } # this part is O(n)
end
```

so.. is the Big O of this?

```
O(2n + 1)
```

Seems reasonable, people will nod their heads...

But **No!**. This is not the right answer. You might think you're being extra precise by breaking down the problem like this, but this is not what Big O is about. Big O is a very high level summary of the algorithm. This is still O(n) aka linear time. The fancy way of putting this is that in Big O we drop the constant factor. Take out your red pen and cross out anything that is constant aka does not depend on the size of the input `n`. The `2` and the `+ 1` go away.

`2n` would be a slightly steeper line on our graph, but it is still a line. Big O is concerned about patterns on the graph that will be drastically different than straight lines. Let's take a look at one of those now.

## Step 4: Logarithmic Time

Thought Experiment: Ask students to imagine the world's worst dictionary, one in which there is no pattern to the order of the words. They are randomly ordered.

**Q:** What is the big o of finding a word in this horrible dictionary?

**A:** O(n). This is the same algorithm as we have been discussing all along. Look at the first word, is it the word we want? nope, ok, look at the next word. Is it the word we want? nope, ok, look at the next, etc.

In this world's worst dictionary, clearly finding a word in a "dictionary" with 10 words would take a lot less time than in a dictionary with 10,00 words.

But is this how a real alphabetized dictionary works? Clearly we could find a word in a dictionary with 100 words faster than in a dictionary with 100,000 words.

**Q:** But would it take us significantly longer to find the word we're looking for in a dictionary with 101,000 words vs. the 100,000 word dictionary?

**A:** Not really. There's definitely some relationship to the size of the dictionary and the time it would take, but it's not 1:1 linear.

Thinking about this algorithm further, let's say we:
```
open the dictionary to the exact middle word.
is the word we want before it or after it
if (before) {
  basically throw out the last half of the dictionary.
  If our word exists it cannot be in that half

  look at middle word of first half
  repeat this procedure
} else if (after) {
  basically throw out the first half of the dictionary.
  If our word exists it cannot be in that half

  look at middle word of last half
  repeat this procedure
}
```

Notice that each time we look at a word, the size of our input gets smaller by half. We divide by 2, divide by 2, divide by 2, etc.

What would this look like on our graph?

Draw out a very slow growing line.

This is called "logarithmic time" aka `O(log n)`.

Stress to students that you _truly do not need to know much about math to understand this._ You need to know that _logarithm means slow growing_.

If they care to know, it's really `log base 2` and that 2 comes from the "cut in half; cut in half; cut in half" part of the procedure, but this is usually left off.

This procedure can drastically decrease the runtime of algorithms, let's analyze this further.

## Step 5: Not exactly sorting, but why sorting is important
### Let's add a ton of `console.log`s to some code

I use this repo [https://github.com/alexgriff/intro-to-sorting](https://github.com/alexgriff/intro-to-sorting) and run the code in the chrome console.

Specifically,
* this file which has both a binary search and linear search implemented [https://github.com/alexgriff/intro-to-sorting/blob/master/binarysearch.js](https://github.com/alexgriff/intro-to-sorting/blob/master/binarysearch.js)
* and this file which makes randomly generated arrays of various lengths [https://github.com/alexgriff/intro-to-sorting/blob/master/randomArray.js](https://github.com/alexgriff/intro-to-sorting/blob/master/randomArray.js)

First, inspect the `arr` of 1000 random numbers and the `sortedArr1` of 1000 sorted numbers.

Run the `linearSearch` function on each and see the `console.log`s run, passing in a target element that is not in the array if you want to see the worst case.

```js
const linearSearch = (arr, target) => {
  for (let i = 0; i < arr.length; i++) {
    console.log(`Remaining elements to search: ${arr.length - i}`);

    const elem = arr[i];
    if (elem === target) {
      return target;
    }
  }
  return false;
};
```

Ask students to analyze the runtime of this.

**Q:** Did the array being sorted affect the performance of the algorithm?

**A:** No. We still just looked at the first, checked it, looked at the next, checked it, etc.

But if we do have a sorted array, we can do better than linear time. Think of the dictionary, we know there is a pattern to arrangement of the elements, they're in alphabetical order, so we can use that knowledge to make choices, which half does the element live in.

Take a look at the following code, it's _recursive_ and we wont exactly cover that here, but it exactly follows the algorithm we described. (it's ok to gloss over most of this, the important parts are the `console.log`s)

```js
const binarySearch = (arr, target) => {
  console.log(`Remaining elements to search: ${arr.length}`);

  const midpoint = Math.floor(arr.length / 2);
  const middle = arr[midpoint];

  if (arr.length <= 1) {
    return middle === target ? target : false;
  }

  if (middle === target) {
    // we found the thing
    return target
  } else if (middle >= target) {
    // let's look in the first half
    return binarySearch(arr.slice(0, midpoint), target);
  } else if (middle < target){
    // let's look in the second half
    return binarySearch(arr.slice(midpoint), target);
  }
};
```

Run `binarySearch` on a worst case scenario for the `sortedArr1`.  There are so many less console.log's! the array is _cut in half_ each time!

Here's a really crazy thing: Check out `sortedArr2`, it is double the size of `sortedArr1`. With the `linearSearch` function we would, therefore, double the number of logs. What happens with the `binarySearch` is **1 additional console log**.

_The size of the input (`n`) has to double in size in order for our computer to have to do 1 more unit of work._ This is the very slow growing (logarithmic) line we see on the graph.

What would a very fast growing line on the graph look like?

## Step 6: Analyze Prerequisite problem.

Have a student describe from a high level the procedure they or their group used to solve this problem.

I'll jot this down, and usually it looks something like this.

(In theory if the volunteer described the linear time solution I would probably pick another group, but it generally works out pretty straightforwardly where people have O(n^2) code)

```
Make an empty array `results`

Look at each element (i)

try to combine it with every other element (j)

if (i + j == the target) {
  add the pair to the results array
}

return results
```

After confirming people understand this from a high level supply the functioning code.

```js
const hasTargetSum = (arr, target) => {
  const results = []

  for (let i = 0; i < arr.length; i++) {
    const current = arr[i]

    for (let j = 0; j < arr.length; j++) {
      const next = arr[j]

      if (current + next === target) {
        arr.push([current, next])
      }
    }
  }

  return results
}
```

**Q:** What's the big o of this?

**A:** Hmmm.. for every element of the array (that's `O(n)`) we have to try to combine it with _every other element of the array_ (also `O(n)`). So if we had an 8 element array for the first element I would attempt to add it to 8 other things. Then for the second element, I would add it to all 8 other elements, same for each element, so this puts as at 8 * 8 aka 64 comparisons.

This is `O(n^2)` aka "O of n squared" aka "quadratic time" and is very _fast growing_. Doubling the size of the input (`n`) _quadruples_ the amount of work the computer has to do. 5 squared is 25, 10 squared is 100.

Draw it out on the graph.

*The TL;DR of this entire lecture is if you see a loop inside of a loop it's O(n^2). Though there are some important talking points around this discussed further below.*

#### Aside on optimization
The reason students often get duplicates in their outputs (`[[3,7], [7,3]]`) is that they are attempting to combine each element with every single other one. Really, they just need to compare each element to the remaining elements they have not seen.

Simply changing the inner loop to start at the next element, will remove the duplicates in the outputs

```js
for (let j = i + 1; j < arr.length; j++)  { /* ... */ }
```

**Q:** What's the Big O of this though?

**A:** It is still quadratic time. You might want to quibble and be like this isn't fully `n` times `n`, but in fact it's `n` times `n minus a little`. But that `minus a little` is exactly the "constant factor" we don't care about in Big O. Just because we haven't changed the big o doesn't mean optimizations like this are not worth making :)

## Step 7: Iteratively approach a linear time solution

There is a way to solve this problem in linear time. Aka _one loop_.

In computer science everything is a tradeoff. If we want this to take up less _time_ we will have to take up more _space_.  This is often a totally fine tradeoff to make. Memory is cheap.

Let's begin by allocating an extra auxiliary data structure in memory. We'll store the numbers we have already seen and then check if the number we want is a number we have already seen.

The insight is that if the target is `10` and the current num is `6`, I want to know if I have already seen a `4` (ie `target - current`; `10 - 6`).

```js
const hasTargetSum = (arr, target) => {
  const results = []
  const numsIHaveAlreadySeen = []

  for (let i = 0; i < arr.length; i++) {
    const current = arr[i]
    const numIWant = target - current

    if (numsIHaveAlreadySeen.includes(numIWant)) {
      arr.push([current, numIWant])
    }

    numsIHaveAlreadySeen.push(current)
  }

  return results
}
```
**Q:** Did we change the Big O? Is there a loop inside of a loop?

**A:** Nope, we did not-- There are still 2 loops! It may seem like there are not, but what is the algorithm for that inner `.includes` we see in there? That's that same linear time algorithm we have discussed all along. It's nice to point out that especially in Ruby these loops can often be hidden from us. Every time you see a `.compact` or a `.uniq`... that's a loop.

But this refactor gave us an interesting idea...

Asking an array the question, "hey array do you contain an element?" with `.includes` was O(n). What are some other data structures we can ask questions of to see if they contain values?

## Step 8: Discuss Hashes and Constant Time

Poll that everyone is comfortable with asking a question of a hash or object.

"hey hash do you have a value at this key?"

```ruby
hash = {name: "Ian"}
hash[:name]
# => Ian

hash[:something_else]
# => nil
```

**Q:** Does anyone happen to know what's the big o of asking a hash this question?

**A:** It's _Constant Time_, we write this as `O(1)`. This means determining if a hash has a key in it works independently of the number of elements in the hash. A hash with 10 key value pairs or a hash with 10,000 key value pair can has equally fast look ups.

Let's do a brief digression on how this works.

Imagine a big messy pile of clothes [google image](https://www.google.com/search?rlz=1C5CHFA_enUS767US767&biw=1280&bih=652&tbm=isch&sa=1&ei=WJz-XPjjFOTs_Qb43p3YDA&q=big+pile+of+clothes&oq=big+pile+of+clothes&gs_l=img.3..0.7701.7701..8179...0.0..0.63.63.1......0....1..gws-wiz-img.gqw8JnFZtgA#imgrc=ZXaPip3IIBNWJM:)

What's the big O of finding your the missing sock you need in this pile? It's O(n), pull out one element, is that the sock? nope, discard and pull out the next, is that the sock? nope, etc.

Now imagine some Marie Kondo style drawer of very neat clothes [google image](https://www.google.com/search?rlz=1C5CHFA_enUS767US767&biw=1280&bih=652&tbm=isch&sa=1&ei=6Jz-XLXnA-S3gge8sokI&q=very+neat+clothes+drawers&oq=very+neat+clothes+drawers&gs_l=img.3...5824.6732..7040...0.0..0.64.449.8......0....1..gws-wiz-img.7CZYRdwRrYU#imgdii=EQ7I7x3U-xo-LM:&imgrc=ZMIGcnC7_8vswM:)

Because every item has a designated place to live, we can do much better than O(n).

Now imagine that you could have an infinite number of infinitely small drawers and a perfect system of remembering such that each item had one and exactly one drawer it could belong in if it exists. We could effectively say that finding the sock is independent of the total number clothes because there would be one designated drawer for the sock to be in if it existed. This is what constant time means.

Getting back to computers, the way this is done is with a _hash function_ works (hence the term 'hash'). When you ask a hash the question of wether it has a certain key it runs the key through a _hash function_ which corresponds to one place in memory where that value must exist if it is present in the hash.

So let's get rid of that inner loop.

## Step 9: Complete Refactor

So remember that the inner loop came from asking an array the question `includes`. Let's change that so we ask a hash. Spend some time showing that we don't really care what the value of the key value pair is. We just want a quick way to know if we have seen the element already or not.

```js
const hasTargetSum = (arr, target) => {
  const results = []
  const numsIHaveAlreadySeen = {}

  for (let i = 0; i < arr.length; i++) {
    const current = arr[i]
    const numIWant = target - current

    if (numsIHaveAlreadySeen[numIWant]) {
      arr.push([current, numIWant])
    }

    numsIHaveAlreadySeen[current] = true
    console.log(numsIHaveAlreadySeen); // helps students visualize what is happening
  }

  return results
}
```

Now we truly have 1 loop which means this operates linear time. We did this by making a decision to make a tradeoff, we took up more space, allocated more memory for this extra hash.

Discuss when and why you might or might not decide to make this tradeoff.

## Step 10: Why Do We Care & Assign Practice Problem

There are tons of interview type problems that have both a quadratic time solution and a more optimal linear time solution. You can apply this strategy in a lot of places...

### BUT...
### DO NOT PREMATURELY OPTIMIZE...

The _wrong_ takeaway from this lecture would be:

> uh oh im thinking of a solution that would mean a loop inside of a loop and i heard that's bad so there must be a better solution so im going to let that stop me from writing any functioning code.

You would mark yourself as an _amazing_ candidate if you instead said something like

> ok, for the solution i want to attempt i want to compare every element to each other element. This will mean the function will run in O(n^2) time. I want to start writing that code, get it working, and then see if we can refactor to linear time. Usually you can do that by creating a hash to store data so that's where i would start with that.

Even if you didn't fully get to the refactored solution you still did an amazing job articulating yourself.

Here's an example problem where you can apply this same strategy. Start with the quadratic time solution and refactor to linear time by using a hash!
[Ransom Note Problem](https://gist.github.com/alexgriff/0061bd3ff76c06341a62489899890b01)

+++
title = "Day 2 - Rock Paper Scissors"
date = "2022-12-01T00:03:04+01:00"
author = "atla"
authorTwitter = "@atla_" #do not include @
cover = ""
tags = ["AoC2022", "Calorie Counting"]
keywords = ["AoC2022"]
description = ""
showFullContent = false
readingTime = false
hideComments = false
+++

On day 2 (see https://adventofcode.com/2022/day/2) we are tasked to help the elves decide who gets to sleep next to the snack store. And to do that the elves start playing the classic game of Rock Paper Scissors - the winner gets to tent up close to the snacks!

And as a single round is not sufficient they actually play multiple rounds. To win here we need a clever strategy guide. Of course it happens that this strategy guide is your puzzle input! 

See an example puzzle input below:
```go {linenos=table, style=dracula}
A Y
B X
C Z
```

## Part 1
Now in our Part 1 we are actually interpreting the puzzle input as pairs of player choices of rocks, paper or scissors and we assign a value that is the sum of both points for the choice (rock=1, paper=2, scissors=3) and the result of the game (we lost= 0, we draw = 3, we won = 6). That being said all that is left is a small iteration involving a lookup table because all combinations can easily be predefined.

With that given, a simple solution goes as follows:

```go {linenos=table, style=dracula}
lookup := map[string]int{
	"A X": 4, "A Y": 8, "A Z": 3, // 1+3, 2+6, 3+0
	"B X": 1, "B Y": 5, "B Z": 9, // 1+0, 2+3, 3+6
	"C X": 7, "C Y": 2, "C Z": 6, // 1+6, 2+6, 3+3
}
if data, err := ioutil.ReadFile("input.txt"); err == nil {
	points, rounds := 0, strings.Split(string(data), "\n")
	for _, round := range rounds {
		points += lookup[round]
	}
	fmt.Println("My total score according to the given strategy
	guide is: ", points)
}
```

## Part 2

So part 2 spices things up - at least a little bit - actually in part 1 the elf giving you instructions about the puzzle input didn't tell you what the second column (X,Y,Z) was for. It was actually not your hand gesture but the outcome of the round where X is lose, Y is draw and Z is a win. Given that our task is basically to transform the input we get before we use it to run it with the same code as above.

So all that is left is defining a second lookup table and passing that one in before passing it to the lookup table from part 1.

And here goes the code:


```go {linenos=table, style=dracula}
prune := map[string]string{
	"A X": "A Z", "A Y": "A X", "A Z": "A Y",
	"B X": "B X", "B Y": "B Y", "B Z": "B Z",
	"C X": "C Y", "C Y": "C Z", "C Z": "C X",
}
if data, err := ioutil.ReadFile("input.txt"); err == nil {
	points, rounds := 0, strings.Split(string(data), "\n")
	for _, round := range rounds {
		points += lookup[prune[round]]
	}
	fmt.Println("My total score according to NEW rules is: ", points)
}
```

And last but not least, of course you can find the solution also directly in the github repo. Click here: [https://github.com/atla/go-aoc-2022/blob/main/day2/main.go](https://github.com/atla/go-aoc-2022/blob/main/day2/main.go)

Again we get another two stars (yay!) and keep waiting till day 3 tomorrow.
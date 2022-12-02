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
draft = true
+++

On day 2 (see https://adventofcode.com/2022/day/2) we are tasked helping the elves decides who gets to sleep next to the snack store. And to do that the elves start playing the classic game of Rock Paper Scissors - the winner gets to tent up close to the snacks!

And as a single round is not sufficient enough they actually play multiple rounds. To win here we need a clever strategy guide. Of course it happens that this strategy guide is your puzzle input! 

See an example puzzle input below:


## Part 1

In the first part we are confronted with a puzzle input of single numbers per line (calories) split in multiple lines (all calories carried by a single elf) with empty newlines in between. As an easy first task to get off the ground we have to find the elf carrying the most amount of calories (sum of all lines for a single elf) - and sent in the number.6

puzzle input example:
```go {linenos=table, style=dracula}
A Y
B X
C Z
```



Today i didnt really abstract the file loading or build any re-use functions for the next days, plain old and simple code for day 1 goes here:

```go {linenos=table, style=dracula}
if data, err := ioutil.ReadFile("input.txt"); err == nil {
		input := string(data)
		elves := strings.Split(input, "\n\n")
		max := -1
		for _, elv := range elves {
			current := 0
			for _, food := range strings.Split(elv, "\n") {
				f, _ := strconv.Atoi(food)
				current += f
			}
			max = int(math.Max(float64(max), float64(current)))
		}

		fmt.Println("Elv with most calories caried is carrying:", max,
		 "calories.")
	}
```

## Part 2

Now as a small additionall numbers - and because the food of a single elf doesn't cut it - we want to know the calories of the top three carrying elves combined. So instead of finding the max value we add all elves to a slice, sort it and sum up the top three. Fair enough, code goes here:


```go {linenos=table, style=dracula}
if data, err := ioutil.ReadFile("input.txt"); err == nil {
		input := string(data)
		elves := strings.Split(input, "\n\n")
		values := []int{}
		for _, elv := range elves {
			current := 0
			for _, food := range strings.Split(elv, "\n") {
				f, _ := strconv.Atoi(food)
				current += f
			}
			values = append(values, current)
		}
		sort.Slice(values, func(i, j int) bool {
			return values[i] > values[j]
		})

		fmt.Println("Elv with most calories caried is carrying:",
			func(vals []int) int {
				s := 0
				for _, i := range vals {
					s += i
				}
				return s
			}(values[0:3]),
			"calories.")
	}

```

And last but not least, of course you can find the solution also directly in the github repo. Click here: [https://github.com/atla/go-aoc-2022/blob/main/day1/main.go](https://github.com/atla/go-aoc-2022/blob/main/day1/main.go)

Finally we grab the two star (fruits!) and move on! Let's see what is in store for day 2 tomorrow.
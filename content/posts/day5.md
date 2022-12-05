+++
title = "Day 5 - Supply Stacks"
date = "2022-12-05T00:03:04+01:00"
author = "atla"
authorTwitter = "@atla_" #do not include @
cover = ""
tags = ["AoC2022", "Supply Stacks"]
keywords = ["AoC2022"]
description = ""
showFullContent = false
readingTime = false
hideComments = false
+++
Our challenge for day 5 (see https://adventofcode.com/2022/day/5) is to determine which crate will end up on top of each stack after the rearrangement procedure is complete. The starting configuration of the crates is given, with each stack represented by a line of square brackets containing the letters of the crates in the stack from bottom to top. A series of instructions is then given that describe how to rearrange the crates by moving a specified quantity of crates from one stack to another stack. We must determine the top crate in each stack after all of the instructions have been executed. In the example, the top crates are C in stack 1, M in stack 2, and Z in stack 3, so the message is CMZ.

See an example puzzle input below:
```go {linenos=table, style=dracula}
    [D]    
[N] [C]    
[Z] [M] [P]
 1   2   3 

move 1 from 2 to 1
move 3 from 1 to 3
move 2 from 2 to 1
move 1 from 1 to 2
```

## Part 1

As already explained above in the general description our task is to properly parse todays (a bit more complex) input structure and carefully prepare a field with stacks as well as a list of instructions. The actualy processing of the instructions modifying the stacks is the easiert part.

The code goes as follows:
 
```go {linenos=table, style=dracula}

type Stack []rune
type Field struct {
	Columns []Stack
	Size    int
}

func makeField(cols int) *Field {
	columns := []Stack{}
	for i := 0; i < cols; i++ {
		columns = append(columns, Stack{})
	}
	return &Field{
		Columns: columns,
		Size:    cols,
	}
}

func (s *Stack) IsEmpty() bool {
	return len(*s) == 0
}
func (s *Stack) Push(r rune) {
	*s = append(*s, r)
}

func (s *Stack) PushMany(r ...rune) {
	*s = append(*s, r...)
}

func (s *Stack) Pop() (rune, bool) {
	if s.IsEmpty() {
		return rune(' '), false
	} else {
		idx := len(*s) - 1
		r := (*s)[idx]
		*s = (*s)[:idx]
		return r, true
	}
}

func (s *Stack) PopMany(count int) ([]rune, bool) {
	if s.IsEmpty() {
		return []rune{}, false
	} else {
		idx := len(*s)
		r := (*s)[idx-count : idx]
		*s = (*s)[:idx-count]
		return r, true
	}
}

func main() {
	fmt.Println("2022 Advent of Code Day 5")

	fmt.Println("--- Part 1 ---")

	findTopElements(func(field *Field, amount int, from int, to int) *Field {
		for p := 0; p < amount; p++ {
			r, _ := field.Columns[from-1].Pop()
			field.Columns[to-1].Push(r)
		}
		return field
	}, 9)
}

func findTopElements(move func(field *Field, amount int, from int, to int) *Field, cols int) {

	if data, err := ioutil.ReadFile("input.txt"); err == nil {
		input := strings.Split(string(data), "\n\n")		
		field := parseInitialStack(input[0], cols)		

		for _, ins := range strings.Split(input[1], "\n") {
			amount, from, to := parseInstruction(ins)
			field = move(field, amount, from, to)
		}
		field.printTopElements()
	}
}

func parseInstruction(ins string) (int, int, int) {
	split := strings.Split(ins, " ")
	v1, _ := strconv.Atoi(split[1])
	v2, _ := strconv.Atoi(split[3])
	v3, _ := strconv.Atoi(split[5])
	return v1, v2, v3
}

func (field *Field) printTopElements() {
	for _, e := range field.Columns {
		fmt.Print(string(e[len(e)-1]))
	}
	fmt.Println("")
}

func parseInitialStack(initialStack string, cols int) *Field {

	f := makeField(cols)

	lines := strings.Split(initialStack, "\n")
	for line := len(lines) - 2; line > -1; line-- {
		for i := 0; i < cols; i++ {
			r := rune(lines[line][i*4+1])
			if r != ' ' {
				f.Columns[i].Push(r)
			}
		}
	}
	return f
}

```

## Part 2

In part two of this challenge, the elves are using a different type of crane called the CrateMover 9001. This new crane has some exciting new features, like air conditioning and leather seats, but most importantly it has the ability to pick up and move multiple crates at once. This means that when multiple crates are moved from one stack to another, they will stay in the same order rather than being moved one at a time like in part one.

For example, in the given example, the initial configuration of the crates is the same as in part one. However, when three crates are moved from stack 1 to stack 3, they are moved together in the order D, N, C, resulting in the following configuration:

```go {linenos=table, style=dracula}
       [D]
       [N]
[C]    [Z]
[M]    [P]
 1   2  3
```
The remaining instructions are executed in a similar manner, resulting in the final configuration:

```go {linenos=table, style=dracula}
        [D]
        [N]
        [Z]
[M] [C] [P]
1    2   3
```

At the end of the rearrangement procedure, the elves want to know which crate is on top of each stack. In this example, the top crate in stack 1 is M, the top crate in stack 2 is C, and the top crate in stack 3 is D, so the elves would combine these together to get the message MCD.

To solve this challenge, we must update our simulation of the rearrangement process to account for the CrateMover 9001's ability to move multiple crates at once. 
See the according code below, this is all we had to change:

```go {linenos=table, style=dracula}
	fmt.Println("--- Part 2 ---")

	findTopElements(func(field *Field, amount int, from int, to int) *Field {
		r, _ := field.Columns[from-1].PopMany(amount)
		field.Columns[to-1].PushMany(r...)
		return field
	}, 9)
```


As always find the code here: [https://github.com/atla/go-aoc-2022/blob/main/day5/main.go](https://github.com/atla/go-aoc-2022/blob/main/day5/main.go)

Yay, another two stars for day 5. Lets keep on going!
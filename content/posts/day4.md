+++
title = "Day 4 - Camp Cleanup"
date = "2022-12-04T00:03:04+01:00"
author = "atla"
authorTwitter = "@atla_" #do not include @
cover = ""
tags = ["AoC2022", "Camp Cleanup"]
keywords = ["AoC2022"]
description = ""
showFullContent = false
readingTime = false
hideComments = false
+++

On day 4 (see https://adventofcode.com/2022/day/4) the challenge is to help the elves with camp cleanup. All elves are split into teams of two and tasks with two areas each to cleanup - a simple range of numbers. Clever elves as they are they soon figure out there are many overlaps in their assignments.

See an example puzzle input below:
```go {linenos=table, style=dracula}
2-4,6-8
2-3,4-5
5-7,7-9
2-8,3-7
6-6,4-6
2-6,4-8
```

## Part 1

To reduce any duplicated effort our first task is to find in all given pairs (all lines in the puzzle input) if any of the two elves working together actually have a complete overlap (e.g. elf1 or elf2 assignment is completely contained in the other).

In our puzzle input we find two such occasions - of course in the real puzzle input there are hundreds to be found. Overall using a dedicated structure for each elf and a simple contains function exercise 1 can be solved:
 
```go {linenos=table, style=dracula}

type elf struct {
	L int
	U int
}

func makeElf(input []string) elf {
	v1, _ := strconv.Atoi(input[0])
	v2, _ := strconv.Atoi(input[1])
	return elf{
		v1,
		v2,
	}
}

func (e1 elf) contains(e2 elf) bool {
	return e1.L <= e2.L && e1.U >= e2.U
}

func (e1 elf) intersects(e2 elf) bool {
	return (e1.L <= e2.L && e1.U >= e2.L) ||
		(e2.U >= e1.L && e2.U <= e1.U)
}

func main() {
	fmt.Println("2022 Advent of Code Day 4")
	fmt.Println("--- Part 1&2 ---")

	if data, err := ioutil.ReadFile("input.txt"); err == nil {
		duplicates, d2, pairs := 0, 0, strings.Split(string(data), "\n")

		for _, p := range pairs {
			e1, e2 := strings.Split(p, ",")[0], strings.Split(p, ",")[1]
			elf1, elf2 := makeElf(strings.Split(e1, "-")), makeElf(strings.Split(e2, "-"))
			if elf1.contains(elf2) || elf2.contains(elf1) {
				duplicates++
			}
			if elf1.intersects(elf2) || elf2.intersects(elf1) {
				d2++
			}
		}
		fmt.Println("Duplicate assignments: ", duplicates)
		fmt.Println("intersectign assignments: ", d2)
	}
}
```

## Part 2

Actually today part 2 is also directly inside the same code as part 1. The only difference is that we want to find intersections. The actual task is to find out if the elves have at least one intersecting area that they both have to clean up. So instead of only just checking for an complete overlap we added a second function to check for any intersection.


As always find the code here: [https://github.com/atla/go-aoc-2022/blob/main/day4/main.go](https://github.com/atla/go-aoc-2022/blob/main/day4/main.go)

Adding two more stars to our record and moving on to the next day...
+++
title = "Day 3 - Rucksack Reorganization"
date = "2022-12-03T00:03:04+01:00"
author = "atla"
authorTwitter = "@atla_" #do not include @
cover = ""
tags = ["AoC2022", "Rucksack Reorganization"]
keywords = ["AoC2022"]
description = ""
showFullContent = false
readingTime = false
hideComments = false
+++

Todays blog post is sponsored by ChatGPT ;) - solutions were still build by myself.

## Part 1

Once upon a time, in the land of Elves, there was a very important job to be done. The task was to load up all the rucksacks with supplies for a jungle journey. However, one Elf didn't quite follow the packing instructions and now some items need to be rearranged.

Each rucksack has two large compartments and all items of a given type are meant to go into exactly one of the two compartments. The mischievous Elf failed to follow this rule for exactly one item type per rucksack.

The Elves made a list of all the items in each rucksack, but they need help finding the errors. Every item type is identified by a single lowercase or uppercase letter. The list of items for each rucksack is given as characters all on a single line.

For example, suppose you have the following list of contents from six rucksacks:

vJrwpWtwJgWrhcsFMMfFFhFp
jqHRNqRjqzjGDLGLrsFMfFZSrLrFZsSL
PmmdzqPrVvPwwTWBwg
wMqvLMZHhHMvwLHjbvcjnnSBnvTQFn
ttgJtRGJQctTZtZT
CrZsJsPPZsGzwwsLwLmpwMDw
The first rucksack contains the items vJrwpWtwJgWrhcsFMMfFFhFp, which means its first compartment contains the items vJrwpWtwJgWr, while the second compartment contains the items hcsFMMfFFhFp. The only item type that appears in both compartments is lowercase p.
The second rucksack's compartments contain jqHRNqRjqzjGDLGL and rsFMfFZSrLrFZsSL. The only item type that appears in both compartments is uppercase L.
The third rucksack's compartments contain PmmdzqPrV and vPwwTWBwg; the only common item type is uppercase P.
The fourth rucksack's compartments only share item type v.
The fifth rucksack's compartments only share item type t.
The sixth rucksack's compartments only share item type s.
To help prioritize item rearrangement, every item type can be converted to a priority:

Lowercase item types a through z have priorities 1 through 26.
Uppercase item types A through Z have priorities 27 through 52.
In the above example, the priority of the item type that appears in both compartments of each rucksack is 16 (p), 38 (L), 42 (P), 22 (v), 20 (t), and 19 (s); the sum of these is 157.

The Elves need to find the item type that appears in both compartments of each rucksack and calculate the sum of the priorities of those item types. Will they be able to solve this rucksack reorganization problem and save the day?

And here goes the code for Part 1:

```go {linenos=table, style=dracula}
func priority(r rune) int32 {
	if unicode.IsUpper(r) {
		return r - 'A' + 27
	}
	return r - 'a' + 1
}

func main() {
	fmt.Println("2022 Advent of Code Day 3")
	fmt.Println("--- Part 1 ---")

	if data, err := ioutil.ReadFile("input.txt"); err == nil {
		sum, backpacks := 0, strings.Split(string(data), "\n")
		for _, backpack := range backpacks {
			items := map[rune]bool{}
			for i, r := range backpack {
				if i < len(backpack)/2 {
					items[r] = true
				} else {
					if _, ok := items[r]; ok {
						sum += int(priority(r))
						//fmt.Println("Duplicate is ", strconv.QuoteRune(r), " (", priority(r), ")")
						break
					}
				}
			}
		}
		fmt.Println("Sum is ", sum)
	}
```

## Part 2

But that's not all, the Elves are divided into groups of three and each group carries a badge that identifies them. For efficiency, the badge is the only item type carried by all three Elves in the group. That means, if a group's badge is item type B, then all three Elves will have item type B somewhere in their rucksack, and at most two of the Elves will be carrying any other item type.

The problem is that someone forgot to put this year's updated authenticity sticker on the badges. All of the badges need to be pulled out of the rucksacks so the new authenticity stickers can be attached. The only way to tell which item type is the right one is by finding the one item type that is common between all three Elves in each group.

Every set of three lines in the list corresponds to a single group, but each group can have a different badge item type. So, in the above example, the first group's rucksacks are the first three lines:

vJrwpWtwJgWrhcsFMMfFFhFp
jqHRNqRjqzjGDLGLrsFMfFZSrLrFZsSL
PmmdzqPrV

In the first group, the only item type that appears in all three rucksacks is lowercase r; this must be their badges. In the second group, their badge item type must be Z. Priorities for these items must still be found to organize the sticker attachment efforts: here, they are 18 (r) for the first group and 52 (Z) for the second group. The sum of these is 70.

The Elves now need to find the item type that corresponds to the badges of each three-Elf group and calculate the sum of the priorities of those item types. Will they be able to solve this issue and attach the new authenticity stickers to the badges? It's up to them to save the day and make sure the jungle journey goes smoothly.

```go {linenos=table, style=dracula}
if data, err := ioutil.ReadFile("input.txt"); err == nil {
		sum, backpacks := 0, strings.Split(string(data), "\n")

		for i := 0; i < len(backpacks)/3; i++ {
			group, items1, items2 := backpacks[i*3:(i+1)*3], map[rune]bool{}, map[rune]bool{}
			for _, r := range group[0] {
				items1[r] = true
			}
			for _, r := range group[1] {
				items2[r] = true
			}
			for _, r := range group[2] {
				_, ok1 := items1[r]
				_, ok2 := items2[r]
				if ok1 && ok2 {
					sum += int(priority(r))
					//fmt.Println("Duplicate is ", strconv.QuoteRune(r), " (", priority(r), ")")
					break
				}
			}
		}
		fmt.Println("Sum is ", sum)
	}
```

And last but not least, of course you can find the solution also directly in the github repo. Click here: [https://github.com/atla/go-aoc-2022/blob/main/day3/main.go](https://github.com/atla/go-aoc-2022/blob/main/day3/main.go)

And of course we got another two stars today!
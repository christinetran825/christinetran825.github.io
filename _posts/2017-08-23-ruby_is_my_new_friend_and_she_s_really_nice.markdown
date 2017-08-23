---
layout: post
title:  "Ruby is my new friend, and she’s really nice. :]"
date:   2017-08-23 13:05:38 -0400
---


As I mentioned before in my first post, I was practicing learning how to code through Flatiron’s bootcamp prep courses and Codecademy. The prep courses were primarily on Javascript. Boy, that language is tough to learn and execute. When I enrolled to the Full Stack course, I started from the beginning with the introduction of programming and the Ruby language.

Ruby is truly my new friend. It’s super easy to understand and write. It gave me a better understanding of the basics of Javascript from the prep courses. What’s funny is that I think my 8th grade computer science class may have worked with a language similar to Ruby.

I enjoy the videos included within some of the Ruby lessons. They weren’t long and very easy to follow along with the lesson itself. Sometimes you just need a lot more visuals and a live action view of how codes are written and executed to understand how things work.

What I have difficulty most so far with Introduction to Ruby was learning how to read and write tests and spec files.

So far my biggest question was whether or not when writing our code or definition in the main file (../fizzbuzz.rb) are we supposed to the follow the exact layout of the spec file (fizzbuzz_spec.rb)? Meaning does the overall code layout in the spec file must be written exactly as it was listed?

**SPEC CODE:**

```
describe "fizzbuzz" do
  it 'returns "Fizz" when the number is divisible by 3' do
    fizz_3 = fizzbuzz(3)

    expect(fizz_3).to eq("Fizz")
  end
  it 'returns "Buzz" when the number is divisible by 5' do
    fizz_5 = fizzbuzz(5)

    expect(fizz_5).to eq("Buzz")
  end
  it 'returns "FizzBuzz" when the number is divisible by 3 and 5' do
    fizz_15 = fizzbuzz(15)

    expect(fizz_15).to eq("FizzBuzz")
  end
  it 'returns nil when the number is not divisible by 3 or 5' do
    fizz_4 = fizzbuzz(4)

    expect(fizz_4).to eq(nil)
  end
end
```

**MY CODE:**

```
def fizzbuzz(number) 
  if number % 3 == 0 "Fizz"      #spec said '...number is divisible by 3' first
  elsif number % 5 == 0 "Buzz"      #spec said '...number is divisible by 5' second
  elsif number % 3 == 0 && number % 5 == 0 "FizzBuzz"        #spec said '...number is not divisible by 3 or 5' third
  end             #expect(fizz_4).to eq(nil) last expectation is nil/false
end
```

When running my code it didn’t pass. I thought I had written the code according to the expectations.

I reached out for help in the “Ask A Question” field about this and got a great response, even though I had a hard time comprehending it at first. Here’s a recap of what I’ve learned.

We shouldn’t worry about what the program does. Codes are read from top to bottom, left to right. The spec file is for the developer who has expectations in which they expect the code to behave the way they intended. Our goal for these labs is to test these codes to get pass. The way or convention in which the spec file is written doesn’t have to be followed explicitly. However, the more dry and concise and clear the program reads, the better.

Here's the correct code:

**MY CODE:**

```
def fizzbuzz(number) 
  if number % 3 == 0 && number % 5 == 0 "FizzBuzz"   #switch the arrangement of this code so it can implicitly return nil/false.
  elsif number % 3 == 0 "Fizz"
  elsif number % 5 == 0 "Buzz"
  end
end
```


That new set passed everything. It implicitly returns nil. I had to switched the arrangement of the conditionals to get rid of the wordy nil statement (elsif number % 3 == 0 && number % 5 == 0 "FizzBuzz") if placed before the end statement of the if block.
The spec file just threw me off. The file just says I expect this, this, and that. Since I was working on the bootcamp prep courses, I was following along their test files, to make the code pass. That was a poor way of going about writing or testing codes. I guess I was in a Javascript mindset. Reading tests are very important. It's not stated that the spec file "directions" should have to be followed explicitly.



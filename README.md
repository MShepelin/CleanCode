# Clean Code

I was given a task to refactor a file (from an initial commit) and describe the refactoring process

### Codebase Organization

The initial code is a cpp file with around 1000 lines, which contain global definitions (in the form of enum, for example), includes, class definitions and methods implementations, the "main" function. First thing to mention - it is a bad practice to put everything in one cpp file as it is difficult to observe. It is true that modern IDE supports screen splitting which allows us to observe the same file from different view, but you still end up looking at two separate sections of codebase in the same. However, if you create a header file, for each class (or one for a logically connected group of classes) and several cpp files (one for each class and one for the "main" function) the picture will be very clear and readable.

### Style

What we see here is an inconsistency which makes a potential new programmer ask additional "Why?" question instead of writing a line of code. But let's be serious, inconsistent design really erases more questions then answers and makes the code less readable.

![2021-09-29_19-56-00](/2021-09-29_19-56-00.png)![2021-09-29_19-48-47](/2021-09-29_19-48-47.png)

Using namespace std is also not recommended, because sometimes we want to introduce custom string, vector or unordered_map implementations.

### Encapsulation and bracket-to-bracket range

All functions should fit on the monitor screen. Otherwise, they are difficult to read, refactor and maintain. The source code has a huge problem with this concept as some of the methods are around 200 lines of code long.

![2021-09-29_18-53-42](/2021-09-29_18-53-42.png)

As a part of this task several other functions with readability issues were fixed. To be precise, the "main" function length was greatly reduced with the usage of encapsulation.

<img src="/2021-09-29_18-56-25.png" alt="2021-09-29_18-56-25" style="zoom:67%;" />

Another point of refactoring is to make the logic of the program more readable. For example, there was a long if-else check, which was very difficult to understand by the first look, although the meaning of it was fairly straightforward.

| Before                                                       | After                                                        |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| <img src="/2021-09-29_19-15-48.png" alt="2021-09-29_19-15-48" style="zoom:50%;" /> | <img src="/2021-09-29_19-23-09.png" alt="2021-09-29_19-23-09" style="zoom:67%;" /> |

In fact, this usage of a static unordered map can be used in several other places (in the function "parse_group" for example).

Although technically it is difficult to call it "encapsulation", it is a good practice to join member variables in groups to form structs and avoid a picture like this one:

<img src="/2021-09-29_19-39-06.png" alt="2021-09-29_19-39-06" style="zoom:50%;" />

In the picture bellow we can join is_offline (which was renamed, but will talk about it later), sets_marked, skip, skip_if_not_judges etc to form something like GroupStats or GroupStatus.

It is also a nice idea to use std::ostream instead of printf and include it function input. What if we need this code to work with a different output system and we need a generalized approach?

### Names

In the previous section we discussed functions, so let's continue with function names. Although it is more preferable to use short names, long names are still better then non-obvious or non-general abbreviations.

Examples: 

- calc_sum instead of calculate_sum or find_sum
- next_char instead of generate_next_char or move_to_next_char or move_char
- t instead of test (from the first view t looks like a time variable and confuses a lot)
- g instead of test_group

It is also a good practice to start the name of boolean variables with "is". It will help programmers to read if statements.

Example: offline -> is_offline. The phrase if (is_offline) is more straightforward then if (offline).

### Readability

Tabulation can be seen different on different platforms based on system settings. However, these settings can also express a programmer choice of comfort. In other words, in general it is better to avoid tabulations because they can change the look of code. In this code several tabs were removed and the indent was generalized.

spaces between logical sections

### Comments

We have already discussed that the initial code had an issue with function implementation length. Because of poor separation a few comments were used to express the meaning behind some actions. So these comments were moved to function names, where the commented functionality was put.

| Before                                                       | After                                                        |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| <img src="/2021-09-29_20-26-16.png" alt="2021-09-29_20-26-16" style="zoom:67%;" /> | <img src="/2021-09-29_20-36-21.png" alt="2021-09-29_20-36-21" style="zoom:50%;" /> |

### Repetitions

Some of the functions have repeated sections. It is better to create a general function to handle repeated operations to create a space for an easier scalability.

<img src="/2021-09-29_20-25-34.png" alt="2021-09-29_20-25-34" style="zoom:50%;" />

### Results

A huge work was dedicated to function separation and style reformatting. From the commits in this repository it can be seen how "main" function and "Group" member functions were made readable and eye-observable.

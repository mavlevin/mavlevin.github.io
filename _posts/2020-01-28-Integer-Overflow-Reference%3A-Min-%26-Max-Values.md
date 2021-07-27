---
layout: post
title: >
    Integer Overflow Reference: Min & Max Values
hide_title: false
tags: ['vulnerabilities', 'Integer Overflow', 'safe code', 'Uninitialized Memory Vulnerability']
excerpt_separator: <!--more-->
---

A reference for when working with integers, and looking for integer overflows and underflows.
<!--more-->

When an integer type, such as an int or unsigned short, overflows \(the variable is given a value *greater* than the maximum value it can hold\), the integer "wraps around" and becomes the minimum value the type can store.

Similarly, when an integer type underflows \(the variable is given a value *smaller* than the maximum value it can hold\), the integer "wraps around" and becomes the maximum value the type can store.

Use the chart below to find the minimum and maximum values each type can hold.
# Integer Size Chart

| Type               | Size In Bytes |              Minimum Value |               Maximum Value |
|--------------------|---------------|---------------------------:|----------------------------:|
| char               | 1 byte        |                       -128 |                        +127 |
| unsigned char      | 1 byte        |                          0 |                        +255 |
| short              | 2 bytes       |                    -32,768 |                     +32,767 |
| unsigned short     | 2 bytes       |                          0 |                     +65,535 |
| int                | 4 bytes       |             -2,147,483,648 |              +2,147,483,647 |
| long               | 4 bytes       |             -2,147,483,648 |              +2,147,483,647 |
| unsigned int       | 4 bytes       |                          0 |              +4,294,967,295 |
| unsigned long      | 4 bytes       |                          0 |              +4,294,967,295 |
| long long          | 8 bytes       | -9,223,372,036,854,775,808 |  +9,223,372,036,854,775,807 |
| unsigned long long | 8 bytes       |                          0 | +18,446,744,073,709,551,615 |

Notice that the minimum value for an unsigned type is always 0. Additionally, signed types have a greater negative range than positive range. This is because 0 is considered a positive number.

*Values in the table are for 64\-bit CPUs.*

*Types are signed unless marked otherwise. *

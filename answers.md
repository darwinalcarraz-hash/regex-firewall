# Lab - Regex Firewall Solutions

## Task 1 - Count Events
**Command:** `grep -cE '^[^#]' firewall.log`
**Result:** `100000`
**Explanation:** I used `^` to anchor at the start of the line, and `[^#]` is a negated character class that matches any character except `#`. This skips the 4 header lines that start with `#` and counts only the actual event lines. The `-c` flag tells grep to count matches instead of printing them, and `-E` enables extended regex.

## Task 2 - Blocked Traffic
**Command:** 
**Result:** 
**Explanation:** 

## Task 3 - Source Subnet
**Command:** 
**Result:** 
**Explanation:** 

## Task 4 - Large Packets
**Command:** 
**Result:** 
**Explanation:** 

## Task 5 - Reformat Lines
**Command:** 
**Result:** 
**Explanation:** 

## Task 6 - Real Metric
**Command:** 
**Result:** 
**Explanation:** 

## Task 7 - Time Window
**Command:** 
**Result:** 
**Explanation:** 

## Bonus
**Regex:** 
**Explanation:** 
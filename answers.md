# Lab - Regex Firewall Solutions

## Task 1 - Count Events
**Command:** `grep -cE '^[^#]' firewall.log`
**Result:** `100000`
**Explanation:** I used `^` to anchor at the start of the line, and `[^#]` is a negated character class that matches any character except `#`. This skips the 4 header lines that start with `#` and counts only the actual event lines. The `-c` flag tells grep to count matches instead of printing them, and `-E` enables extended regex.

## Task 2 - Blocked Traffic
**Command:** `grep -cE ' (DROP|REJECT) ' firewall.log`
**Result:** `60156`
**Explanation:** I used `(DROP|REJECT)` with the alternation operator `|` inside a group `()` to match either action. The spaces around it ` (DROP|REJECT) ` anchor the match to the action field, so it doesn't accidentally match those letters in other fields like IP addresses. The `-c` counts the lines and `-E` enables extended regex.

## Task 3 - Source Subnet
**Command:** `grep -cE ' 11\.' firewall.log`
**Result:** `33217`
**Explanation:** I used `11\.` where the backslash escapes the dot so it's treated as a literal dot, not as the regex "any character" metacharacter. The leading space ` 11\.` anchors it to the src-ip field (the 5th field), making sure I don't match something like `211.100` in another field. The `-c` counts and `-E` enables extended regex.

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
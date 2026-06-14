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
**Command:** `grep -cE ' [0-9]{7}$' firewall.log`
**Result:** `2343`
**Explanation:** I used `[0-9]{7}` with the quantifier `{7}` to match exactly 7 digits, which means the size is at least 1,000,000. The space before it anchors to the size field (the last field), and the `$` anchor ensures it matches the end of the line so I don't accidentally match 7 digits in the middle of another field. The `-c` counts and `-E` enables extended regex.

## Task 5 - Reformat Lines
**Command:** `sed -E 's/^([0-9-]+) ([0-9:]+) ([A-Z]+) ([A-Z]+) .*/\1 \3 \4/' firewall.log | grep -v '^#' | head -5`
**Result:** 
2018-05-25 FORWARD TCP
2018-02-22 FORWARD UDP
2018-03-20 REJECT UDP
2018-11-08 REJECT TCP
2018-07-24 REJECT TCP
**Explanation:** I used `sed -E` with capture groups to extract only the date, action, and protocol. `([0-9-]+)` captures the date, `([0-9:]+)` captures the time (ignored), `([A-Z]+)` captures the action, and `([A-Z]+)` captures the protocol. The `.*` ignores the rest. The backreferences `\1`, `\3`, `\4` rebuild the line as `date action protocol`. I piped through `grep -v '^#'` to skip headers and `head -5` to show only the first 5 lines.

## Task 6 - Real Metric
**Command:** `grep -cE ' ACCEPT TCP .* 80 [0-9]+$' firewall.log`
**Result:** `93`
**Explanation:** I combined three literal fields in one regex: ` ACCEPT` matches the action, ` TCP` matches the protocol, and ` 80` matches the destination port. The `.*` allows anything in between (IPs and src-port). The `[0-9]+$` anchor at the end ensures that `80` is the dst-port (second-to-last field) and not the src-port, because after `80` must come a space, the size digits, and the end of line. The `-c` counts and `-E` enables extended regex.

## Task 7 - Time Window
**Command:** `grep -cE '^[0-9-]+ 0[0-2]:[0-9]{2}:[0-9]{2}' firewall.log`
**Result:** `13138`
**Explanation:** I used a character-class range `0[0-2]` to match hours 00, 01, and 02. The `^[0-9-]+` anchors the date at the start of the line, and the space after it ensures the time comes right after the date. The `:[0-9]{2}:[0-9]{2}` matches the minutes and seconds with exactly 2 digits each. This covers the full window from 00:00:00 to 02:59:59. The `-c` counts and `-E` enables extended regex.

## Bonus
**Regex:** `^[^ ]+$`
**Result:** 
webhost001
webhost002
proxy_07
**Explanation:** I used a negated character class `[^ ]` with the `+` quantifier to match one or more characters that are NOT spaces. The `^` and `$` anchors ensure the entire line has no spaces anywhere. To test it, I created a temporary file `test_bonus.txt` with the four lines and ran `grep -E '^[^ ]+$' test_bonus.txt`. It matched the three valid host names but rejected `that server is broken` because it contains spaces. The `wc -l` confirmed exactly 3 matches.
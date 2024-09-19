# Restore-IP-Addresses

A valid IP address consists of exactly four integers separated by single dots. Each integer is between 0 and 255 (inclusive) and cannot have leading zeros.
For example, "0.1.2.201" and "192.168.1.1" are valid IP addresses, but "0.011.255.245", "192.168.1.312" and "192.168@1.1" are invalid IP addresses.
Given a string s containing only digits, return all possible valid IP addresses that can be formed by inserting dots into s. You are not allowed to reorder or remove any digits in s. You may return the valid IP addresses in any order.

Constraints:

1 <= s.length <= 20
s consists of digits only.

class Solution:
    def restoreIpAddresses(self, s: str) -> List[str]:
        def valid(segment: str) -> bool:
            segment_length = len(segment)  
            if (
                segment_length > 3
               ):
                return False
            return int(segment) <= 255 if segment[0] != "0" else len(segment) == 1

        def update_segment(
            s: str, curr_dot: int, segments: List[str], result: List[str]
        ):
            segment = s[curr_dot + 1 : len(s)]
            if valid(segment):  
                segments.append(segment)  
                result.append(".".join(segments))
                segments.pop()  

        def backtrack(
            s: str, prev_dot: int, dots: int, segments: List[str], result: List[str]
        ):
            size = len(s)
            for curr_dot in range(prev_dot + 1, min(size - 1, prev_dot + 4)):
                segment = s[prev_dot + 1 : curr_dot + 1]
                if valid(segment):
                    segments.append(segment)

                    if dots - 1 == 0:
                        update_segment(s, curr_dot, segments, result)
                    else:
                        backtrack(s, curr_dot, dots - 1, segments, result)

                    segments.pop()  

        result, segments = [], []
        backtrack(s, -1, 3, segments, result)
        return result

## Suffix Arrays
- A suffix is a substring at the end of a string of characters. For our purposes suffixes are non empty.
- Suffix array is an array which contains all the sorted suffixes of a string.
- example: SA of "camel" is
  - 0:camel
  - 1:amel
  - 2:mel
  - 3:el
  - 4:l

  - 1: amel
  - 0: camel
  - 3: el
  - 4: l
  - 2: mel
- The actual suffix array is the array of sorted indices. In above example [1,0,3,4,2] is an suffix array for "camel"
- This provides a compressed representation of the sorted suffixes without actually needing to store suffixes
- The suffix array provides a space efficient alternative to a suffix tree which itself is a compressed version of a trie. (another data structure)
- Note: Suffix arrays can do everything suffix trees can, with some additional information such as a Longest Common Prefix (LCP) array.

- Longest Common Prefix Array (LCP array)
  - The LCP array is an array in which every index tracks how many characters two sorted adjacent suffixes have in common.
  - By convention, LCP[0] is undefined, but for most purposes its fine to set it to zero.
  - Note: There exists many methods for efficiently constructing the LCP array in O(nlog(n)) and O(n).

- Usage in finding and counting unique substrings
  - The problem of finding/counting all the unique substrings of a string is a common place problem in computer science.
  - The naive algorithm generates all substrings and places them in a set resulting in a O(n²) algorithm.
  - A better approach is to use the LCP array. This provides not only a quick but also a space efficient solution.
  - The number of unique substrings in a string is given by:
    n(n=1)/2 - sum of all the LCP values (duplicates)
  - which is number of strings minus duplicates

- Longest Common Substring (LCS) or K-common substring problem
  - suppose we have n strings, how do we find the longest common substring that appears in at least 2 <= k <= n of the strings.
  - One approach is to use dynamic programming running in O(n₁ * n₂ * n₃ * ... * nₘ), where nᵢ is the length of the string sᵢ. This may be ok for a few small strings, but rapidly gets unwieldy.
  - An alternative method is to use a suffix array which can find the solution in O(n₁ + n₂ + n₃ + ... + nₘ) time. This is a far superior approach.
    - To find the LCS first create a new larger string T which is the concatenation of all the strings Sᵢ separated by UNIQUE SENTINELS. eg: #, $, % signs
    - Note: The sentinels must be unique and lexicographically less than any of the characters contained in any of the strings Sᵢ
  - LCS algorithm:
    - For each valid combination (window) we perform a range query on the LCP array between the bottom and top endpoints. The LCS will be the maximum LCP value for all possible windows.
    - Luckily, the minimum sliding range query problem can be solved in a told of O(n) time for all windows.
    - alternatively, we can use min range query DS (could be hash table) such as a segment tree to perform queries in log(n) time which may be easier but slightly slower running for a total of O(nlog(n))

 Longest Repeated Substring (LRS)
 - The brute force method requires O(n²) time and lots of space. Using the information inside the LCP array saves you time and space.
 - string appears at least twice, hence the longest repeated substring.
 - Solution is to find all maximum LCP values to get the LRS, there can be more than one substrings.
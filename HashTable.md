## Hash Tables
- Hash table (HT) is a data structure that provides a mapping from keys to values using a technique called hashing.
- Entries stored in a key-value pairs.
- Key can be any value as long as it is unique.
- Values in key-value pairs can be repeated.
- Often used to track item frequencies.
- The key-value pairs you can place in a HT can be of any type not just strings or numbers but also objects. However the keys need to be HASHABLE.

- Hash function
  - h(x) is a function that maps a key 'x' to a whole number in a fixed range.
  - We an also define hash functions for arbitrary objects such as strings, lists, tuples, multi data objects, etc.
  - If H(x) = H(y) then objects x and y might be equal, but if H(x) != H(y) then x and y are certainly not equal.
  - How can we use above to our advantage to speedup object comparisons: This means that instead of comparing x and y directly a smarter approach is to first compare their hash values, and only if the hash values match do we need to explicitly compare x and y.
  - Note: Hash functions for files are more sophisticated than those used for hashtables. Instead for files we use what are called as cryptographic hash functions also called CHECKSUMS.
  - A hash function H(x) must be deterministic. This means that if H(x) = y then H(x) must always produce y and never another value. This may seen obvious, but it is critical to the functionality of a hash function.
  - We try hard to make them uniform hash functions to minimize the number of hash collisions.
  - A hash collision is when two objects x and y hash to the same value (i.e. H(x) = H(y)).
  - What makes a key of type T hashable?: Since we are going to use hash functions in the implementation of our hash table we need our hash functions to be deterministic. to enforce this behavior, we demand that the keys used in our hash table are IMMUTABLE data types. Hence, if a key of type T is immutable, and we have a hash function H(k) defined for all keys k of type T then we say a key of type T is hashable.

  - How does the hash table work?
    - Ideally we would like to have a very fast insertion, lookup and removal time for the data we are placing within our hash table.
    - Remarkably, we can achieve all this in O(1)* time using a hash function as a way to index into a hash table.
    * The constant time behavior attributed to hash tables is only true if you have a good uniform hash function.
    - Use one of many hash collision handling techniques to handle hash collisions.
      - Separate chaining:
        - deals with hash collisions by maintaining a data structure (usually a linked list) to hold all different values which hashed to a particular value.
      - Open addressing:
        - deals with hash collisions by finding another place within the hash table for the object to go by offsetting it from the position to which it hashed to.

- Complexity analysis
    Operation: Average worst
  - insertion: O(1)* O(n)
  - removal: O(1)* O(n)
  - search: O(1)* O(n)
* The constant time behavior attributed to hash tables is only true if you have a good uniform hash function.

- Hash table - Separate Chaining
  - It is one of many strategies to deal with hash collisions by maintaining a data structure (usually a linked list) to hold all the different values which hashed to a particular value.
  - Note: The data structure used to cache the items which hashed to a particular value is not limited to a linked list. Some implementations use one or a mixture of: arrays, binary trees, self balancing trees etc.
  - How do i maintain O(1) insertion and lookup time complexity once my HT gets really full and i have long list chains?: Once thr HT contains a lot of elements you should create a new HT with a larger capacity and rehash all the items inside the old HT and disperse them throughout the new HT at different locations.
  - How do i remove key-value pairs from HT?: Apply the same procedure as doing a lookup for a key, but this time instead of returning the value associated with the key remove the node in the linked list data structure.
  - Can i use another data structure to model the bucket bahaviour required for the separate chaining method?: yes, common data structures used instead of a linked list include: arrays, binary trees, self balancing trees etc. You can even go with a hybrid approach like Java's HashMap. However, note that some of these are much more memory intensive and complex to implement than a simple linked list which is why they may be less popular.

- Hash table - Open Addressing
  - When using open addressing as a collision resolution technique the key-value pairs are stored in the table (array) itself as opposed to a data structure like an separate chaining.
  - This means we need to care a great deal about the size of our hash table and how many elements are currently in the table.
  - Load factor = items in table / size of table.
  - The O(1) constant time behaviour attributed to hash tables assumes the load factor (alpha) is kept below a certain fixed value. This means once alpha > threshold we need to grow the table size (ideally exponentially, e.g. double).
  - When we want to insert a key-value pair (k, v) into the hash table we hash the key and obtain an original position for where this key-value pair belongs, i.e. H(k).
  - If the position our key hashed to is occupied we try another position in the hash table by offsetting the current position subject to a probing sequence P(x). We keep doing this until an unoccupied slot is found.
  - There are many amount of probing sequences
    - Linear probing: P(x) = ax + b, where a, b are constants
    - Quadratic probing: P(x) = ax*2 + bx + c, where a,b,c are constants
    - Double hashing: P(k, x) = x*H2(k), where H2(k) is a secondary hash function
    - Pseudo random number generator: P(k, x) = x*RNG(H(k), x), where RNG is a random number generator function seeded with H(k)
  - Issues with cycles
    - Most randomly selected probing sequences modulo N will produce a cycle shorter than the table size.
    - This becomes problematic when you are trying to insert a key-value pair and all the buckets on the cycle are occupied because you will get stuck in an infinite loop.
    - How do we handle probing functions which produce cycles shorter than the table size? (incase they go in an infinite loop): IN general this consensus is that we don't handle this issue, instead we avoid it altogether by restricting our domain of probing functions to those which produce a cycle of exactly length N*
    *: There are a few exceptions with special properties that can produce shorter cycles.

- Hash table - Linear Probing
  - LP is a probing method which probes according to a linear formula, specifically: P(x) = ax + b where a(!=0), b are constants (note: the constant b is obsolete.)
  - However, as we previously saw not all linear functions are viable because they are unable to produce a cycle of order N. We will need some way to handle this.
  - Which values of the constant a in P(x) = ax produce a full cycle modulo N?: This happens when a and N are relatively prime. Two numbers are relatively prime if their Greatest Common Denominator (GCD) is equal to one. Hence, when GCD(a, N)  = 1 the probing function P(x) be able to generate a complete cycle and we will always be able to find an empty bucket.

- Hash table - Quadratic Probing
  - QP is a probing method which probes according to a quadratic formula, specifically: P(x) = ax*2 + bx + c, where a,b,c are constants and a != 0, (otherwise we have linear probing) (Note: the constant c is obsolete)
  - However, as we previously saw not all quadratic functions are viable because they are unable to produce a cycle of order N. We will need some way to handle this.
  - There are numerous ways to pick a probing function, 3 of the most popular ways:
    case 1: Let P(x) = x², keep the table size a prime number > 3 and also keep alpha <= 1/2
    case 2: Let P(x) = (x² + x)/2 and keep the table size a power of two
    case 3: Let P(x) = (-1ˣ) * x² and keep the table size a prime N where N is congruent of 3 mod 4

- Hash table - Double hashing (DH)
  - DH is a probing method which probes according to a constant multiple of another hash function, specifically:
  P(k, x) = x * H₂(k), where H₂(k) is a second hash function
  - H₂(k) must has the same type of keys as H₁(k)
  - Note: Notice that doubling hashing reduces to linear probing (except that the constant is unknown until runtime)
  - Since DH reduces to linear probing at runtime we may end up with a linear probing function such as P(x) = 3x, H₁(k) = 4, and table size in nine (N = 9) in which case we end up with infinite loop/cycle.
  - To fix the issue of cycles pick the table size to be a prime number and also compute the value of delta
  delta = H₂(k) mod N
  - If delta  = 0 then we are guaranteed to be stuck in a cycle, so when this happens set delta = 1
  - Notice that 1 <= delta < N and GCD(delta, N) = 1 since N is prime. Hence, with these conditions we know that modulo N the sequence
  H₁(k), H₁(k)+1delta, H₁(k)+2delta, H₁(k)+3delta, H₁(k)+4delta, ... is certain to have order N
  - Constructing H₂(k)
    - suppose the key K has type T, whenever we want to use double hashing as a collision resolution method we need to fabricate a new function H₂(k) that knows how to hash keys of type T.
    - It would be really nice to have a systematic way to be able to effectively produce a new hash function every time we need one.
    - Luckily for us the keys we used to hash are always composed of the same fundamental building blocks. In particular: integers, strings, real numbers, fixed length vectors etc.
  - There are many well known high quality hash functions for these fundamental data types. Hence, we can use and combine them to construct our function H₂(k).
  - Frequently the hash functions selected to compose H₂(k) are picked from a pool of hash functions called universal hash functions which generally operate on one fundamental data type.
  - Inserting with DH:
    - suppose we have an originally empty hash table and we want to insert some (kᵢ, vᵢ) pairs with DH and we selected our hash table to have:

- Remove elements from HT - By Open Addressing scheme
  - Suppose we have an empty hash table and we're using linear probing with P(x) = x as our probing function.
  - The solution is to place toa unique marker called TOMBSTONE instead of null to indicate that a (k, v) pair has been deleted and that the bucket should be skipped during search.
  - I have a lot of tombstones cluttering my HT how do i get rid of them?: Tombstones count as filled slots in the HT so they increase the load factor and will be removed when the table is resized. Additionally, when inserting a new (k, v) pair you can replace buckets with tombstones with the new key-value pair.
  - An optimization we can do is to replace the earliest tombstone encountered with the value we did a lookup for. The next time we lookup they key it'll be found much faster. We can this LAZY DELETION.
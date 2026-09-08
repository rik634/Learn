# Boyer-moore voting algorithm
- The Boyer–Moore Voting Algorithm is used to find the majority element in a sequence — the element that appears more than ⌊n/2⌋ times (i.e., more than half the array). It does this in O(n) time and O(1) space, which is what makes it special — no hash maps, no sorting.
- The core idea
   - Think of it as a game of cancellation. You walk through the array keeping track of a "candidate" and a "count":
     1. If count == 0, you pick the current element as the new candidate.
     2. If the current element equals the candidate, increment count.
     3. If it's different, decrement count.
- Intuition: pair up each occurrence of the majority element with a "different" element and cancel them out. Since the majority element occurs more than n/2 times, it has more instances than all other elements combined — so even after all this cancellation, it will always survive as the final candidate.

Code:
```python
def majority_element(nums):
    count = 0
    candidate = None
    for num in nums:
        if count == 0:
            candidate = num
        count += 1 if num == candidate else -1

    return candidate
```
- This gives you a candidate, not a guarantee. If the problem guarantees a majority element exists, you're done. If not, you need a second pass to verify:
```python
def majority_element_verified(nums):
    candidate = majority_element(nums)
    if nums.count(candidate) > len(nums) // 2:
        return candidate
    return None  # no majority element exists
```
- Why it works (informal proof)
  - Think of count as a "potential" that tracks (occurrences of candidate seen so far) minus (occurrences of everything else seen so far), within the current "streak" since the last reset to 0.
  - Key fact: whenever count hits 0, we throw away everything seen so far and start fresh. This is safe because in any prefix where count returns to 0, the number of majority-element occurrences equals the number of non-majority occurrences — so the true majority element (over the whole array) must still have a majority in the remaining suffix. So discarding that prefix never discards the eventual answer.
  - At the very end, since the true majority element has more than n/2 occurrences overall, it cannot be fully "cancelled out" by the rest of the array — so it must be the final candidate 
- Complexity
   - Time: O(n) — one pass (or two, if you verify)
   - Space: O(1) — just two variables

## Extension: Boyer–Moore for n/3 (elements appearing more than ⌊n/3⌋ times)
- There can be at most two such elements in an array (since three elements each appearing > n/3 times would exceed n). So you track two candidates and two counts:
```python
def majority_element_n3(nums):
    count1, count2 = 0, 0
    candidate1, candidate2 = None, None

    for num in nums:
        if candidate1 == num:
            count1 += 1
        elif candidate2 == num:
            count2 += 1
        elif count1 == 0:
            candidate1, count1 = num, 1
        elif count2 == 0:
            candidate2, count2 = num, 1
        else:
            count1 -= 1
            count2 -= 1

    # Verification pass required
    result = []
    for c in (candidate1, candidate2):
        if c is not None and nums.count(c) > len(nums) // 3:
            result.append(c)
    return result
```
### Interview tips: 
⦁	Plain language explanation: Every non majority vote can cancel at most one majority vote, but there simply are not enough non majority votes to cancel them all. 
- ⦁	Can you do this without extra space -> Boyer-moore voting, a running candidate and count, relying on the guaranteed majority. 
⦁	What if the majority is not guaranteed -> Boyer-moore voting algorithm still finds a candidate, but a second verification  pass is nore required.
- ⦁	Why does the cancellation trick actually work -> The majority element out-numbers every other value combined, so it can never be fully cancelled away.  

### follow up questions
1. Why does the "more than n divided by 2" guarantee specifically make Boyer-Moore voting correct
   - The condition "appears more than ⌊n/2⌋ times" guarantees that the majority element outnumbers all other elements combined. That single fact is what makes the cancellation strategy — pairing off one majority-element occurrence against one occurrence of anything else — guaranteed to leave at least one majority-element instance standing at the end, no matter how the cancellations happen to line up.
   - If the threshold were anything ≤ n/2 (say, "appears most frequently" or "appears at least n/3 times"), this guarantee breaks, and the algorithm can return a wrong answer.
   - Boyer-Moore's correctness hinges entirely on the fact that m > n - m, i.e., the majority element has a strict numerical edge over the combined weight of every other element — that's the only thing that guarantees it survives universal cancellation, regardless of arrangement.
   - This is also exactly why the n/3 variant needs two candidate slots: with the "more than n/3" threshold, up to two elements can each individually outweigh "the rest of the array minus that element's own count" when split across two competing cancellation slots — the arithmetic of how many slots you need is directly derived from this same outnumbering logic.
2. How would you adapt this to find every element appearing more than n divided by 3 times, the Majority Element II variant?
   - First, the key insight: how many such elements can exist?
     - If an element appears more than n/3 times, then at most two such elements can exist in the array. Why? If there were three elements each appearing more than n/3 times, their combined count would exceed 3 × (n/3) = n, which is impossible since the total array size is only n. So the answer set has size 0, 1, or 2 — never more.
     - This is exactly why the algorithm needs two candidate slots instead of one — the problem structure itself tells you how many "winners" to track for.
   - The adapted algorithm:
     - You maintain two candidates and two counters simultaneously, applying the same cancellation logic, but now a single "cancellation" only fires when an element matches neither current candidate — because it takes away from both counts at once (paired against both slots).
     - Why the logic changes this way
       1. If the current number matches candidate1 or candidate2, just bump that slot's count — no cancellation needed.
       2. If it matches neither, and a slot is empty (count == 0), claim that slot as a new candidate.
       3. If it matches neither and both slots are full, this is the cancellation case — decrement both counters by 1. Conceptually, this single "outsider" element is being used to cancel one unit of "support" from both candidate1 and candidate2 simultaneously.
   - That last point is the crucial difference from the n/2 version: in the single-candidate version, one non-matching element cancels one unit from the one candidate. Here, one non-matching element cancels one unit from each of the two candidates — because you're maintaining two independent survival races at once, and a generic "outsider" opposes both.
   - Why the verification pass is mandatory here (even more so than in n/2):
     - In the n/2 case, if the problem guarantees a majority element exists, you can skip verification. In the n/3 case, verification is essentially always required in practice, because:
       1. There might be zero elements satisfying the condition (e.g., [1,2,3,4] — every element appears exactly once, none exceeds n/3 = 1.33... wait, 1 > 1.33 is false, so none qualify).
       2. The two candidates that survive the pairing process are just "the two most resilient values" — they aren't automatically guaranteed to individually cross the n/3 threshold. Classic example: [1,2,3] — the algorithm may produce candidates like 1 and 3, but neither actually appears more than 3/3 = 1 time.
   - So you always do a second O(n) pass (or use a counted pass with a hashmap, though that reintroduces O(n) space — the point of Boyer-Moore is to avoid that) to filter down to only the values that truly qualify.
   - Complexity
     - Time: O(n) — two passes (candidate-finding + verification), still linear
     - Space: O(1) — just four variables (two candidates, two counters)
   - The general pattern (n/k)
     - This generalizes: to find elements appearing more than n/k times, you need k−1 candidate slots, since at most k−1 such elements can exist (by the same pigeonhole argument: k elements each with > n/k support would exceed n total). The n/2 case is the k=2 special case (1 slot), and n/3 is the k=3 case (2 slots).
     
3. How would you find the majority element if the array were streamedand could not be stored in memory all at once.
   - This is actually the ideal use case for Boyer-Moore — it was practically built for this, since it never needs random access or storage of prior elements. Let's walk through what changes and what doesn't.
   - The first pass (candidate finding) — no changes needed
     - The core algorithm already processes elements one at a time, using only two variables (candidate, count), and never looks back. So as data arrives from a stream — socket, Kafka topic, file being read chunk-by-chunk, sensor feed, etc. — you just feed each element into the same update rule as it arrives:
       ```python
       def process_stream_element(num, state):
       if state['count'] == 0:
          state['candidate'] = num
       state['count'] += 1 if num == state['candidate'] else -1
       ```
     - You maintain state externally (e.g., in a small object or even just two variables), update it as each new item streams in, and discard the raw item immediately afterward. Memory usage stays O(1) regardless of stream length — this is exactly what makes it viable when the full array can't be held in memory.
   -  The real challenge: verification
      - The candidate from a single pass is not guaranteed correct — you normally verify with nums.count(candidate) > n/2, which requires a second pass. In streaming, this is where it gets interesting, because "can I do a second pass?" depends on the nature of the stream:
      - Case 1: Replayable stream (e.g., reading a file twice, replaying a Kafka topic from offset 0, querying a database twice)
Just do exactly what you'd do offline: pass 1 finds the candidate, pass 2 counts its actual occurrences and confirms > n/2. Still O(1) space, just two sequential O(n) reads instead of one.
      - Case 2: True unbounded/one-shot stream (live data, can't rewind — e.g., a live sensor feed or transaction stream with no persistence)
You can't verify with certainty in a strict single pass. Two practical approaches:
         1. Maintain a running verified count alongside the candidate. Every time the candidate changes, reset a counter; increment it whenever the current true candidate is seen. At any query point ("what's the majority so far?"), you can report the current candidate along with its live count vs. elements-seen-so-far, giving you an approximate/anytime answer rather than a final proof — useful for dashboards or monitoring where "best guess so far" is acceptable.
         2. Accept a small memory budget and track exact counts for a bounded set of frequent candidates — this moves you toward the Misra-Gries algorithm, which is the natural generalization of Boyer-Moore to streaming top-k / frequency estimation. Misra-Gries keeps k−1 candidate-counter pairs (like the n/3 extension) and guarantees that any element truly exceeding n/k support will show up as a candidate — with a bounded overestimate on counts. This is the standard real-world approach for "find frequent items in a stream" (used in networking for heavy-hitter detection, in analytics pipelines, etc.).
   
   - Boyer-Moore's first pass is inherently streaming-friendly since it's already O(1) space and single-pass by design — the real question is verification. If the stream can be replayed, do a second pass to confirm. If it's truly one-shot and unbounded, you either accept an "anytime" approximate answer, generalize to Misra-Gries for a bounded-error frequency sketch, or — if the stream is sharded across machines — exploit the fact that (candidate, count) states are algebraically mergeable, enabling a distributed map-reduce style solution.  

4. how would this approach change if we need index of majority element's first occurence , not just its value
   - Good question — this requires a small but careful addition to the algorithm, because the naive approach of "just track the index of the last candidate reset" turns out to be wrong, which is a common trap.
   - Why it's trickier than it looks
   - Your first instinct might be: track the index whenever candidate gets reassigned, and that's your answer. But this is wrong, because a candidate can be discarded and then re-emerge later as the candidate again — and if it does, its true first occurrence in the array was earlier than the point where it most recently got reassigned.
   - The correct fix: track first-occurrence index separately, per value
   - You can't rely on the algorithm's internal reset points to tell you first occurrence, because those resets are about cancellation bookkeeping, not about the element's true history in the array. So you need to decouple these two concerns:
   - Approach: single pass, using a small auxiliary structure
```python
def majority_element_first_index(nums):
    count = 0
    candidate = None
    first_seen = {}  # value -> first index it appeared at

    for i, num in enumerate(nums):
        if num not in first_seen:
            first_seen[num] = i
        
        if count == 0:
            candidate = num
        count += 1 if num == candidate else -1

    # Optional verification
    if nums.count(candidate) > len(nums) // 2:
        return candidate, first_seen[candidate]
    return None, -1
```
   - This still finds the correct candidate via standard Boyer-Moore, but separately maintains a first_seen dictionary that records, for every distinct value, the index of its first appearance — completely independent of the cancellation dynamics. At the end, you look up the final candidate's true first index.
   - Trade-off: this needs O(k) space where k = number of distinct elements, not O(1) anymore. This is a fundamental tension: Boyer-Moore's O(1) space guarantee is specifically about not tracking per-value history, but "first occurrence index" is per-value history — so some space cost is unavoidable if you want it in a single pass.
   - Naively assuming "first occurrence = last time the candidate was (re)assigned" is a classic trap, because a candidate can drop out and later re-emerge — its true first occurrence predates that. The clean fix is to decouple concerns: let Boyer-Moore find which value is the majority element (its core strength), and use either a small per-value index map (O(k) space, single pass) or a dedicated second pass (O(1) space, two passes) to actually locate its first index.

### Pattern connection:
1. contains duplicate and missing number, whoch hash based counting and membership techniques are the more general purpose ancestor of this frequency map step
2. majority element II, which extends the same voting idea to track 2 candidates at once for lower threshold.
3. single number, which uses different constant space trick, XOR to find value defined its own special count guarantee
4.  any straming or single pass problem where a string guarantee about dataa unlocks constant space trick beyond ordinary counting. 


Trick: adopt new candidate when count hits zero, add on match, subtract on mismatch. 

### Dynamic Programming
https://leetcode.com/problems/target-sum/discuss/455024/DP-IS-EASY!-5-Steps-to-Think-Through-DP-Questions <br />
https://leetcode.com/discuss/interview-question/1380561/template-for-dynamic-programming

##### 1. 0/1 Knapsack
Include/exclude the element at hand. <br />
**Item first and dp in reverse**

Item repetition isn't allowed:
```py
for item in item:
    # next reverse step is extremely important to remove repetitions.
    # visualize it, the first one would only update dp[first_one] since dp[0] = positive
    # the next one will build up on that, the next on top of that.
    for small_amount in range(len(dp), num-1, -1):
        dp[small_amount] = f(dp[small_amount-item])
```
* https://leetcode.com/problems/partition-equal-subset-sum/
_NOTE:_ Make sure we don't overflow; inner loop must be capped at `(num-1)`
* https://leetcode.com/problems/ones-and-zeroes/
Similar question, but with 2 constraints, ie 2D DP

#### 2. Unbounded Knapsack

```js
dp[i] = f(dp[i-a] + A . nums[i-a], dp[i-b] + B . nums[i-b], ..., dp[i-z] + C . nums[i-z])
```

**DP first: Loops are flipped wrt 1/0 knapsack**

If it just depends on last/last two elements, ie `a = b = 1` type of situation, we can get use variables instead of the whole array. Example: https://leetcode.com/problems/house-robber-ii/

Don't over complicate things: `len(dp) == (n+1)` because we want to reach `(n+1)`st stair. <br />
* https://leetcode.com/problems/climbing-stairs/
* https://leetcode.com/problems/min-cost-climbing-stairs/

* https://leetcode.com/problems/house-robber/
* https://leetcode.com/problems/house-robber-ii/ <br />
The trick here is to solve for `nums[1:]` and `nums[:-1]` then add

* https://leetcode.com/problems/delete-and-earn/ <br />
Extremely similar questions. Forget about next ones, if we take care of previous, everything automatically falls into place. `dp[i]` gives a solution for array that ends at i

* https://leetcode.com/problems/coin-change/ <br />
Similar questions <br />
Difference: dp array initialization: `dp[0] = nums[0] and dp[1] = max(nums[0:2])` <br />
*NOTE:* BFS is a valid solution for coin-change: generate solutions 

* https://leetcode.com/problems/decode-ways/ <br />
Great twist. Current character influences dp values of previous ones, not desirable. We traverse backwards. Rest of the things same as climbing-stairs (some checks obv)

* https://leetcode.com/problems/longest-arithmetic-subsequence-of-given-difference/ <br />
Good twist; dp has to be a dict instead of array.
```py
            dp[n] = dp.get(n-difference, 0) + 1
            max_seq = max(max_seq, dp[n])
```

* https://leetcode.com/problems/counting-bits/ <br />
Tricky to figure out. <br />
`Number of bits[i] = number of bits in the previous power of 2 ie. (1) + bits[i - previous power of 2]` <br />
`i - previous power of 2` is obviously smaller than the previous power of 2. <br />
Keep on generating powers of 2.


```js
dp[i] = f(dp[1], dp[2], ..., dp[i-1] or dp[i])
```
Here, rather than going to the previous computation, we traverse previous computations, check where we satisfy the constraint and then update `dp[i]`

* https://leetcode.com/problems/divisor-game/: Noted in the `Games` section <br />
Second loop to check whether a number less than i is a divisor

* https://leetcode.com/problems/perfect-squares/ <br />
Second loop to subtract j's square from i and update `dp[i]` based on `dp[i-j]` <br />
*NOTE:* BFS is also a valid solution

* https://leetcode.com/problems/maximum-length-of-pair-chain/ <br />
Similar idea. Sorting by the first element is the key

* https://leetcode.com/problems/partition-array-for-maximum-sum/ <br />
Good explanation: https://leetcode.com/problems/partition-array-for-maximum-sum/discuss/299049/DP-python-commented-code. The only difference being we traverse dp array from i backwards with j. `dp[i]` stores the solution for array ending with i as usual.

* https://leetcode.com/problems/best-team-with-no-conflicts/ <br />
The important thing to rememeber would be to sort based on age and scores as well. If scores is not kept a secondary parameter, we encounter a situation where, we append `(2, 4)` to the chain of say, `(2, 7)` which includes `(1, 5)`

* https://leetcode.com/problems/longest-arithmetic-subsequence/ <br />
LIS with a twist. DP needs to be a dict.

```py
        for i in range(len(dp)):
            for j in range(i):
                diff = nums[i] - nums[j]
                dp[i][diff] = max(dp[i].get(diff, 0), dp[j].get(diff, 0) + 1)
```

```js
dp[i] = f(g(j, dp[j]) * g(i-j, dp[i-j])) j E (1 < j <= i)
```

* https://leetcode.com/problems/unique-binary-search-trees/ <br />
dp[i] = number of trees possible with i nodes. <br />
No of trees with root i becomes: `trees possible with i nodes for left * with (i-j) nodes for right` <br/>
Sum for every `dp[i]`

```py
dp[i] += dp[j-1] * dp[i-j]
```

* https://leetcode.com/problems/integer-break/ <br />
Great question. We can break down number i around j `(E 1 < j < i/2)`. <br />
Now, we can chose to either: keep j as it is, break down to the best possible product, ie. `dp[j]`, same thing for the other piece, `(i-j)` <br />

```py
dp[i] = Math.max(dp[i], (Math.max(j, dp[j])) * (Math.max(i - j, dp[i - j])))
```

#### Exhaustive solution generation
* https://leetcode.com/problems/knight-dialer/
* https://leetcode.com/problems/greatest-sum-divisible-by-three/
* https://leetcode.com/problems/out-of-boundary-paths/ <br />
This is a great example on how to generate exhaustive solutions in DP grid: https://leetcode.com/problems/out-of-boundary-paths/discuss/102993/Python-Straightforward-with-Explanation <br />
BFS can solve this but it's extremely slow compared to DP, we'd have to traverse every single path
* https://leetcode.com/problems/knight-probability-in-chessboard/


#### DFS + Memoization
* https://leetcode.com/problems/target-sum/
* https://leetcode.com/problems/2-keys-keyboard/


#### Divide into at most K groups
* https://leetcode.com/problems/largest-sum-of-averages/ <br />
Great example showcasing DP's power.
```java
// Let f[i][j]be the largest sum of averages for first i + 1 numbers(A[0], A[1], ... , A[i]) tojgroups. f[i][j]  consists of two parts: first j-1 groups' averages and the last group' s average. Considering the last group, its  last number must be A[i] and its first number can be from A[0] to A[i]. Suppose the last group starts from A[p+1] , we can easily get the average form A[p+1] to A[i]. The sum of first j-1 groups' average is f[p][j-1] which we  have got before. So now we can write the DP equation:
// f[i][j] = max {f[p][j-1] + (A[p+1] + A[p+2] + ... + A[i]) / (i - p)}, p = 0,1,...,i-1

        for (int j = 2; j <= K; j++) {
            for (int i = 0; i < l; i++) {
                double max = Double.MIN_VALUE;
                for (int p = 0; p < i; p++) {
                    double sum = f[p][j - 1] + (s[i + 1] - s[p + 1]) / (i - p);
                    max = Double.max(sum, max);
                }
                f[i][j] = max;
            }
        }
```
This is convertable into 1D DP with bottom-up approach if you understand the concepts well enough
```py
        P = [0]
        for num in nums: P.append(P[-1] + num)
        
        def average(x, y):
            return (P[x] - P[y]) / (x - y)
        
        dp = [average(0, i+1) for i in range(0, len(nums))]
        
        for K in range(k-1):
            for i in range(len(dp)-1, -1, -1):
                for j in range(i):
                    dp[i] = max(dp[i], dp[j] + average(i+1, j+1))
        
        return dp[-1]
```

#### Kadane's algorithm


#### Straight-forward; not *really* dp; plug-in possibilties
* https://leetcode.com/problems/minimum-cost-for-tickets/
* https://leetcode.com/problems/paint-house/ 
```py
        for i, house in enumerate(dp[1:], start=1):
            dp[i][0] = costs[i][0]  + min(dp[i-1][1], dp[i-1][2])
            dp[i][1] = costs[i][1]  + min(dp[i-1][0], dp[i-1][2])
            dp[i][2] = costs[i][2]  + min(dp[i-1][0], dp[i-1][1])
        
        return min(dp[-1])
```
Space optimization:

```py
        for i, house in enumerate(costs[1:], start=1):
            aux_dp = dp[:]
            
            aux_dp[0] = costs[i][0]  + min(dp[1], dp[2])
            aux_dp[1] = costs[i][1]  + min(dp[0], dp[2])
            aux_dp[2] = costs[i][2]  + min(dp[0], dp[1])
            
            dp = aux_dp
        
```

* https://leetcode.com/problems/minimum-swaps-to-make-sequences-increasing/
* https://leetcode.com/problems/domino-and-tromino-tiling/

#### HARD
----------------------------------------------------------------------------

* https://leetcode.com/problems/count-all-valid-pickup-and-delivery-options/
* https://leetcode.com/problems/stone-game-iii/
* https://leetcode.com/problems/form-largest-integer-with-digits-that-add-up-to-target/
* https://leetcode.com/problems/stone-game-iv/
* https://leetcode.com/problems/coin-change-2/
* https://leetcode.com/problems/wiggle-subsequence/
* https://leetcode.com/problems/filling-bookcase-shelves/
* https://leetcode.com/problems/student-attendance-record-ii/
* https://leetcode.com/problems/decode-ways-ii/
* https://leetcode.com/problems/triples-with-bitwise-and-equal-to-zero/
* https://leetcode.com/problems/maximum-profit-in-job-scheduling/
* https://leetcode.com/problems/minimum-number-of-taps-to-open-to-water-a-garden/
* https://leetcode.com/problems/restore-the-array/

* https://leetcode.com/problems/profitable-schemes/
* https://leetcode.com/problems/tallest-billboard/
* https://leetcode.com/problems/pizza-with-3n-slices/
* https://leetcode.com/problems/reducing-dishes/

---------------------------------------------------------------------------

2.Knapsack <br />

* https://leetcode.com/problems/shopping-offers/

#### 3.Multi Dimensional DP
* https://leetcode.com/problems/triangle/
* https://leetcode.com/problems/minimum-falling-path-sum/

* https://leetcode.com/problems/combination-sum-iv/ <br />
Unbounded knapsack.
```py
   for small_target in range(1, len(dp)):
            for num in nums:
                possible_target = small_target - num
                if possible_target < 0:
                    continue
                dp[small_target] += dp[small_target - num]
```

* https://leetcode.com/problems/knight-probability-in-chessboard/
* https://leetcode.com/problems/minimum-number-of-refueling-stops/

* https://leetcode.com/problems/champagne-tower/
* https://leetcode.com/problems/video-stitching/
* https://leetcode.com/problems/stone-game-ii/
* https://leetcode.com/problems/number-of-dice-rolls-with-target-sum/
* https://leetcode.com/problems/dice-roll-simulation/
* https://leetcode.com/problems/number-of-sets-of-k-non-overlapping-line-segments/
* https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/
* https://leetcode.com/problems/create-maximum-number/
* https://leetcode.com/problems/frog-jump/
* https://leetcode.com/problems/split-array-largest-sum/
* https://leetcode.com/problems/freedom-trail/
* https://leetcode.com/problems/number-of-music-playlists/
* https://leetcode.com/problems/count-vowels-permutation/
* https://leetcode.com/problems/minimum-falling-path-sum-ii/
* https://leetcode.com/problems/minimum-distance-to-type-a-word-using-two-fingers/
* https://leetcode.com/problems/minimum-difficulty-of-a-job-schedule/
* https://leetcode.com/problems/number-of-ways-to-paint-n-3-grid/
* https://leetcode.com/problems/build-array-where-you-can-find-the-maximum-exactly-k-comparisons/
* https://leetcode.com/problems/number-of-ways-of-cutting-a-pizza/
* https://leetcode.com/problems/paint-house-iii/
* https://leetcode.com/problems/count-all-possible-routes/

4.Interval DP <br />
https://leetcode.com/problems/guess-number-higher-or-lower-ii/
https://leetcode.com/problems/arithmetic-slices/
https://leetcode.com/problems/predict-the-winner/
https://leetcode.com/problems/palindromic-substrings/
https://leetcode.com/problems/stone-game/
https://leetcode.com/problems/minimum-score-triangulation-of-polygon/
https://leetcode.com/problems/last-stone-weight-ii/
https://leetcode.com/problems/minimum-cost-tree-from-leaf-values/
https://leetcode.com/problems/stone-game-vii/
https://leetcode.com/problems/burst-balloons/
https://leetcode.com/problems/remove-boxes/
https://leetcode.com/problems/strange-printer/
https://leetcode.com/problems/valid-permutations-for-di-sequence/
https://leetcode.com/problems/minimum-cost-to-merge-stones/
https://leetcode.com/problems/allocate-mailboxes/
https://leetcode.com/problems/minimum-cost-to-cut-a-stick/
https://leetcode.com/problems/stone-game-v/

5.bit DP
https://leetcode.com/problems/can-i-win/
https://leetcode.com/problems/partition-to-k-equal-sum-subsets/
https://leetcode.com/problems/stickers-to-spell-word/
https://leetcode.com/problems/shortest-path-visiting-all-nodes/
https://leetcode.com/problems/smallest-sufficient-team/
https://leetcode.com/problems/maximum-students-taking-exam/
https://leetcode.com/problems/number-of-ways-to-wear-different-hats-to-each-other/
https://leetcode.com/problems/minimum-cost-to-connect-two-groups-of-points/
https://leetcode.com/problems/maximum-number-of-achievable-transfer-requests/
https://leetcode.com/problems/distribute-repeating-integers/
https://leetcode.com/problems/maximize-grid-happiness/
https://leetcode.com/problems/find-minimum-time-to-finish-all-jobs/

6.Digit DP
https://leetcode.com/problems/non-negative-integers-without-consecutive-ones/
https://leetcode.com/problems/numbers-at-most-n-given-digit-set/
https://leetcode.com/problems/numbers-with-repeated-digits/

7.DP on Trees
https://leetcode.com/problems/unique-binary-search-trees-ii/
https://leetcode.com/problems/house-robber-iii/
https://leetcode.com/problems/maximum-product-of-splitted-binary-tree/
https://leetcode.com/problems/linked-list-in-binary-tree/
https://leetcode.com/problems/longest-zigzag-path-in-a-binary-tree/
https://leetcode.com/problems/binary-tree-cameras/
https://leetcode.com/problems/maximum-sum-bst-in-binary-tree/
https://leetcode.com/problems/number-of-ways-to-reorder-array-to-get-same-bst/

8.String DP <br />
https://leetcode.com/problems/is-subsequence/
https://leetcode.com/problems/palindrome-partitioning/
https://leetcode.com/problems/palindrome-partitioning-ii/
https://leetcode.com/problems/word-break/
https://leetcode.com/problems/unique-substrings-in-wraparound-string/
https://leetcode.com/problems/minimum-ascii-delete-sum-for-two-strings/
https://leetcode.com/problems/longest-string-chain/
https://leetcode.com/problems/longest-happy-string/
https://leetcode.com/problems/longest-valid-parentheses/
https://leetcode.com/problems/distinct-subsequences/
https://leetcode.com/problems/word-break-ii/
https://leetcode.com/problems/count-the-repetitions/
https://leetcode.com/problems/concatenated-words/
https://leetcode.com/problems/count-different-palindromic-subsequences/
https://leetcode.com/problems/distinct-subsequences-ii/
https://leetcode.com/problems/longest-chunked-palindrome-decomposition/
https://leetcode.com/problems/palindrome-partitioning-iii/
https://leetcode.com/problems/find-all-good-strings/
https://leetcode.com/problems/string-compression-ii/
https://leetcode.com/problems/number-of-ways-to-form-a-target-string-given-a-dictionary/

9.Probability DP
https://leetcode.com/problems/soup-servings/
https://leetcode.com/problems/new-21-game/
https://leetcode.com/problems/airplane-seat-assignment-probability/

10.Classic DPs
A.Kadane's Algorithm
https://leetcode.com/problems/maximum-subarray/
https://leetcode.com/problems/maximum-product-subarray/
https://leetcode.com/problems/bitwise-ors-of-subarrays/
https://leetcode.com/problems/longest-turbulent-subarray/
https://leetcode.com/problems/maximum-subarray-sum-with-one-deletion/
https://leetcode.com/problems/k-concatenation-maximum-sum/
https://leetcode.com/problems/largest-divisible-subset/
https://leetcode.com/problems/length-of-longest-fibonacci-subsequence/
https://leetcode.com/problems/best-time-to-buy-and-sell-stock/
https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/
https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/
https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/

B.LCS
https://leetcode.com/problems/longest-palindromic-substring/
https://leetcode.com/problems/longest-palindromic-subsequence/
https://leetcode.com/problems/maximum-length-of-repeated-subarray/
https://leetcode.com/problems/longest-common-subsequence/
https://leetcode.com/problems/regular-expression-matching/
https://leetcode.com/problems/wildcard-matching/
https://leetcode.com/problems/edit-distance/
https://leetcode.com/problems/interleaving-string/
https://leetcode.com/problems/shortest-common-supersequence/
https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/
https://leetcode.com/problems/max-dot-product-of-two-subsequences/

C.LIS
https://leetcode.com/problems/longest-increasing-subsequence/
https://leetcode.com/problems/number-of-longest-increasing-subsequence/
https://leetcode.com/problems/russian-doll-envelopes/
https://leetcode.com/problems/delete-columns-to-make-sorted-iii/
https://leetcode.com/problems/minimum-number-of-removals-to-make-mountain-array/
https://leetcode.com/problems/maximum-height-by-stacking-cuboids/
https://leetcode.com/problems/make-array-strictly-increasing/

D.2D Grid Traversal
https://leetcode.com/problems/unique-paths/
https://leetcode.com/problems/unique-paths-ii/
https://leetcode.com/problems/minimum-path-sum/
https://leetcode.com/problems/maximum-non-negative-product-in-a-matrix/
https://leetcode.com/problems/where-will-the-ball-fall/
https://leetcode.com/problems/dungeon-game/
https://leetcode.com/problems/cherry-pickup/
https://leetcode.com/problems/number-of-paths-with-max-score/
https://leetcode.com/problems/cherry-pickup-ii/
https://leetcode.com/problems/kth-smallest-instructions/

E.Cumulative Sum
https://leetcode.com/problems/range-sum-query-immutable/
https://leetcode.com/problems/maximal-square/
https://leetcode.com/problems/range-sum-query-2d-immutable/
https://leetcode.com/problems/largest-plus-sign/
https://leetcode.com/problems/push-dominoes/
https://leetcode.com/problems/largest-1-bordered-square/
https://leetcode.com/problems/count-square-submatrices-with-all-ones/
https://leetcode.com/problems/matrix-block-sum/
https://leetcode.com/problems/maximum-points-you-can-obtain-from-cards/
https://leetcode.com/problems/count-submatrices-with-all-ones/
https://leetcode.com/problems/ways-to-make-a-fair-array/
https://leetcode.com/problems/maximal-rectangle/
https://leetcode.com/problems/max-sum-of-rectangle-no-larger-than-k/
https://leetcode.com/problems/super-washing-machines/
https://leetcode.com/problems/maximum-sum-of-3-non-overlapping-subarrays/
https://leetcode.com/problems/number-of-submatrices-that-sum-to-target/
https://leetcode.com/problems/get-the-maximum-score/

F.Hashmap (SubArray)
https://leetcode.com/problems/continuous-subarray-sum/
https://leetcode.com/problems/find-two-non-overlapping-sub-arrays-each-with-target-sum/
https://leetcode.com/problems/maximum-number-of-non-overlapping-subarrays-with-sum-equals-target/

11.DP + Alpha (Tricks/DS)
https://leetcode.com/problems/arithmetic-slices-ii-subsequence/
https://leetcode.com/problems/odd-even-jump/
https://leetcode.com/problems/constrained-subsequence-sum/
https://leetcode.com/problems/delivering-boxes-from-storage-to-ports/

12.Insertion DP
https://leetcode.com/problems/k-inverse-pairs-array/

13.Graph DP
https://leetcode.com/problems/cheapest-flights-within-k-stops/
https://leetcode.com/problems/find-the-shortest-superstring/

14.Memoization
https://leetcode.com/problems/minimum-jumps-to-reach-home/
https://leetcode.com/problems/scramble-string/
https://leetcode.com/problems/tiling-a-rectangle-with-the-fewest-squares/
https://leetcode.com/problems/number-of-ways-to-stay-in-the-same-place-after-some-steps/
https://leetcode.com/problems/jump-game-v/
https://leetcode.com/problems/minimum-number-of-days-to-eat-n-oranges/

15.Binary Lifting
https://leetcode.com/problems/kth-ancestor-of-a-tree-node/

16.Math
https://leetcode.com/problems/ugly-number-ii/
https://leetcode.com/problems/count-sorted-vowel-strings/
https://leetcode.com/problems/race-car/
https://leetcode.com/problems/super-egg-drop/
https://leetcode.com/problems/least-operators-to-express-number/
https://leetcode.com/problems/largest-multiple-of-three/
https://leetcode.com/problems/minimum-one-bit-operations-to-make-integers-zero/
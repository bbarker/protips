# Starting TLA Plus

## NixOS

1. `nix-shell -p tlaplusToolbox -p texlive.combined.scheme-medium`
  (I didn't check to see if this was the minimal texlive system required, probably not though.)
  Also note, this doesn't seem to include TLAPS.


# Model Checking

1. Here's a [good discussion](https://groups.google.com/g/tlaplus/c/mTxo-dmpZu0) on what TLA+ does, versus what TLC (a model checker) does.

## Limitations
1. TLC can't handle specs with `foo' = x` where x is an integer, whereas TLAPS can.
  - Although it is more subtle, a good rule of thumb is to only use:
     `v' = ...` or `v' \in ...` where `...` is an expression not containing a primed variable.

# Invariants

## Solution search
  1. Use an invariant that expressed the negation (`foo /= bar`) of what you actually want (`foo = bar`) to find the steps needed to arrive at a solution.

# Values

1. All values are implicitly sets in TLA+. But the elements of sets aren't necessarily specified, so for example, TLC would report an error if trying to evaluate `42 \in "abc"`.
2. It seems TLA+ does not allow shadowing of named values.
3. Records and Functions are not the same in TLA+, though they are more similar to each other than in most programming languages; `:` is for records, `->` is for functions.

# Formulas

1. Subformulas that involve only the initial state (no primes) are called *enabling conditions*, and should always go at the beginning of the formula (e.g. the first conjunctions in the formula). This makes the action formula easier to understand, but importantly, also TLC may find the formula intractable if they aren't placed first.


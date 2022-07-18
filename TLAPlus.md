# Starting TLA Plus

## NixOS

1. `nix-shell -p tlaplusToolbox -p texlive.combined.scheme-medium`
  (I didn't check to see if this was the minimal texlive system required, probably not though.)
  Also note, this doesn't seem to include TLAPS.


# Limitations
1. TLC can't handle specs with `foo' = x` where x is an integer, whereas TLAPS can.

# Invariants

## Solution search
  1. Use an invariant that expressed the negation (`foo /= bar`) of what you actually want (`foo = bar`) to find the steps needed to arrive at a solution.

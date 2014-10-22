#[barcab, `|_`, %brcb](#brcb)

[Short description]

#Syntax

`|_`, `barcab`, `[%brcb p=tile q=(map term foot)]` is a synthetic hoon that
produces a `%gold` door with sample `p`, arms `q`. `|_` takes an associative
array of names (++term) and expressions (++foot), each pair of which is called
an arm. A dry, or %ash, arm is denoted with `++`. `|_` can take an arbitrary
number of arms, but the arm array must be terminated with a `--`

##Produces

[Twig or tile]

##Sample

[`p` is a _
`q` is a _]

##Tall form

|_  p
    ++  p.n.q
      q.n.q
    --

##Wide form

None

##Irregular form

None

##Examples

```
++  fe                                                  ::  modulo bloq
      |_  a=bloq
      ++  dif  |=([b=@ c=@] (sit (sub (add out (sit b)) (sit c))))
      ++  inv  |=(b=@ (sub (dec out) (sit b)))
      ++  net  |=  b=@  ^-  @
               =>  .(b (sit b))
               ?:  (lte a 3)
                 b
               =+  c=(dec a)
               %+  con
                 (lsh c 1 $(a c, b (cut c [0 1] b)))
               $(a c, b (cut c [1 1] b))
      ++  out  (bex (bex a))
      ++  rol  |=  [b=bloq c=@ d=@]  ^-  @
               =+  e=(sit d)
               =+  f=(bex (sub a b))
               =+  g=(mod c f)
               (sit (con (lsh b g e) (rsh b (sub f g) e)))
      ++  ror  |=  [b=bloq c=@ d=@]  ^-  @
               =+  e=(sit d)
               =+  f=(bex (sub a b))
               =+  g=(mod c f)
               (sit (con (rsh b g e) (lsh b (sub f g) e)))
      ++  sum  |=([b=@ c=@] (sit (add b c)))
      ++  sit  |=(b=@ (end a 1 b))
      --
```

In ++fe, `|_` creates a door whose arms contain gates used to calculate modular
arithmetic on bitstrings. The sample of `|_` is the tile `a=bloq` which is a
block size argument that gets passed to each of the arms.

```
++  ne
  |_  tig=@
  ++  d  (add tig '0')
  ++  x  ?:((gte tig 10) (add tig 87) d)
  ++  v  ?:((gte tig 10) (add tig 87) d)
  ++  w  ?:(=(tig 63) '~' ?:(=(tig 62) '-' ?:((gte tig 36) (add tig 29) x)))
  --
::
```

++ne is used to print a single digit in base 10, 16, 32, or 64

Doors are commonly invoked with `%~`, short form ~(arm door samp), which 
replaces the door's sample and pulls the specified arm.

    ~zod/try=> `@t`~(x ne 12)
    'c'
    ~zod/try=> `@ux`12
    0xc
    /~zod/try=> =mol
                  |_  a=@ud
                  ++  succ  +(a)
                  ++  prev  (dec a)
                  --
    /~zod/try=> ~(succ mol 1)
    2
    /~zod/try=> ~(succ mol ~(succ mol ~(prev mol 5)))
    6
    
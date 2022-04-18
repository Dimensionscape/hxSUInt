# hxSUInt
hxSUInt is a simple implementation of a sequentially comparative unsigned integer.

SUInt provides a common solution for sequence number arithmatic wrapped as an abstract unsigned integer Type.

An SUInt(Sequential Unsigned Integer) behaves like a typical 32-bit unsigned integer, but allows for reliable comparisons in 32-bit looping sequence space by using operators. 

A sequence, in this case, provides an increment capable unsigned integer that retains its reliability to compare a sequence even in the event that the sequence reaches the max UInt(4294967295). For further clarification, consider the following code:

```hx
//MAX_UINT represents the maximum value of an unsigned integer.
var s:SUInt = MAX_UINT;
s++;
```

If we print out the value of s, as we expect, `s equals 0`, however, where this begins to deviate from a typical unsigned integer, is if we make a comparison here such as:

```hx
//0 > 4294967295;
var higherSequence:Bool = s > MAX_UINT;
```

In a typical UInt, the value here would print out as `false`, but since we are doing the comparison in sequential space, the value of higherSequence is actually `true`. This is important because it allows us to deal with infinite sequences in a finite number space(32-bits). It allows you to keep a reliable sequence order, even when an integer spill-over occurs by providing a 32-bit revolving index pool. 

Here's a brief demystification at how this actually works under the hood:

![image](https://user-images.githubusercontent.com/26172437/163863420-1c571939-fec0-4a02-b729-e0f075fc2674.png)

Take a 8-bit uint sequence space where the max unsigned integer is 255(8 bits is a little easier to visualize in this case).

We know from the previous run through that `0 > 255 = true`, but what is missing from the explanation is how this assumption is made. How we reach this conclusion is actually quite simple. Lets assume `s equals 64`. Applying the same rules of our sequence space, we can reliably determine that `64 > 193 = true`. We do this by calculating the distance between s and s + MAX_UINT and determine that any numbers preceding the sequence `s`, within that range, is `less than s`.

![image](https://user-images.githubusercontent.com/26172437/163866795-7953a5b2-b702-4f93-80eb-1ea1c0f876a9.png)

*A particular common use case for this can be seen in the TCP protocol which relies on this type of sequence number arithmatic to ensure features like reliable packet delivery.*


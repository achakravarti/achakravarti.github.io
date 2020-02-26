---
layout: post
title: Applying functions in RefPerSys
subtitle: Notes based on commit 6ecae5d2
tags: [refpersys, applying-functions, notes]
comments: false
---

These notes are based on commit 
[6ecae5d2686c44145cf9a0e6b39a3c13a8a65996](https://gitlab.com/bstarynk/refpersys/-/tree/6ecae5d2686c44145cf9a0e6b39a3c13a8a65996)
of the [RefPerSys](http://refpersys.org) project, and have been kindly reviewed
and improved by 
[Basile Starynkevitch](http://starynkevitch.net/Basile/index_en.html), the
architect and project lead of RefPerSys.

In [inline_rps.hh](https://gitlab.com/bstarynk/refpersys/-/blob/6ecae5d2686c44145cf9a0e6b39a3c13a8a65996/inline_rps.hh),
lines 1451 to 1470, we have the following code:

```cpp
Rps_TwoValues
Rps_ClosureValue::apply2(Rps_CallFrame*callerframe, const Rps_Value arg0,
                         const Rps_Value arg1) const
{
  RPS_ASSERT(callerframe && callerframe->stored_type() == Rps_Type::CallFrame);
  if (is_empty() || !is_closure())
    return  Rps_TwoValues(nullptr);
  Rps_ObjectRef obconn = connob();
  if (!obconn)
    return  Rps_TwoValues(nullptr);
  rps_applyingfun_t*appfun = obconn->get_applyingfun(*this);
  if (!appfun)
    return nullptr;
  callerframe->set_closure(*this);
  Rps_TwoValues res= appfun(callerframe, arg0, arg1,
                            Rps_Value(nullptr), Rps_Value(nullptr),
                            nullptr);
  callerframe->clear_closure();
  return res;
} // end Rps_ClosureValue::apply2
```


* This function is applying the closure to 2 arguments; this is analogous to the
  Scheme `apply` function: `apply (some-function '(arg0 arg1))`
* We are asserting that the call frame is valid, and are enforcing a check even
  with assertions off.
* `obconn` is the connective object (defined in the `fn` field of closures in
  the persisten store.
* We get a pointer to the function of the closure through a call to
  `obconn->get_applyingfun(*this)` and save it in `appfun`.
* Call frames have slots for closures, and we set the call frame slot for the 
  closure through `callerframe->set_closure(*this)`.
* We apply the closure function and get two values as the result, which are
  saved in `res`; we anticipate that the two values are stored in registers.
* We clear the closure from the call frame through a call to
  `callerfame->clear_closure()` before returning the result; we expect this to
  be compiled to one machine instruction with the `-O2` optimisation flag
  enabled.


In lines 1588 to 1594, we have the following code snippet from
`Rps_ClosureValue::apply7()`:

```cpp
std::vector<Rps_Value> restvec(3);
  restvec[0] = arg4;
  restvec[1] = arg5;
  restvec[2] = arg6;
  Rps_Value res= appfun(callerframe, arg0, arg1,
                        arg2, arg3,
                        &restvec);
```


* Arguments 4 to 6 of the closure are passed to the applying function through an
  `std::vector` of `Rps_Value`s called `restvec`. `appfun` is limited to 6
  parameters to take advantage of the 
  [Linux ABI](https://github.com/hjl-tools/x86-psABI/wiki/x86-64-psABI-1.0.pdf)
  specification that the first six parameters are passed through registers and not
  the call stack
* However, `restvec` is expected to be passed on the call stack (and not on the
  heap) since pointers cannot be passed through registers.


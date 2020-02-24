---
layout: post
title: Applying functions in RefPerSys
subtitle: Notes based on commit 6ecae5d2686c44145cf9a0e6b39a3c13a8a65996
tags: [refpersys, applying-functions, notes]
comments: false
---

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


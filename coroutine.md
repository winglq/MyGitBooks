alloc\_pool is a struct Coroutine pool, the pool is organized by QSLIST\(expand\).

qemu\_coroutine\_create get a GThread  from alloc\_pool or create a new one if no one left in alloc\_pool.

qemu\_coroutine\_enter will create\/run a glib thread. The glib thead will wait on the coroutine\_cond and  coroutine\_lock. When the condition is singaled and runable=True, then the coroutine will start.

qemu\_coroutine\_switch the from\_ Coroutine will blocked by coroutine\_lock. The to\_ Coroutine will run by set runnable to true and broadcasted coroutine\_cond.

qemu\_coroutine\_delete will free the resources.

coroutine\_thread is the main callable thread.



=====================

static void coroutine\_wait\_runnable\_locked\(CoroutineGThread \*co\)

{

 while \(!co-&gt;runnable\) {

 g\_cond\_wait\(&coroutine\_cond, &coroutine\_lock\);

 }

}

static void coroutine\_wait\_runnable\(CoroutineGThread \*co\)

{

 g\_mutex\_lock\(&coroutine\_lock\);

 coroutine\_wait\_runnable\_locked\(co\);

 g\_mutex\_unlock\(&coroutine\_lock\);

}

Coroutine will wait until conditonal could be singaled and g\_cond\_wait could unclock coroutine\_lock. After the thread go into the g\_cond\_wait, the coroutine\_lock will be unlocked, but it will need to lock the  coroutine\_lock if it want to return. So if other coroutine owned the lock g\_cond\_wait will wait.\(Perhapes\)


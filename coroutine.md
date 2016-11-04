alloc\_pool is a struct Coroutine pool, the pool is organized by QSLIST\(expand\).

qemu\_coroutine\_create get a GThread  from alloc\_pool or create a new one if no one left in alloc\_pool.

qemu\_coroutine\_enter will create\/run a glib thread. The glib thead will wait on the qemu\_cond and qemu\_mutex. When the condition is singaled and runable=True, then the coroutine will start.


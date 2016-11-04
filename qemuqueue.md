`/*`

` * Singly-linked List access methods.`

` */`

`#define QSLIST_EMPTY(head) ((head)->slh_first == NULL)`

`#define QSLIST_FIRST(head) ((head)->slh_first)`

`#define QSLIST_NEXT(elm, field) ((elm)->field.sle_next)`

`/*`

` * Simple queue definitions.`

` */`

`#define QSIMPLEQ_HEAD(name, type) \`

`struct name { \`

` struct type *sqh_first; /* first element */ \`

` struct type **sqh_last; /* addr of last next element */ \`

`}`

`#define QSIMPLEQ_HEAD_INITIALIZER(head) \`

` { NULL, &(head).sqh_first }`

`#define QSIMPLEQ_ENTRY(type) \`

`struct { \`

` struct type *sqe_next; /* next element */ \`

`}`

===================

struct customed\_struct {

... other fields;

QSIMPLEQ\_ENTRY\(customed\_struct\) next;

};

customed\_struct \*obj;

QLSLIST\_NEXT\(obj, next\) will expand to obj-&gt;next-&gt;sle\_next;




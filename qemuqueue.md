`/*`

` * Singly-linked List definitions.`

` */`

`#define QSLIST_HEAD(name, type) \`

`struct name { \`

` struct type *slh_first; /* first element */ \`

`}`

`#define QSLIST_HEAD_INITIALIZER(head) \`

` { NULL }`

`#define QSLIST_ENTRY(type) \`

`struct { \`

` struct type *sle_next; /* next element */ \`

`}`



`/*`

` * Singly-linked List functions.`

` */`

`#define QSLIST_INIT(head) do { \`

` (head)->slh_first = NULL; \`

`} while (/*CONSTCOND*/0)`

`#define QSLIST_INSERT_AFTER(slistelm, elm, field) do { \`

` (elm)->field.sle_next = (slistelm)->field.sle_next; \`

` (slistelm)->field.sle_next = (elm); \`

`} while (/*CONSTCOND*/0)`

`#define QSLIST_INSERT_HEAD(head, elm, field) do { \`

` (elm)->field.sle_next = (head)->slh_first; \`

` (head)->slh_first = (elm); \`

`} while (/*CONSTCOND*/0)`

`#define QSLIST_INSERT_HEAD_ATOMIC(head, elm, field) do { \`

` typeof(elm) save_sle_next; \`

` do { \`

` save_sle_next = (elm)->field.sle_next = (head)->slh_first; \`

` } while (atomic_cmpxchg(&(head)->slh_first, save_sle_next, (elm)) != \`

` save_sle_next); \`

`} while (/*CONSTCOND*/0)`

`#define QSLIST_MOVE_ATOMIC(dest, src) do { \`

` (dest)->slh_first = atomic_xchg(&(src)->slh_first, NULL); \`

`} while (/*CONSTCOND*/0)`

`#define QSLIST_REMOVE_HEAD(head, field) do { \`

` (head)->slh_first = (head)->slh_first->field.sle_next; \`

`} while (/*CONSTCOND*/0)`

`#define QSLIST_REMOVE_AFTER(slistelm, field) do { \`

` (slistelm)->field.sle_next = \`

` QSLIST_NEXT(QSLIST_NEXT((slistelm), field), field); \`

`} while (/*CONSTCOND*/0)`

`#define QSLIST_FOREACH(var, head, field) \`

` for((var) = (head)->slh_first; (var); (var) = (var)->field.sle_next)`

`#define QSLIST_FOREACH_SAFE(var, head, field, tvar) \`

` for ((var) = QSLIST_FIRST((head)); \`

` (var) && ((tvar) = QSLIST_NEXT((var), field), 1); \`

` (var) = (tvar))`

`/*`

`* Singly-linked List access methods.`

`*/`

`#define QSLIST_EMPTY(head) ((head)->slh_first == NULL)`

`#define QSLIST_FIRST(head) ((head)->slh_first)`

`#define QSLIST_NEXT(elm, field) ((elm)->field.sle_next)`

`/*`



===================

struct customed\_struct {

... other fields;

QSIMPLEQ\_ENTRY\(customed\_struct\) next;

};

customed\_struct \*obj;

QLSLIST\_NEXT\(obj, next\) will expand to obj-&gt;next-&gt;sle\_next;


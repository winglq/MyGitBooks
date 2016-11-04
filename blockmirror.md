mirror\_start\_job: first choose the right copy granularity\(align to block device cluster\). Initialize the BlockJob struct. BlockJob is async the callback is set by BlockCompletionFunc \*cb.  Use bs-&gt;open\_flags \|= BDRV\_O\_CACHE\_WB to enable target cache. create the mirror coroutine and enter to it. coroutine function is mirror\_run.

mirror\_run: Get block length.  A in\_flight\_bitmap will be created, and the bitmap length will equal to the UP\_ROUND\(block\_length\_by\_byte \/ granularity\). Create a cow\_bitmap whose length is equal to in\_flight\_bitmap. BlockJob buffer size must be bigger than cluster\_size. Create buf which is aligned by buf\_size. Set all alocated sector as dirty, the dirty\_map length is equal to number of sectors. After the dirty\_map is setup we could now move the source block to the buffer. First calculate the length of the left bytes. s-&gt;common.offset + \(cnt\(dirty sector numbers\) + s-&gt;sectors\_in\_flight\) \* BDRV\_SECTOR\_SIZE;  enter mirror\_iteration to copy data. If no buffer or will exceed max inflight bytes, do coroutine yield. If no in\_flight or no dirty, flush data to target.

mirror\_iteration: under stand hbitmap.




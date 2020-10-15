### 1、数据结构  
include/linux/mmzone.h（kernel-4.4.138）

```c
 626 /*
 627  * The pg_data_t structure is used in machines with CONFIG_DISCONTIGMEM
 628  * (mostly NUMA machines?) to denote a higher-level memory zone than the
 629  * zone denotes.
 630  *
 631  * On NUMA machines, each NUMA node would have a pg_data_t to describe
 632  * it's memory layout.
 633  *
 634  * Memory statistics and page replacement data structures are maintained on a
 635  * per-zone basis.
 636  */
 637 struct bootmem_data;
 638 typedef struct pglist_data {
         // 分别对应4种zone类型：ZONE_DMA、ZONE_DMA32、ZONE_NORMAL、ZONE_MOVABLE 
 639     struct zone node_zones[MAX_NR_ZONES];
     	 // 备用区域链表，从本节点分配不到内存可以从别的节点的zone中分配
 640     struct zonelist node_zonelists[MAX_ZONELISTS];
         // node_zones中有效zone的个数
 641     int nr_zones;
 642 #ifdef CONFIG_FLAT_NODE_MEM_MAP /* means !SPARSEMEM */
 643     struct page *node_mem_map;	// 该node管理的页面数组，用于非稀疏模型
 644 #ifdef CONFIG_PAGE_EXTENSION
 645     struct page_ext *node_page_ext;	// 页的扩展属性
 646 #endif
 647 #endif
 648 #ifndef CONFIG_NO_BOOTMEM
 649     struct bootmem_data *bdata;
 650 #endif
 651 #ifdef CONFIG_MEMORY_HOTPLUG
 652     /*
 653      * Must be held any time you expect node_start_pfn, node_present_pages
  654      * or node_spanned_pages stay constant.  Holding this will also
 655      * guarantee that any pfn_valid() stays that way.
 656      *
 657      * pgdat_resize_lock() and pgdat_resize_unlock() are provided to
 658      * manipulate node_size_lock without checking for CONFIG_MEMORY_HOTPLUG.
 659      *
 660      * Nests above zone->lock and zone->span_seqlock
 661      */
 662     spinlock_t node_size_lock;
 663 #endif
         //node的第一个页框号
 664     unsigned long node_start_pfn;
         //node内的页框数，不包含洞
 665     unsigned long node_present_pages; /* total number of physical pages */
         //node内的页框数，包含洞
 666     unsigned long node_spanned_pages; /* total size of physical page
 667                          range, including holes */
 		 //当前node的编号
 668     int node_id;
 669     wait_queue_head_t kswapd_wait;
 670     wait_queue_head_t pfmemalloc_wait;	// 在当前节点上分配内存时如果页面不足，进程被挂载到该节点的内存分配等待队列上，等该节点的kswapd进程交换内存满足时唤醒该节点上等待分配内存的进程
 671     struct task_struct *kswapd; /* Protected by
 672                        mem_hotplug_begin/end() */
 673     int kswapd_max_order;
 674     enum zone_type classzone_idx;
 675 #ifdef CONFIG_NUMA_BALANCING
 676     /* Lock serializing the migrate rate limiting window */
 677     spinlock_t numabalancing_migrate_lock;
 678 
 679     /* Rate limiting time interval */
 680     unsigned long numabalancing_migrate_next_window;
 681 
 682     /* Number of pages migrated during the rate limiting time interval */
 683     unsigned long numabalancing_migrate_nr_pages;
 684 #endif
 685 
 686 #ifdef CONFIG_DEFERRED_STRUCT_PAGE_INIT
 687     /*
 688      * If memory initialisation on large machines is deferred then this
 689      * is the first PFN that needs to be initialised.
 690      */
 691     unsigned long first_deferred_pfn;
 692     /* Number of non-deferred pages */
 693     unsigned long static_init_pgcnt;
 694 #endif /* CONFIG_DEFERRED_STRUCT_PAGE_INIT */
 695 } pg_data_t;
```


# Linux 中断子系统




## IRQ Domain

关于IRQ Domain的介绍: linux-6.1.23\Documentation\core-api\irq\irq-domain.rst

IRQ 关联一个irq Domain, 



## IRQ Chip


## 代码走读

GIC 初始化

```c
static int gic_init_bases(struct gic_chip_data *gic,
			  struct fwnode_handle *handle)
{
	int gic_irqs, ret;

    // 针对不支持BANKED寄存器的特殊处理
	if (IS_ENABLED(CONFIG_GIC_NON_BANKED) &amp;&amp; gic-&gt;percpu_offset) {
		/* Frankein-GIC without banked registers... */
		unsigned int cpu;

		gic-&gt;dist_base.percpu_base = alloc_percpu(void __iomem *);
		gic-&gt;cpu_base.percpu_base = alloc_percpu(void __iomem *);
		if (WARN_ON(!gic-&gt;dist_base.percpu_base ||
			    !gic-&gt;cpu_base.percpu_base)) {
			ret = -ENOMEM;
			goto error;
		}

		for_each_possible_cpu(cpu) {
			u32 mpidr = cpu_logical_map(cpu);
			u32 core_id = MPIDR_AFFINITY_LEVEL(mpidr, 0);
			unsigned long offset = gic-&gt;percpu_offset * core_id;
			*per_cpu_ptr(gic-&gt;dist_base.percpu_base, cpu) =
				gic-&gt;raw_dist_base &#43; offset;
			*per_cpu_ptr(gic-&gt;cpu_base.percpu_base, cpu) =
				gic-&gt;raw_cpu_base &#43; offset;
		}

		enable_frankengic();
	} else {
		/* Normal, sane GIC... */
		WARN(gic-&gt;percpu_offset,
		     &#34;GIC_NON_BANKED not enabled, ignoring %08x offset!&#34;,
		     gic-&gt;percpu_offset);
		gic-&gt;dist_base.common_base = gic-&gt;raw_dist_base;
		gic-&gt;cpu_base.common_base = gic-&gt;raw_cpu_base;
	}

	/*
	 * Find out how many interrupts are supported.
	 * The GIC only supports up to 1020 interrupt sources.
	 */
	gic_irqs = readl_relaxed(gic_data_dist_base(gic) &#43; GIC_DIST_CTR) &amp; 0x1f;
	gic_irqs = (gic_irqs &#43; 1) * 32;
	if (gic_irqs &gt; 1020)
		gic_irqs = 1020;
	gic-&gt;gic_irqs = gic_irqs;

	if (handle) {		/* DT/ACPI */
		gic-&gt;domain = irq_domain_create_linear(handle, gic_irqs,
						       &amp;gic_irq_domain_hierarchy_ops,
						       gic);
	} else {		/* Legacy support */

        // 已经删除了对第二级GIC的支持(仅限传统板级方式)
		/*
		 * For primary GICs, skip over SGIs.
		 * No secondary GIC support whatsoever.
		 */
		int irq_base;

		gic_irqs -= 16; /* calculate # of irqs to allocate */

		irq_base = irq_alloc_descs(16, 16, gic_irqs,
					   numa_node_id());
		if (irq_base &lt; 0) {
			WARN(1, &#34;Cannot allocate irq_descs @ IRQ16, assuming pre-allocated\n&#34;);
			irq_base = 16;
		}

		gic-&gt;domain = irq_domain_add_legacy(NULL, gic_irqs, irq_base,
						    16, &amp;gic_irq_domain_ops, gic);
	}

	if (WARN_ON(!gic-&gt;domain)) {
		ret = -ENODEV;
		goto error;
	}

	gic_dist_init(gic);
	ret = gic_cpu_init(gic);
	if (ret)
		goto error;

	ret = gic_pm_init(gic);
	if (ret)
		goto error;

	return 0;

error:
	if (IS_ENABLED(CONFIG_GIC_NON_BANKED) &amp;&amp; gic-&gt;percpu_offset) {
		free_percpu(gic-&gt;dist_base.percpu_base);
		free_percpu(gic-&gt;cpu_base.percpu_base);
	}

	return ret;
}
```


## 中断向量表的安装
```
__primary_swtich 
```


## 参考
1、[GICv3_v4_overview.pdf](images/GICv3_v4_overview.pdf)
2、[IHI0069H_gic_architecture_specification.pdf](images/IHI0069H_gic_architecture_specification.pdf)



---

> Author: Kristoffer  
> URL: https://psuvtk.github.io/posts/%E4%B8%AD%E6%96%AD%E5%AD%90%E7%B3%BB%E7%BB%9F/  


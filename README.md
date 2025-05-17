# Book-Doc
books and docments

我们在做内核热补丁，对于一个已经导出的函数(funcA)修改了其内部实现，同时也新增了一个函数(funcB)并导出，内核热补丁编译失败，打印如下：.export_symbol section contains strange symbol '(unknown)'

初步分析发现，对于是否调用sym_add_exported存在变更，在https://github.com/torvalds/linux/commit/ddb5cdbafaaad6b99d7007ae1740403124502d03提交之前，仅判断是否和__ksymtab_相等，但是现在，通过遍历.rela.export_symbol section获取导出符号，随后调用check_export_symbol失败(find_fromsym处理funcA时返回null)

During the kernel hot patch, the internal implementation of an exported function (funcA) is modified, and a new function (funcB) is added and exported. The kernel hot patch fails to be compiled, and the following information is displayed: .export_symbol section contains strange symbol '(unknown)'

According to the preliminary analysis, whether sym_add_exported is invoked is changed. Before https://github.com/torvalds/linux/commit/ddb5cdbafaaad6b99d7007ae1740403124502d03 is submitted, only the system checks whether sym_add_exported is equal to __ksymtab_. However, the system traverses the .rela.export_symbol section to obtain the export symbol. A subsequent call to check_export_symbol fails (find_fromsym returns null when handling funcA)

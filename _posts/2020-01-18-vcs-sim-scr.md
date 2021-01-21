---
layout: post
title: vcs仿真脚本
subtitle:
author: Liang Chen
date: 2020-01-18 18:00:00 +0800
tags: [Notes, EDA]
catalog: true
---

<head>
    <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
    <script type="text/x-mathjax-config">
        MathJax.Hub.Config({
            tex2jax: {
            skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
            inlineMath: [['$','$']]
            }
        });
    </script>
</head>

0. 在`.bashrc`中加入:

    ```bash
    #Verdi                                                                                                                                                                                     
    export VERDI_HOME=$/harddisk/disk1/eda/synopsys/verdi-2019/verdi/P-2019.06-SP1-1
    export PATH=$VERDI_HOME/bin:$PATH
    export LD_LIBRARY_PATH=$VERDI_HOME/share/PLI/vcs/linux64:$PATH
    export LD_LIBRARY_PATH=$VERDI_HOME/share/PLI/lib/linux64:$PATH
    ```

1. 仅编译vcs库文件（两步法）

    ```
    Makefile
    file_list.f
    dump_fsdb_vcs.tcl
    ```
    
    `Makefile`:
    
    ```Makefile
    export DESIGN_NAME= nnic
    export TB_NAME = ok_for_nnic_tb
    
    DESIGN_NAME = nnic
    TB_NAME = ok_for_nnic_tb
    
    VERDI_HOME = /harddisk/disk1/eda/synopsys/verdi-2019/verdi/P-2019.06-SP1-1
    PLATFORM = linux64
    
    VERDI = -P 	${VERDI_HOME}/share/PLI/VCS/${PLATFORM}/novas.tab \
    						${VERDI_HOME}/share/PLI/VCS/${PLATFORM}/pli.a 
    
    FILE_LIST = ./file_list.f
    
    
    INCDIR1 = +incdir+../example_design/sim
    INCDIR2 = +incdir+../example_design/rtl/rtl_axi_0907
    INCDIR3 = +incdir+../example_design/sim/oksim
    
    DEFINE = +define+x4Gb+sg125+x16 
    
    OPT = -full64 -sverilog +v2k +vcs+flush+all -timescale=1ns/1ps -Mupdate -debug_pp +notimingcheck +nospecify
    
    comp:
    	vcs -full64 \
    	-f ${FILE_LIST} \
    	${OPT} \
    	${INCDIR1} \
    	${INCDIR2} \
    	${INCDIR3} \
    	${DEFINE}
    	-o ${DESIGN_NAME}_simv \
    	${VERDI} \
    	-l comp.log
    
    run:
    	./${DESIGN_NAME}_simv \
      -ucli -i ./dump_fsdb_vcs.tcl \
      +fsdb+autoflush \
      -l run.log 
    
    all: comp run
    
    dbg:
    	verdi \
    	-sv \
    	-f ${FILE_LIST} \
    	${INCDIR1} \
    	${INCDIR2} \
    	${INCDIR3} \
    	${DEFINE} \
    	-top ${TB_NAME} \
    	-ssf ${DESIGN_NAME}.fsdb \
    	-nologo&
    
    clean:
    	@rm -rf \
    		*.log \
    		csrc \
    		*.conf \
    		*.rc simv \
    		*.daidir \
    		ucli.key \
    		*Log \
    		*.fsdb
    ```
    
    `file_list.f`:
    
    ```
    ../mult/src/booth_r4_64x64.v
    ../mult/src/counter_5to3.v
    ../mult/src/full_adder.v
    ../mult/src/half_adder.v
    ../mult/src/mult64x64_top.v
    ../mult/src/wtree_4to2_64x64.v
    ../mult/tb/mult64x64_check_s.v
    ../mult/tb/mult64x64_check_us.v
    ../mult/tb/testbench.v
    ```
    
    `dump_fsdb_vcs.tcl`:
    
    ```tcl
    global env
    fsdbDumpfile $env(DESIGN_NAME).fsdb 
    fsdbDumpvars 0 $env(TB_NAME)
    # Dumps the value changes of MDA(multidimensional array) signals to the FSDB file
    fsdbDumpMDA 0 $env(TB_NAME)
    run 2800000 ns
    quit
    ```

2. 编译混合库文件（三步法）

    ```
    Makefile
    dump_fsdb_vcs.tcl
    file_list.f
    synopsys_sim.setup
    ````
    
    `Makefile`:
    
    ```Makefile
    export DESIGN_NAME= nnic
    export TB_NAME = ok_for_nnic_tb
    
    VERDI_HOME = /harddisk/disk1/eda/synopsys/verdi-2019/verdi/P-2019.06-SP1-1
    PLATFORM = linux64
    
    VERDI = -P 	${VERDI_HOME}/share/PLI/VCS/${PLATFORM}/novas.tab \
    						${VERDI_HOME}/share/PLI/VCS/${PLATFORM}/pli.a 
    
    FILE_LIST = ./file_list.f
    WORK = xilinx_vcs_lib
    
    DESIGN_NAME = nnic
    TB_NAME = ok_for_nnic_tb
    
    INCDIR1 = +incdir+../example_design/sim
    INCDIR2 = +incdir+../example_design/rtl/rtl_axi_0907
    INCDIR3 = +incdir+../example_design/sim/oksim
    DEFINE = +define+x4Gb+sg125+x16 
    
    OPTS1 = -full64 -sverilog +v2k +vcs+flush+all -timescale=1ns/1ps
    OPTS2 = -full64 -Mupdate -debug_pp 
    
    
    comp:
    	#for multi-person work
    	rm -rf /harddisk/disk1/xilinx_vcs_lib/AN.DB/
    	mkdir -m 777 /harddisk/disk1/xilinx_vcs_lib/AN.DB/
    	#Step1
    	vlogan -sverilog \
    	-f ${FILE_LIST} \
    	-work ${WORK} \
    	${OPTS1} \
    	${INCDIR1} \
    	${INCDIR2} \
    	${INCDIR3} \
    	${DEFINE}
    	
    	#Step2
    	vcs -full64 \
    	${WORK}.${TB_NAME} ${WORK}.glbl \
    	${VERDI} \
    	${OPTS2} \
    	-o ${DESIGN_NAME}_simv \
    	-l comp.log
    	
    run:
    	#Step3
    	./${DESIGN_NAME}_simv \
    	-ucli -i ./dump_fsdb_vcs.tcl \
    	+fsdb+autoflush \
    	-l run.log
    
    dbg:
    	verdi \
    	-sv \
    	-f ${FILE_LIST} \
    	-work ${WORK} \
    	${INCDIR1} \
    	${INCDIR2} \
    	${INCDIR3} \
    	${DEFINE} \
    	-top ${TB_NAME} \
    	-ssf ${DESIGN_NAME}.fsdb \
    	-nologo&
    
    
    all: comp run
    
    
    clean:
    	@rm -rf \
    		*.log \
    		*Log \
    		csrc \
    		*.conf \
    		*.rc \
    		*simv \
    		*.daidir \
    		ucli.key \
    		*.fsdb* \
    		novas* \
    		.cxl.* 
    ```
    
    `file_list.f`: ...
    
    `dump_fsdb_vcs.tcl`: ...
    
    `synopsys_sim.setup`:
    
    ```
    OTHERS = /harddisk/disk1/xilinx_vcs_lib/synopsys_sim.setup
    xilinx_vcs_lib : /harddisk/disk1/xilinx_vcs_lib
    ```

-----------------------------------------------------------------

产生波形文件`*.vcd`: 所有的波形查看器都支持的文件类型

在`testbench.v`文件中加入:

```verilog
initial begin                                                                                                                                                                             
  $dumpfile("nnic.vcd");
  $dumpvars(0,SoC_U0);  // 0: all signals under module, 1: only signals within module, 2: two-level signals within module
end
```

# 9.DESIGN-AND-SIMULATION-OF-CONFIGURABLE-UVM-TEST-ENVIRONMENT-PG_UVM_LAB
## EXPERIMENT – 9 DESIGN AND SIMULATION OF CONFIGURABLE UVM TEST ENVIRONMENT
## AIM
To design and simulate a configurable UVM test environment using configuration database.
## THEORY
UVM configuration database (uvm_config_db) is used to:
•	Configure verification components 
•	Pass parameters dynamically 
•	Improve reusability 
## Advantages:
•	Flexible environment 
•	Easy parameter modification 
•	Reusable testbench structure 
## PROGRAM
Configurable UVM Environment (exp9.sv)
`include "uvm_macros.svh"
import uvm_pkg::*;
```
class my_driver extends uvm_component;

  `uvm_component_utils(my_driver)

  int data_width;

  function new(string name, uvm_component parent);
    super.new(name,parent);
  endfunction

  function void build_phase(uvm_phase phase);

    if(!uvm_config_db #(int)::get(this,"","data_width",data_width))
      data_width = 8;

  endfunction

  task run_phase(uvm_phase phase);

    `uvm_info("DRIVER",
    $sformatf("Configured Data Width = %0d",
    data_width),
    UVM_NONE)

  endtask

endclass


class my_test extends uvm_test;

  `uvm_component_utils(my_test)

  my_driver drv;

  function new(string name, uvm_component parent);
    super.new(name,parent);
  endfunction

  function void build_phase(uvm_phase phase);

    super.build_phase(phase);

    uvm_config_db #(int)::set(this,"*","data_width",16);

    drv = my_driver::type_id::create("drv",this);

  endfunction

endclass


module top;

  initial begin

    run_test("my_test");

  end

endmodule
## COMPILATION
vcs -sverilog -ntb_opts uvm exp9.sv -o simv
./simv
## RESULT
Thus, a configurable UVM test environment was designed and simulated successfully using Synopsys VCS.

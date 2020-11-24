# Integrating and simulating hardware accelerators in TVM

This RFC proposes support for integrating hardware designs in TVM,
specifically before they are manufactured in silicon or prototyped
in an FPGA. Regardless of the hardware target, ML accelerators
are built today, using the *waterfall* approach described in the
following figure:

![some](https://github.com/vegaluisjose/figures/blob/main/tvm/rfc_verilator/today.png)

Starting from an accelerator idea, hardware is built incrementally
until is manufactured after several weeks or in most cases months of engineering.
Once this happens, software teams start working on integrating the new
piece of hardware to a particular ML framework of choice i.e., TVM.
One challenge of this approach is the fact that hardware-design decisions
are evaluated too late, which turns out to be counterproductive. Additionally,
there are higher probabilities that the target ML model gets updated
along the way, which makes critical the efficiency of this process.

Motivated by this reality, we believe ML accelerators should be integrated,
and more importantly evaluated once the hardware design process begins.
Concretely, this approach allows engineers to have design feedback
and hardware-software continuous integration, since the first day as
shown in the following figure:

![some](https://github.com/vegaluisjose/figures/blob/main/tvm/rfc_verilator/idea.png)

We achieved this integration by leveraging the fact that most ML accelerators
are funnel down to a hardware language i.e., Verilog, regardless of the source
language. Moreover, we have today efficient Verilog-to-C++ open-source
compilers i.e., Verilator that can be used to interoperate via a simple
C interface (CFFI) with TVM.

This RFC proposes a simple codegen, and an *opaque* kernel library, runtime,
and device interface that can be implemented by engineers to efficiently integrate
ML accelerators with TVM early in the design process.
Also, we provide a small example (demo) that shows how to offload an `add` instruction
from a Relay program to a simple hardware implementation (scalar adder) written in Verilog.

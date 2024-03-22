# Lab 5: Sequential Circuit Design!
!

## Overview and Motivation
In this lab, we will design circuits that utilize memory in order to achieve increased functionality. Before this lab, we created different combinational circuits in which the outputs of the circuits were determined by the inputs directly. This means we weren't required to worry about memory, nor any notion of the sequence of operations that may have happened in the past.

This week, we will be working with Sequential Circuits. Sequential circuits introduce the idea of different states and being able to remember things about the previous input. The outputs can now be a function of both, inputs and previous internal state of the sequential circuit. In this lab, we will build a circuit that reads a binary number bit by bit and determines if that input (entire number) is divisible by 3. Each bit of the binary number will be clocked into the circuit sequentially, from the highest order bit to the lowest (left to right). As we recieve each bit, we must interpret it as this will equal to a different number than previous input/s and update our circuit's state accordingly.

To successfully be able to create this circuit, we need to make ourselves fimilar with Finite State Machine Design.

## Materials
For this lab, we will need to consult IC data sheets for the NOT, AND, XOR, and the JK Flip-flop:
    - IC data sheets

    - PB-503 breadboard prototyping station with the Logic Probes, NC Push Button and Logic Switches 

    - Wires and connection tools

    - Logic Probes (if not included in the breadboard)

    - Logic Switches (if not included in the breadboard)

    - Push Button (if not included in the breadboard)

    - 7404 NOT gate IC

    - 7408 AND gate IC

    - 7486 XOR gate IC

    - 7476 JK Flip Flop IC

    - Resistors

Once getting all things prepared, we can get to work!

## Project Steps
In order to design a sequential circuits, we have to go thorugh 5 steps:
* **Step 1:** Design a finite state machine (FSA)
* **Step 2:** Construct a state table
* **Step 3:** Use the FSM and state table to build a function table
* **Step 4:** Read values from the function table into K-maps
* **Step 5:** Use the K-maps to build for the next state and output blocks.
#### Design a finite state machine (FSA) 
For this task, we will draw a DFA for the task. We will draw a DFA that will accept the binary numbers that are divisible by 3. Notice that the 0 number will be accepted, we will have the first state as our accepting state. We know that number that is divisible by 3 is when the sum of the all digits in the number is divisible by 3. Writing out some examples of binary numbers that are/aren't divisible by 3, we can see that if the number has more than 1's or there are an even number of 0's between the 1's, the number is accepted.
We have the tranisition table as follow:

<!-- // Image of transiition table -->

From the transition table, we can easily construct the DFA. 

<!-- DFA image -->

#### Construct a state table
Now, let's set our state stable as follow:
<!-- Insert the simple state table -->

Now, based on the DFA we just construct, sketch out the truth table for our circuit. However, before drawing the truth table we should notice the the basic idea of how the JK flip-flop works according to our inputs and expected outputs. 

<!-- Insert the J-K table according to inputs -->

#### Use the FSM and state table to build a function table
Now, we are ready to draw our truth table.

<!-- Insert truth table -->

#### Read values from the function table into K-maps
Once we have the function table, we can go ahead to draw K-map. 

<!-- Insert K-map image -->

#### Boolean circuits
Now, thanks to the K-map, we can reduce our boolean expression to:

<!-- Insert image of reduced SOP -->

#### Construct circuits:
Before jumping right into buiding a real circuit, we should always test the logic of our circuits using Logisim. 

<!-- Insert logisim image -->

#### Construct real circuits:
In this section, we will get into constructing a real sequential circuit. The wiring steps for this circuit is quite complicated. As stated above, we will need a 7476 JK flip-flop, a 7404 NOT chip, and a 7486 XOR chip. 

##### Wire input and Clock:

Following the Logisim, we will have Clock and an input point. We will use the `NC PB` push button as our Clock. To wire the push button, 

- We wire a resistor with the `HIGH` hole on the breadboard. Then, wire the other end of the resistor to the `NC PB`. Use a wire and connect the hole on the `NC PB` with a row on the breadboard. 

For the input, we use Switch 1 `S1` on the breaboard. 

We will wire the input in the order of the JK flip-flop K's and J's. Since each 7476 JK flip-flop chip has 2 JK flip-flops, we will use, in this lab, the **JK gate 1** (at the top of the 7476 JK chip) for the **JK flip-flop 0** in our Logisim circuit. **JK gate 2** will be used as **JK gate 0** in our Logisim circuit. 

#### Wire the 7476 JK flip-flop chip:
First, we will need to wire the basics for the JK flip-flop:
- Wire the `GND` of the chip to the `GND` on the breadboard and `Vcc` to the `HIGH` hole. 

- Now, each FK flip-flop has to be connected to the Clock. Wire `1CLK` and `2CLK` to the row on the breadboard that is connected with the `NC PB` we just wired in the previous step. 

- We also need to wire `~CLR`. We use another Switch (we used `S8`) and connect the switch to `1~CLR` and `2~CLR` of the 2 JK flip-flops. 

##### Wire K_0:
Based on our reduced SOP, $K_0 = 1$. This is simple! We just need to take a wire and connect the `K_0` to a `HIGH` hole on the breadboard. 

##### Wire J0:
We have $J_0 = x \oplus Q_1$. Hence, take a 7486 XOR chip and follow the instructions:
    - Wire the input (from the Switch 1) to an `XOR` gate on the 7486 (`XOR`) chip. 
    - Use another wire to connect the output `Q_1` from the gate 1 on the 7478 JK chip to the other input pin on the `XOR` chip. 
    - Wire the output of the `XOR` chip to `J0`.

##### Wire K1:
From our SOP, `K1 = ~x`. Hence,
    - From the Switch 1 `S1`, wire the input to 1 input pin on the NOT chip. 
    - Take the output of the NOT gate and connect to the `K1` on the first JK flip-flop of the 7476 JK chip. 

#### Wire J1:
We have `J1 = Q0 (~x)`. To wire the circuit:
    - Take the 7408 AND chip. Wire the chip with `HIGH` AND `GND` holes on the breadboard. 
    - From the output of the **second JK flip flop** output, connect the output pin to the first input pin on the 7408 AND chip. 
    - On the row connected with the `~(x)` we just created at **Wire K1** step, plug a wire to connect the output `~x` to the second input pin of the 7408 AND chip. Now, we're done with wiring the `J1`!

##### Wire output F:
Lastly, we will need to wire our overall output `F`. `F = (~Q0)(~Q1)`. To wire `F`, we follow:
    - Each JK flip-flop gate has `Q` and `~Q` outputs. We will wire the `~Q0` (equivalent to `~Q2`) pin with one input pin of another AND gate on the 7408 AND chip. 
    - Similarly, wire the `~Q1` (equivalent to `~Q1` on the real JK flip-flop) to the second input pin of the AND gate. 
    - Connect the output to one Logic probe (we used `LED 8`) to check for the input. 

## Testing



## Conclusion



https://github.com/mlcourses/lab-5-blog-post-group6_cs281/assets/112486168/099952a6-7552-46d3-90e1-27faab2224c1





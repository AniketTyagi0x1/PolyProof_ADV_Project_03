# Chandigarh University Summer Internship With Metacrafters PolyProod ADV Project 03 (ZK-Circuit)

## Project Introduction 

This Project presents an overview of a custom zero-knowledge circuit implemented with the Circom language. The circuit, named Multiplier2, exemplifies the process of creating logic gates and connecting them to perform a specific computation. This circuit can generate zero-knowledge proofs, enabling the verification of computations while keeping the input values confidential.

![image](https://github.com/Metacrafters0x1/PolyProof_ADV_Project_03/assets/149813536/df765b56-4bde-4d04-b27d-a1af441711f8)

The circuit operates with two input signals, A and B, serving as operands for the computation. It utilizes three distinct logic gates—AND, NOT, and OR—each defined as individual templates. Two intermediate signals, X and Y, are generated as outputs from the AND and NOT gates, respectively. The final output signal, Q, is computed using the OR gate, with inputs derived from the intermediate signals.

## Project Objective 

To successfully complete the Final Challenge, your project should:

1. Write a correct circuit.circom implementation
2. Compile the circuit to generate circuit intermediaries
3. Generate a proof using inputs A=0 B=1
4. Deploy a solidity verifier to Sepolia or Mumbai Testnet
5. Call the verifyProof() method on the verifier contract and assert output is `true`

## Circom Language 

Circom is a domain-specific language designed for creating zero-knowledge circuits, which are used in cryptographic protocols to prove the validity of computations without revealing the input data. It provides a framework for defining and connecting logic gates and arithmetic operations to build complex cryptographic proofs, enabling secure and private verification of computations. By abstracting low-level details, Circom simplifies the development of zero-knowledge proof systems, making it accessible for developers to implement and verify privacy-preserving applications.

## ZKSnark Circuit Code Explanation 

```circom
pragma circom 2.0.0;

 template Multiplier2 () {  

   // User Inputs 
   signal input a;  // 0 
   signal input b; // 1

   // Circuit Internal Inputs

   signal x;  
   signal y;  

   // Output Circuit 
   signal output q; // 0

   //component

   component andGate = AND();
   component notGate = NOT();
   component orGate = OR();

   // Singals Operatio to AND Gate

   andGate.a <== a;
   andGate.b <== b;
   x <== andGate.out;

   // Signals Operation to Not Gate 

   notGate.in <== b;
   y <== notGate.out;

   // Getting Final Output From Logical Gates (q)

   orGate.a <== x;
   orGate.b <== y;
   q <== orGate.out;

}

template AND() {
    signal input a;
    signal input b;
    signal output out;

    out <== a*b;
}

template NOT() {
    signal input in;
    signal output out;

    out <== 1 + in - 2*in;
}

template OR() {
    signal input a;
    signal input b;
    signal output out;

    out <== a + b - a*b;

    log("ZkCircuit Execution Successfully Complete");
    log("Result - ", out);
}

component main = Multiplier2();
```

This code defines a zero-knowledge circuit using the Circom language, specifying version 2.0.0 with the pragma circom 2.0.0; directive. The primary template, Multiplier2, establishes the circuit's structure, including user inputs a and b, internal signals x and y, and an output signal q. The template instantiates three logic gates—AND, NOT, and OR—represented by the components andGate, notGate, and orGate, respectively.

The Multiplier2 template connects these logic gates through a series of operations. The AND gate takes inputs a and b, producing an output stored in x. The NOT gate processes input b, resulting in output y. Finally, the OR gate combines the results of the AND and NOT gates (signals x and y), yielding the final output q, which represents the circuit's overall computation result.

Additionally, the code defines the templates for the individual logic gates. The AND template multiplies its inputs a and b, outputting the result. The NOT template computes the logical NOT of its input using the expression 1 + in - 2*in. The OR template combines its inputs a and b using the expression a + b - a*b. Logging statements in the OR template provide feedback on the circuit's execution and output. The main component instance of Multiplier2 initiates the circuit, ready for zero-knowledge proof generation.



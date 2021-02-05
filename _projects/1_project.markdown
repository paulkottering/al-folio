---
layout: page
title: Molecule Simulation Project
description: Using a Quantum Recurrant Neural Network and the Variational Quantum Eigensolver.
img: /assets/img/ResultEthene.png
importance: 1
---

In the Spring 2020 semester I completed a group project focusing on simulating molecules using a quantum Recurrent Neural Network (RNN) to initialize a Variational Quantum Eigensolver, implemented in TensorFlow Quantum and OpenFirmion Cirq. We showed that substantial performance increases could be achieved through a chemically motivated RNN ansatz. Please find the final project [pdf](/assets/pdf/5_Kottering_paper.pdf){:target="\_blank"} for a more detailed explanation of our project.

Project completed with Pratik Brahma, Kian Jansepar, Joseph Palakapilly and Michael Qi.

Our project involved two tasks, the first was to research and investigate both quantum chemistry and quantum machine learning and the second was to implement a new method which combined recent advances in both fields and evaluate its performance. We chose the VQE method for solving for the ground state of a Hamiltonian of a molecule due to its short required coherence time. The VQE method is based on the Rayleigh-Ritz variational principle which states that the normalized expectation value of the Hamiltonian for an arbitrary wave function must be greater or equal to the ground state energy of the Hamiltonian. It is the algorithmic equivalent to the analytical method which finds the associated expectation value of a parametrized wavefunction and subsequently performs simple calculus to minimize that expectation with respect to the parameters of the wavefunction. The quantum computer is re-initialized at each step which means only short coherence times are needed.

The wavefunction that is initially applied is called the ansatz state and it can be prepared by applying a set of parametrized gates to our set of qubits. The choice of ansatz is critical to the performance of the algorithm and was a focus of our project. We proposed using QML to learn where to initialize parameters in a process known as meta-learning. This process extracts common structure among instances of a class of problem by training an outer-loop optimizer on many instances of this class which, by exploiting this common structure, improves our ability to solve unseen instances in the class or a related class. We use a recurrent neural network (RNN) optimizer to learn heuristics of our class of problem. The RNN receives as input the previous VQE query’s estimated cost function as well as the information stored internally from previous steps. This then generates a new suggestion for the VQE parameters and hopefully does so in very few steps. The loss function of the RNN when we are applying it to the VQE depends explicitly on Hamiltonian cost function, but the specific architecture is variable. We only use the RNN architecture to learn the problem-class-specific initialization heuristic and leave the fine optimization to a classical optimizer. After the RNN has learnt the VQE method problem-class during the training phase, the RNN is able to find appropriate initial parameters quickly during the test phase.

<div class="center">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/circuit.jpg' | relative_url }}" alt="" title="Ansatz Circuit"/>
    </div>
</div>
<div class="caption">
    Reference state preparation and ansatz circuit.
</div>

We decided to use Google’s Open Source library called Cirq as the circuit development library. For our project we decided to focus on two molecules that were adequately simple such that simulation times were not too long, hydrogen and ethylene. We used a chemically motivated ansatz and one of the most popular choices for this is the so-called unitary coupled cluster ansatz with single double excitation (UCCSD). The parametrized trial state created by the UCC method results from considering the excitations above the initial reference state. Our ansatz circuit only had one variable parameter ’theta’ which, due to the simplicity of the problem is appropriate, but when more complicated optimization problems are being solved we can provide our RNN with a vector of parameters which are simultaneously optimized.

To evaluate the cost function, which is the expectation value of the Hamiltonian, we must build a quantum circuit for our Hamiltonian. Using the Born-Oppenheimer approximation, and then using the Jordan-Wigner encoding we can obtain a Hamiltonian for hydrogen. We can obtain the Hamiltonian circuit using a computational chemistry package that is fully integrated with Cirq called OpenFermion. The RNN training and testing was implemented using another package that can be integrated with Cirq called TensorFlow Quantum (TFQ) released in March 2020. TFQ allows to connect classical and quantum layers of a RNN through seamless backpropagation and directly harnesses existing TensorFlow and Keras tools. TFQ constructs a dataflow graph for our choice of neural network, where the tensors are the directed edges and the operations are the nodes. The meta-learning RNN loss function uses a weighted history of the expectation values with a preferential weighting given to the expectation value of the last layers.The architecture of our QML-VQE implementation has been adapted from a paper produced by the creators of TensorFlow Quantum that performs RNN meta learning on a Quantum Approximate Optimization Algorithm. We also used an example from OpenFermion-Cirq to assist us with developing the ansatz and Hamiltonian which we adapted for our specific project.

We wanted to test the ability of the RNN to learn the heursitics of the VQE problem for hydrogen and ethylene. We trained the molecule on an arbitrary set of feasible training bounds and then tested the model on a larger range of bond lengths/torsion angles to show the model could extrapolate. Whilst our initial results provided confidence that we were not only able to reproduce results from literature, but also that we could use this RNN to find appropriate initial values, in order to answer our principle questions we would have to show that the RNN could assist in solving more complex molecules. To do this we trained the RNN model on the hydrogen molecule over a range of bond lengths and then used the same model to test on the ethylene model. The results from the training the RNN on the hydrogen Hamiltonian, and testing on the ethylene Hamiltonian showed that the RNN was able to guess initial parameters that resulted in a ground state energy close to the true ground state for all torsion angles tested. Note, that we are again not applying the VQE method at this point to the initial guess but instead comparing the energies from the RNN initial parameters and random initial parameters.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/ResultHydrogen.png' | relative_url }}" alt="" title="example image"/>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/ResultEthene.png' | relative_url }}" alt="" title="example image"/>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/BothErrorResults.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
    On the left, energies from RNN provided initial parameters vs randomly chosen initial parameters for Hydrogen. Middle, the same but for the larger ethylene molecule. Right, noisy ansatz circuit RNN parameter energies. Please see the full write-up for more results.
</div>

Furthermore, we averaged over the number of circuit evaluations that were performed for each torsion angle of the ethylene molecule. We can see that the number of circuit evaluations required when initializing with the RNN is less than when we use a random initial parameter. However, the total circuit evaluations, including the 5 required for the RNN test, is above the classic VQE for all precision requirements. In order to test the robustness of the RNN assisted VQE method against qubit error I encoded random error channels into the ansatz circuit. The two types of error I chose to investigate were bit-flip and phase-flip. The probability of the bit-flip error occurring at some point in the circuit was hard coded as 8 percent and the same for a phase-flip error. We notice that although the RNN guesses do not follow the true energy curve as closely as the noiseless results, the shape matches the true curve accurately.

<div class="center">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/SLSQPResult.png' | relative_url }}" alt="" title="Ansatz Circuit"/>
    </div>
</div>
<div class="caption">
    SLSQP Circuit Evaluation Comparison.
</div>

We can see from the results presented that we are able to use the RNN assisted VQE method to scale to more complex molecules. Considering the ability of the RNN to guess effective initial parameters, we are confident that applying the RNN method to a more complex ansatz, perhaps with more than one parameter, will mean that the total circuit evaluations will be lower than for the classic VQE method. The simulation times and lack of hardware accessibility make larger quantum computation difficult, but performance on complex molecules is crucial for determining whether the RNN assisted VQE is effective in the scaling of the VQE method. Our results showed that the neural network was used to rapidly find a global approximate optimum of the ansatz parameters. This then acted as an effective starting point for the classical optimizer, used in the classic VQE method, to search for the local minimum. We also showed that the RNN assisted VQE method had a certain ability to solve more complex molecules after being trained on a simpler molecule.

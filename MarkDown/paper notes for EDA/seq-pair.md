[toc]

# VLSI module placement 

## Rectangle Packing Problem:RP

The decision version of **RP(A)** is to decide whether $\cal M$ Can be packed onto a chip of area.

*we can call that 'module' 'cell' in our own research*

**RP(H,W)** can be polynomially reducible to an instance of **RP(A)** by the following conversion.
$$
r \gets \frac{the\space maximun\space width\space over \space modules}{2H} 
\\
A \gets (W+rH)(W+2rH) 
\\
{\cal M^{'}} \gets {(w \times rh| \forall (w \times h) \in {\cal M} )}\cup \{{rH \times rH},(W+rH)\times(W+rH)\}
$$


'combinational search' aims at finding a best code in the solution space.

The solution to NP-hard problem can be exponential.

Heuristics: SA(Simulated Annealing) & GA(Generatic Alogrithm) 



***P-admissible***

- slicing floorpan -> SA
- assigning one out of the four  relations
- This paper's approach  qualified



Orgnization:

- From a Packing to a sequence-pair
- From a Sequence-pair to a packing
- How the approach was utilized?
- Conclusions

## 1. From a Packing to a sequence-pair

##### **Gridding**

## 2.From a Sequence-pair to a packing

#### A. Constraint of a Sequence-Pair

**Theorem 2**: *The constraint is always satisfiable*

*Proof*:

Put firstly on (x,x), and then expand the separation of grid lines enough to eliminate overlapping of modules.($ \sqrt 2$ Times larger than the longest width/height over modules). As a consequence, the constraint implied by sequence-pair can be easily satisfied.



#### B. The best packing under the constraint

Can be obtained in O($m^2$) time by applying the well-known longest path algorithm(Dijiskra) for vertex weighed directed  acyclic graphs.



#### C. The P-admissible Solution Space

Minimizing the area of the chip.

Perimeter of the chip may be assumed.



#### D. Use of the solution space and experiments

##### 1. Rectangle Packing

packed by standard simulated annealing method

Using Three kinds of interchanges:

iii) was selected with higer probablity in lower temperatures.

##### 2. Module Placement with Wires

The $W^{'}$ and $H^{'}$ need to be calculated cause of the wires.

code for orientation and a sequence-pair are put together into a simulated annealing process in our system. The process runs in a similar fashion as the rectangle packing optimization, and explores the solution space of size $(m!)^{2}8^m$.

How the location of each individual module is calculated.

*coordinate* has the meaning of 'location'.

aspect ratio meas h:w = 1



## Conclusion

Seq-pair feasibly corresponds to a packing, and at least one seq-pair corresponds to an area-minimum packing.



Need to be concerned:

1.Under various constraints and different objectives.

- Wire congestion estimation
- timing
- power ==dissipation== 
- Clock distribution
- ==crosstalk==

2. The packing based approach is more concrete compare with the slicing structure based approach.

slicing structure approach:

- Safe routing order

- Channel definition

  
























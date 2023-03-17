dysfunctional: not operating normally or properly.
dysregulation: abnormality
perturbation: a deviation of a system

origin example:
>13gs_0-A
A 0 M 0


its coverage is limited, as the pool of experimentally characterized protein folds or structural motifs is small.
what are characterized protein folds or structural motifs?

what kind of prt is for aa "A", and study its atm-struct, covalent and valency??



### input_aa:
frame_indices_aa: (None, Lmax_aa, 3), triplet(center_Ca, prev_Ca, SCoM), triplet is index of coord
attributes_aa: (None, Lmax_aa, 20) aa type index
sequence_indices_aa: (None, Lmax_aa, 1)
point_clouds_aa: (None,  2 * Lmax_aa + 1, 3)

### input_atom:
frame_indices_atom: (None, Lmax_atom, 3), triplet, triplet is index of coord
attributes_atom: (None, Lmax_atom,) atom type index
sequence_indices_atom: (None, Lmax_atom, 1)
point_clouds_atom: (None, 11 * Lmax_aa, 3)


### embed aa:
embedding:Turns positive integers (indexes) into dense vectors of fixed size.
attributes_aa(masked with max aa number in a chain):
(None, 256, 20) --NN along dim[1]--> (None, 256, 32)


### embed atom:
#### build local frame:
compute aa frame data(local frame, [center, x, y, z]):
center=Ca
z-axis=vec(center->prev, Ca->prev_Ca)
y-axis=vec(z-axis) X vec(center->next, Ca->SCoM)
x-axis=vec(y-axis) X vec(z-axis)

#### build graph by finding neighborhood
LocalNeighborhood(OP) take [frame, attribute] as input
select K=16 neighbors:
atom to atom, use center distance
1) reshape attribute(a) by neighorhood
2) compute gaussian kernel
3) apply neighborhood embedding module(NEM): Relu(G(x)*a + G(x) + a) (None, 2304, 128) see eq.4 
4) compute:
  att:  (None, 2304, 128)->(None, 2304, 64)
  feat: (None, 2304, 128)->(None, 2304, 64)


### build aa-atom graph
1)Build graph aa->atom, for each amino acid we look for the 14 closest atoms in terms of sequence distance.
  get aa's neighbors' att and feat: (None,256,14,64), (None,256,14,64)
2) multi-head att:
  att(None,256,14,64), feat(None,256,14,64)->(None, 256, 64)
att: coeff * w * node

### embed aa:

###
take aa embedding feature and do att








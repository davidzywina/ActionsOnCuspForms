This is a very basic Magma implementation of the algorithm from the paper `Computing actions on cusp forms` for computing the Atkin-Lehner operator on 
spaces of cusp forms with integral weight k>1.   

The main functions are the following (see the function in the code for detailed descriptions):
- `ComputeAtkinLehner`  :  for a congruence subgroup Gamma between Gamma_1(N) and Gamma_0(N), computes a basis of S_k(Gamma,Z) given
                                 by their q-expansion and a matrix W that gives the action of the Atkin-Lehner operator W_N on S_k(Gamma) in terms of this basis.
- `SL2ActionOnCuspForms` : computes a basis of S_k(Gamma(N),Z) and gives the natural right action of SL_2(Z/NZ) on S_k(Gamma(N),Q(zeta_N))
                                 with respect to this basis.
- `FindCanonicalModel`   : for a subgroup G of GL(2,Z/NZ) with full determinant and containing -I, computes a set of generators of the homogeneous 
                                 ideal of the image of a canonical map X_G -> P^{g-1},  where X_G is the modular curve of genus g associated to G.
                                 This works well for small N but has not been optimized.
    

Remarks on numerical computations:
  The only thing not fully implemented is working out the error terms in the numerical approximations. We simply compute everything to a much higher accuracy
  than is actually needed.  When rounding real numbers that should be integers, we use `AlmostEqual` to ensure that they are indeed close; our default setting 
  is within distance 0.0001 of each other.   When we finally compute the matrix W describing the Atkin-Lehner operation, we also verify that W^2 is the 
  appropriate scalar matrix
  In each of the above functions, there is a parameter `prec` which says how many terms of the q-expansions to compute. If not enough terms have been computed
  to distinguish cusp forms, the function is halted or an error is produced.   One could use the Sturm bound, but this is often much larger than required.

  The q-expansions are also used to compute the pseudo-eigenvalues of newforms; the series involved converge quickly so not that many terms are actually needed 
  in practice.    In our function for computing pseudo-eigenvalues, there is also a parameter b>0 that one is allowed to vary (we work with a fixed b that has 
  worked for all examples so far).    

  Some examples to try:

        _,B,W,_:=ComputeAtkinLehner(7^2,2,[1+7],100 : real:=true);
        B; W/7;  

        G:=sub<GL(2,Integers(13))|[ [2,0,0,2], [1,0,0,5], [0,-1,1,0], [1,1,-1,1] ]>;
        FindCanonicalModel(G,200);

        G:=sub<GL(2,Integers(17))|[ [1,2*3,2,1],[1,0,0,-1]]>;
        FindCanonicalModel(G,200);

        G:=sub<GL(2,Integers(9))| [[2,0,0,1],[1,0,0,2]]>;
        FindCanonicalModel(G,200);

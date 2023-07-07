### ACE InP fits with FitSNAP

### First set of fits.

NOTE: These fits have poor test errors due to lack of random sampling. See next section for 
a k-fold cross validation study.

Each directory is a different fit.
To run the fit in FitSNAP, proceed into a directory and do

    mpirun -np P python -m fitsnap3 InP-example.in --overwrite

|Fit |Num. Coeffs  | Energy Test MAE (eV/atom)| Force Test MAE (eV/A)  | Notes|
--- | --- | ---| ---| ---|
1|35|0.099724|0.0791888|lmax = 1 1 1, nmax = 2 2 1 
2|49|0.0226652|0.0261319|lmax = 1 1 2, nmax = 8 1 1 
3|126|0.0115804|0.0175822|lmax = 1 2 3, nmax = 8 3 1
4|223|0.0120229|0.0177446|lmax = 1 2 4, nmax = 16 4 1
5|369|0.0097602|0.0181453|lmax = 1 3 5, nmax = 12 5 1
6|595|0.00772834|0.0183082|lmax = 1 4 6, nmax = 8 6 1
7|3028|0.00828747|0.015467|lmax = 1 4 6 2, nmax = 8 6 2 2

### Cross validation with random sampling.

The directories `fit1-*` show different cross validation fits, with 80/20 train/test split.

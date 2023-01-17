### ACE fits with FitSNAP

Each directory is a different fit.
To run the fit in FitSNAP, proceed into a directory and do

    mpirun -np P python -m fitsnap3 Ta.in --overwrite

Then to run MD with the potential, do

    lmp -in in.run

Errors and number of descriptors are listed here:

|Fit |Num. Descriptors  | Energy MAE (eV/atom)| Notes|
--- | --- | ---| ---|
1|73|0.00678|
2|52|0.0167|
3|64|0.0124|
4|35|0.0211|
5|43|0.0209|   same as 4, but increased number of radial basis, seems unimportant
6|44|0.00959|  nmax=3 on rank 2 descriptors, big improvement without big gain in descriptors
7|65|0.00801|  compare to fit3, decent improvement here





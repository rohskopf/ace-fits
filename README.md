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
8|77|0.00756|
9|125|0.00299|

The following fits use rank up to 3 for similar comparison with SNAP.
More experimenting with lmax and nmax:

|Fit |Num. Descriptors  | Energy MAE (eV/atom)| Notes|
--- | --- | ---| ---| ---|

10|109|0.00356| lmax = 1 2 2, nmax = 16 3 3
11|101|0.00406| Same as 10 but 8 RBFs (nmax = 8 3 3)
12|113|0.00229| lmax = 1 2 2, nmax = 8 4 3
13|176|0.00179| lmax = 1 2 3, nmax = 8 4 3
14|85|0.00437| lmax = 1 2 3, nmax = 8 4 2
15|95|0.00382 | lmax = 1 3 3, nmax = 8 4 2 # maybe keep rank 2 lmax = 2, compare with fit14

Now that we're getting energy errors ~2-3 meV/atom, let's try a more robust comparison with SNAP.
Here we will try `lmax = 1,2,3,4,5,6` for fair comparison with SNAP `twojmax = 2,4,6,8,10,12`.
The number of radial basis functions `nmax` is adjusted to have similar number of descriptors as SNAP,
but since we're only using 8 RBFs we might keep this constant.
**To start, let's try to get lmax=4 on rank 3 to have similar number of descriptors and error as SNAP**.
Goal: Shoot for ~55 descriptors and 6 meV/atom MAE.

|Fit |Num. Descriptors  | Energy MAE (eV/atom)| Notes|
--- | --- | ---| ---|
16|25|0.0234| lmax = 1 1 4, nmax = 8 1 1
17|33|0.0210| lmax = 1 1 4, nmax = 16 1 1 # increasing rank 1 nmax beyond 16 does not help
18|51|0.0060| lmax = 1 1 4, nmax = 16 4 1 # increasing rank 2 nmax to 4 has big improvement

Fit18 is looking good. Note that increasing rank 1 nmax beyond 16 shows negligible improvements,
so it would not be fair to do that. Let's try more changes.

|Fit |Num. Descriptors  | Energy MAE (eV/atom)| Notes|
--- | --- | ---| ---|
19|119|0.00225| lmax = 1 1 4, nmax = 16 4 2 # lies near POD and SNAP trend line

Changing rank 3 nmax from 1 to 2 gives a large increase in descriptors that follow the POD/SNAP 
trend line, but we can do achieve this by changing rank 3 lmax = 1,2,3,4,5,6.
So to further improve the rank 3 lmax=4 fits, let's try changing rank 3 nmax.

|Fit |Num. Descriptors  | Energy MAE (eV/atom)| Notes|
--- | --- | ---| ---|
20|61|0.00397| lmax = 1 1 4, nmax = 16 5 1 # lies on the POD and SNAP trend line, good!

Now that we have a rank 3 lmax=4 fit corresponding to same error and number of descriptors as SNAP
twojmax = 8, let's try changing lmax to compare with other data points.

Starting with lmax = 1 :

|Fit |Num. Descriptors  | Energy MAE (eV/atom)| Notes|
--- | --- | ---| ---|
21|17|0.0439| lmax = 1 1 1, nmax = 12 1 1 # let's try decreasing number of descriptors

Let's try decreasing number of descriptors. Here are fits from lmax = 1 to 6 :

|Fit |Num. Descriptors  | Energy MAE (eV/atom)| Force MAE (eV/A)  | Notes|
22|11|0.104|0.143|lmax = 1 1 1, nmax = 2 2 1 # in between POD and SNAP, fine
23|16|0.0527|0.168|lmax = 1 1 2, nmax = 8 1 1 # higher than POD and SNAP
24|35|0.00977|0.147|lmax = 1 2 3, nmax = 8 3 1 # agrees with POD and SNAP, fine
25|61|0.00376|0.102|lmax = 1 2 4, nmax = 16 4 1 # a little better than fit20 above
26|93|0.00259|0.0788|lmax = 1 3 5, nmax = 12 5 1
27|144|0.00115|0.0534|lmax = 1 4 6, nmax = 8 6 1




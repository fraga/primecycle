# What is this ?

A experimental project and library writed in Haskell to explore the prime cycles based on continued fractions of 1/n

I was trying to register the serie in OEIS , but seems nobody understand. I hope have more lucky with this repository. The library is just an experiment. Im glad to receive any feedback, is my main motivation to publish, find other crazy people who wants to share his experiments to solve this awesome problem . For now is just a demo, aren't valid for big numbers, im working on finish a stable version to publish as soon as posible, thanks. Questions are welcome .


![Image of Graph](./graph.png)


# Vol1. The Semiprime numbers and the cyclic behavior

5, 7, 9, 7, 9, 11, 15, 17, 9, 11, 13, 17, 19, 23, 25, 13, 15, 17, 21, 23, 27, 29, 33, 39, 41, 47,
15, 17, 19, 23, 25, 29, 31, 35, 41, 43, 49, 53, 55, 19, 21, 23, 27, 29, 33, 35, 39, 45, 47, 53,
57, 59, 63, 69, 75, 77, 21, 23, 25, 29, 31, 35, 37, 41, 47, 49, 55, 59, 61, 65, 71, 77 

The sequence represents the difference between, the prime products (q*p) and his
totient (q-1*p-1). The sequence allow us to identify primes with the same
fraction cycle , and is composed by semi prime numbers and prime numbers. This
can be interesting for cryptography purposes, or prime factorization
investigation, have some properties who allow to represent graphically the prime
numbers, and this common cycles among numbers.

To build the sequence you just need to get 2 prime numbers, and make the product ,
after that you make the same with the same prime numbers discounting 1 to each
one, the sequence contains the difference between them, each time you get one
difference , you can make the next, with the next combination, (3 * 3)-(2 * 2), 3 * 5 - (2 * 4 ) ...

The sequence can be divided in sub sequences with different longitudes, this
longitudes are prime numbers, and is a way to group numbers by this cycle. The
form to group this sub sequences is each time when the value of the sequence
decreases, for example

5, 7, 9

7, 9, 11, 15, 17

9, 11, 13, 17, 19, 23, 25

.....

Each time the sub secuences are bigger and his longitude is equal to a prime
number, this allow to make groups with numbers with similar properties in factor
terms.




FORMULA a(n) = (p1 * p2) - ((p1 -1) * (p2 - 1))


EXAMPLE With p=3 and q=3, (3 * 3) - (2 * 2) = 5. With p=3 q=5 (3 * 5) - (2*4) = 7. With p=5 q=5 (5 * 5) - (4 * 4) = 9.



# Diferences between n and totient , generate a serie with different primes longitudes , 3 , 5, 7, 11 ..


# Haskell Code

`````

primx x= map (primes !!) [1..(fromInteger x)]


sp x =  concat (map (\x-> ( map ( \y-> [x*y,(x-1)*(y-1), (x*y)-((x-1)*(y-1)) ] ) (primx x)) ) (primx x))


cycleserie x = map last (sp x)

splitcycle x s o

        | length rest == 0 = reverse series
        
        | otherwise = splitcycle rest (s+1) series

        where
        
                (out,rest) = splitAt (fromInteger (primes !! (fromInteger s))) ( x)
                
                 series =  [out] ++ o


*Main> splitcycle (cycleserie 10) 1 []

[[5,7,9],[7,9,11,15,17],[9,11,13,17,19,23,25],[13,15,17,21,23,27,29,33,39,41,47],[15,17,19,23,25,29,31,35,41,43,49,53,55],
[19,21,23,27,29,33,35,39,45,47,53,57,59,63,69,75,77],[21,23,25,29,31,35,37,41,47,49,55,59,61,65,71,77,79,85,89],
[25,27,29,33,35,39,41,45,51,53,59,63,65,69,75,81,83,89,93,95,101,105,111],
[31,33,35,39,41,45,47,51,57,59,65,69,71,75,81,87,89,95,99,101,107,111,117,125,129,131,135,137,141],
[33,35,37,41,43,47,49,53,59,61,67,71,73,77,83,89,91,97,101,103,109,113,119,127,131,133,137,139,143,157,161]]




``````



# Getting common factors numbers in the serie directly to N cycle group

`````

ncommons 2047 (ceiling (sqrt 2047))


[(23,253),(23,529),(23,575),(23,713),(23,1081),(23,1219),(23,1403),(23,1541),(89,445),(89,623),(89,979),(89,1513)]

``````


# Cycle Semi Prime Factorization

CF 1/n = semi prime cycle

CF 1/p = prime cycle

CF 1/semiprime cycle = prime cycle


semiprime cycle = part of number wich making the inverse 1/cycle the result is the semiprime number n

prime cycle = part of the number just containing the prime cycle

CF = Continued fraction

----


# Factorize or Crack everything

----

*Main> CF [1,7]

1.142857142857142857142857142857142857142857142857

*Main> CF [1,14]

1.071428571428571428571428571428571428571428571428

*Main> CF [1,28]

1.035714285714285714285714285714285714285714285714

*Main> CF [1,56]

1.017857142857142857142857142857142857142857142857

We use continuous fractions to have allways whole number information and never lose in base decimals

As you can see for the 7 number, is a infinite number who follows a pattern

In the 14 number is a semiprime, yo have some part of the number who have coded the information to know about the main factor and the result of the prime product ( semi prime)

## Decoding natural prime encoding

1/07 = 14 <- the semiprime, the rest of the pattern is 1/7, you have all information about the factors and the product.

By this way we can factorize any number:


# Semi Prime factors derivation

Generate parent semiprime with common factors with the input

showCF (1/164396375351843497554282732571)

"0.00000000000000000000000000000608285917411369656447522836367793197298118143322529168656111059740378154512271186168458684920611773688258313220329834622417693477397227328207607519014030815514145263691845013883634008782830237574753118799267355985617407094979979182165078764118812587715788807771695913204939493251793165991489084248561306593242196020037023264231273060829789866284732955945179005711499368396481429414608475319867399266160327802281535421644871266032396029885066265435384522543990239307358192529666017949169005096814388953894344122757793648255963103865460894078349257122151557604003257383482475445420355343996918489270102906434617762186024493673937186533126851191924003775883423595072864581799114827882749210260408075908821999695354570125741891542593822377347471142730451718142616851191765036913309109418282699405191403401233572336911920196928428803860423178053942191950807745827846839931112162844871041366593498614534696943382326001882687328320042185460646558429132890662322184909024450390247959131841757227199480333455827545100428744635455440338906380893550207957092843196050242351063382605912109623662366502545108311457516905259153214620247092940765701837206537367452960723572534897303795174388576501571850092472845640583352682369791620842348924573179628684798727001819417886131354153815663627391914887084500464132890233351611280603522321156287634372142773625927778555491628313969198329070141658733556743830631422826 ....

we get the semiprime cycle, the period always a infinite number

we make the inverse of the pattern to get a new derivated co related semprimes numbers , this numbers have more factors but all have the same factors of the input semiprime

showCF 
(1/0.00000000000000000000000000000608285917411369656447522836367793197298118143322529168656111059740378154512271186168458684920611773688258313220329834622417693477397227328207607519014030815514145263691845013883634008782830237574753118799267355985617407094979979182165078764118812587715788807771695913204939493251793165991489084248561306593242196020037023264231273060829789866284732955945179005711499368396481429414608475319867399266160327802281535421644871266032396029885066265435384522543990239307358192529666017949169005096814388953894344122757793648255963103865460894078349257122151557604003257383482475445420355343996918489270102906434617762186024493673937186533126851191924003775883423595072864581799114827882749210260408075908821999695354570125741891542593822377347471142730451718142616851191765036913309109418282699405191403401233572336911920196928428803860423178053942191950807745827846839931112162844871041366593498614534696943382326001882687328320042185460646558429132890662322184909024450390247959131841757227199480333455827545100428744635455440338906380893550207957092843196050242351063382605912109623662366502545108311457516905259153214620247092940765701837206537367452960723572534897303795174388576501571850092472845640583352682369791620842348924573179628684798727001819417886131354153815663627391914887084500464132890233351611280603522321156287634372142773625927778555491628313969198329070141658733556743830631422826)

"164396375351843497554282732571.00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000018483008938553632714683400804785600285806703373866444644134

You can see clear decoded of the natural data in the numbers the semiprime you are factorizing , the second number separated by power of 2 is a derivated semiprime who have 2 factors equal to the original. By this way we can factorize , knowing the factors of the derivated  more easy because have more.


you can generate infinite derivates each time bigger

We get the smallest 

*Main> ppfactors 18483008938553632714683400804785600285806703373866444644134

[(2,1),(6911587,1),(216857744866621,1),(758083947856951,1),(8133409916355420373571,1)]


*Main> gcd 164396375351843497554282732571 758083947856951

758083947856951

*Main> divMod 164396375351843497554282732571 758083947856951

(216857744866621,0)


# Extracting the factors like a boss

# All in one natural encoded

showCF (1/1006)

Semiprime part cycle

"0.00099403578528827037773359840954274353876739562624254473161033797216699801192842942345924453280318091451292246520874751491053677932405566600397614314115308151093439363817097415506958250497017892644135188866799204771371769383697813121272365805168986083499005964214711729622266401590457256461232604373757455268389662027833

Prime part cycle 

0019880715705765407554671968190854870775347912524850894632206759443339960238568588469184890656063618290258449304174950298210735586481113320079522862823061630218687872763419483101391650099403578528827037773359840954274353876739562624254473161033797216699801192842942345924453280318091451292246520874751491053677932405566600397614314115308151093439363817097415506958250497017892644135188866799204771371769383697813121272365805168986083499005964214711729622266401590457256461232604373757455268389662027833001988071570576540755467196819085487077534791

# The semiprime part

showCF 

(1/0.00099403578528827037773359840954274353876739562624254473161033797216699801192842942345924453280318091451292246520874751491053677932405566600397614314115308151093439363817097415506958250497017892644135188866799204771371769383697813121272365805168986083499005964214711729622266401590457256461232604373757455268389662027833)

"1006.00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000002012


# The prime part

*Main> showCF 

(1/00198807157057654075546719681908548707753479125248508946322067594433399602385685884691848906560636182902584493041749502982107355864811133200795228628230616302186878727634194831013916500994035785288270377733598409542743538767395626242544731610337972166998011928429423459244532803180914512922465208747514910536779324055666003976143141153081510934393638170974155069582504970178926441351888667992047713717693836978131212723658051689860834990059642147117296222664015904572564612326043737574552683896620278330019880715705765407554671968190854870775347912524850894632206759443339960238568588469184890656063618290258449304174950298210735586481113320079522862823061630218687872763419483101391650099403578528827037773359840954274353876739562624254473161033797216699801192842942345924453280318091451292246520874751491053677932405566600397614314115308)

"0.00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000503



# Extracting primes like a boss with different cycle patterns

showCF (1/377)
"0.002652519893899204244031830238726790450928381962864721485411140583554376657824933687002652519893899204244031830238726790450928381962864721485411140583554376657824933687



showCF (1/26525198938992042440318302387267904509283819628647214854111405835543766578249336872652519893899204244031830238726790450928381962864721485411140583554376657824933687)



0.000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000003769999999999
99999999999999999999999999999999999999999999999999999999999999999999962677000000000000000
00000000000000000000000000000000000000000000000000000000000000373267699999999999999999999
99999999999999999999999999999999999999999999999999999996267326770000000000000000000000000
00000000000000000000000000000000000000000000000037326732676999999999999999999999999999999
999999999999999999999999999999999999999996267326732677000000000000000000000000000000000000
000000000000000000000000000000000373267326732676999999999999999999999999999999999999999999
999999999999999999999999962673267326732677000000000000000000000000000000000000000000000000
0000000000000000037326732673267326769999999999999999999999999999999999999999999999999999999
9999999962673267326732673267700000000000000000000000000000000000000000000000000000000000003
7326732673267326732676999999999999999999999999999999999999999999999999999999999996267326732
6732673267326770000000000000000000000000000000000000000000000000000000003732673267326732673
2673267699999999999999999999999999999999999999999999999999999996267326732673267326732673267
7000000000000000000000000000000000000000000000000000003732673267326732673267326732676999999
9999999999999999999999999999999999999999999996267326732673267326732673267326770000000000000
0000000000000000000000000000000000003732673267326732673267326732673267699999999999999999999
9999999999999999999999999996267326732673267326732673267326732677000000000000000000000000000
0000000000000000003732673267326732673267326732673267326769999999999999999999999999999999999
9999999996267326732673267326732673267326732673267700000000000000000000000000000000000000000
3732673267326732673267326732673267326732676999999999999999999999999999999999999999626732673
2673267326732673267326732673267326770000000000000000000000000000000000000373267326732673267
3267326732673267326732673267699999999999999999999999999999999999626732673267326732673267326
7326732673267326732677000000000000000000000000000000000373267326732673267326732673267326732
6732673267326769999999999999999999999999999999626732673267326732673267326732673267326732673
2673267700000000000000000000000000000373267326732673267326732673267326732673267326732673267
6999999999999999999999999999626732673267326732673267326732673267326732673267326732677000000
0000000000000000000373267326732673267326732673267326732673267326732673267326769999999999999
9999999999626732673267326732673267326732673267326732673267326732673267700000000000000000000
0373267326732673267326732673267326732673267326732673267326732676999999999999999999962673267
3267326732673267326732673267326732673267326732673267326770000000000000000037326732673267326
7326732673267326732673267326732673267326732673267699999999999999962673267326732673267326732
6732673267326732673267326732673267326732677000000000000037326732673267326732673267326732673
2673267326732673267326732673267326769999999999962673267326732673267326732673267326732673267
3267326732673267326732673267700000000037326732673267326732673267326732673267326732673267326
7326732673267326732676999999962673267326732673267326732673267326732673267326732673267326732
6732673267326770000037326732673267326732673267326732673267326732673267326732673267326732673
2673267699962673267326732673267326732673267326732673267326732673267326732673267326732673267
7037326732673267326732673267326732673267326732673267326732673267326732673267326732673267326
7326732673267326732673267326732673267326732673267326732673267326732673267330502673267326732
6732673267326732673267326732673267326732673267326732673267326732669535026732673267326732673
2673267326732673267326732673267326732673267326732673267330465350267326732673267326732673267
3267326732673267326732673267326732673267326732669534653502673267326732673267326732673267326
7326732673267326732673267326732673267330465346535026732673267326732673267326732673267326732
6732673267326732673267326732669534653465350267326732673267326732673267326732673267326732673
2673267326732673267330465346534653502673267326732673267326732673267326732673267326732673267
3267326732669534653465346535026732673267326732673267326732673267326732673267326732673267326
7330465346534653465350267326732673267326732673267326732673267326732673267326732673266953465
3465346534653502673267326732673267326732673267326732673267326732673267326733046534653465346
5346535026732673267326732673267326732673267326732673267326732673266953465346534653465346535
0267326732673267326732673267326732673267326732673267326733046534653465346534653465350267326
7326732673267326732673267326732673267326732673266953465346534653465346534653502673267326732
6732673267326732673267326732673267326733046534653465346534653465346535026732673267326732673
2673267326732673267326732673266953465346534653465346534653465350267326732673267326732673267
3267326732673267326733046534653465346534653465346534653502673267326732673267326732673267326
7326732673266953465346534653465346534653465346535026732673267326732673267326732673267326732
6733046534653465346534653465346534653465350267326732673267326732673267326732673267326695346
5346534653465346534653465346534653502673267326732673267326732673267326732673304653465346534
6534653465346534653465346535026732673267326732673267326732673267326695346534653465346534653
4653465346534653465350267326732673267326732673267326732673304653465346534653465346534653465
3465346534653502673267326732673267326732673267326695346534653465346534653465346534653465346
5346535026732673267326732673267326732673304653465346534653465346534653465346534653465346535
0267326732673267326732673267326695346534653465346534653465346534653465346534653465350267326
7326732673267326732673304653465346534653465346534653465346534653465346534653502673267326732
6732673267326695346534653465346534653465346534653465346534653465346535026732673267326732673
2673304653465346534653465346534653465346


playing with the patters 

showCF (1/0.002673)

"374.111485222596333707444818555929667040778151889263000374111

gcd 374111485222596333707444818555929667040778151889263 377

13

getting another of the patterns 

showCF (1/0.003465)

"288.600288600288600288 ....


*Main> gcd 600288 377

13

more digits after .....

*Main> gcd 5742 377

29

----

# Vol2. Base Prime Products (BPP)


Base Prime Product is a group of semiprimes related with n based on product of prime indices. This technique allow you to keep the order information when the product was executed.

The result always is a semiprime number (2 prime factors). All numbers can be converted to base prime product and viceversa, less primes and semiprime numbers.



## The concept

----Haskell

*Main> baseprimesprod [19,19,19,29,31] []

9345286133

*Main> baseprimesprod [19,19,29,19,31] []

9717178811

----

As you can see in this operation, the result of the product change depending the order of the factors. This allows to make operations with numbers keeping all information in the number.


## Change decimal base to base prime product

*Main> ppfactors 6166241


[(19,3),(29,1),(31,1)]

*Main> 19 * 19

361

*Main> nthPrime 361

Prime 2437

*Main> 

*Main> 2437 * 19

46303

*Main> nthPrime it

Prime 563009

*Main> 563009 * 29

16327261

*Main> nthPrime 16327261

Prime 301460843

*Main> 301460843 * 31

9345286133

----

## Get original factors with order 


----Haskell

*Main> ppfactors 9345286133

[(31,1),(301460843,1)]

*Main> primeCount 301460843

16327261

*Main> ppfactors 16327261

[(29,1),(563009,1)]

*Main> primeCount 563009

46303

*Main> ppfactors it

[(19,1),(2437,1)]

*Main> primeCount 2437

361

*Main> ppfactors it

[(19,2)]



----


# The Base Prime Product series (from 4 to 100)


*Main> map toprimeproduct [4..100]


[4,5,6,7,14,9,10,11,21,13,14,15,86,17,39,19,35,21,22,23,129,25,26,69,49,29,65,31,886,33,34,35,219,37,38,39,215,41,91,43,77,115,46,47,1329,49,145,51,91,53,501,55,301,57,58,59,365,61,62,161,13766,65,143,67,119,69,203,71,2181,73,74,235,133,77,169,79,2215,1041,82,83,511,85,86,87,473,89,835,91,161,93,94,95,20649,97,301,253,745]


# The Base Prime Product series (from 1000 to 1100)

*Main> map toprimeproduct [1000..1100]
 
[330235,5057,2171,1003,1757,3149,1006,1007,2706529,1009,2929,1011,8947,1013,13117,4321,5461,2599,1018,1019,41939,1021,3139,4247,12942000398,3977,68039,1027,1799,25613,2987,1031,31261,1033,3713,14513,8399,1037,2249,1039,253903,1041,1042,1043,39643,4883,1046,1047,5633,1049,92113,1051,1841,107809,4309,1055,2559293,1057,4577,1059,7897,1061,9853,1063,37867,3337,4141,1067,6497,1069,3103,16099,29681,1073,2327,4171,1883,1077,21923,1079,3576295,1081,1082,5111,1897,4619,2353,1087,2534309,17677,3161,1091,47567,1093,1094,3431,5891,1097,10187,1099,62227]



# The Base Prime Product series (from 10000 to 10100)

*Main> map toprimeproduct [10000..10100]
[552708655,10001,21671,10003,114253,251749,10006,10007,2673109,10009,1913717,57581,17521,66371,21697,10015,2154379,1145701,10018,10019,411989,10021,10022,42919,356747,38897,93019,10027,103223,10029,208211,10031,30178327,10033,46883,140713,90131,10037,111613,10039,331069,10041,10042,54863,47652487,340751,10046,45901,23405089,10049,881653,70127,81493,25691,36103,10055,304613,10057,52537,34967,74947,10061,822977,10063,2832091,225761,30917,10067,61247,10069,212053,129431,54137,10073,144467,318463,89081,10077,10078,10079,7268079427,10081,58291,10083,17647,10085,157727,50959,392947,179891,29261,10091,535949,10093,205279,31631,279533,10097,8024867,10099,571357]





# Contributors

All contributors are welcome , please commit your changes

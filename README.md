
This project pertains to the classification of functions, operations on sets, calculations of sums, matrix operations, generating new numbers from an existing set, and deriving formulas for arithmetics sequences.

[1] Define a function get_function_type(func, func_domain, func_co_domain) that receives a function "func" and two sets - domain and co_domain and determines whether the function passed in is "partial", "bijectve", "injective", or "surjective". First, compute func_range by evaluating the function for each value of the domain.

If for any value in the domain, the function evaluates to something outside the co_domain, then the function is partial (and not total), and you can exit early. Otherwise, 
If the size of the domain is the same as the size of the range, and the range is the same as the co_domain, the the function is bijective.
If the range is the same as the co_domain, then  the function is surjective.
If the size of the range is the same as the size of the domain, then the function is injective

[2] Define functions that compute the following set operations. The first five functions are binary and should accept two arguments each, while the last function is unary and should accept only one argument. You may use built-in functions to your language or implement them yourself.     

set_union(set1, set2) : X ∪ Y
set_intersection(set1, set2): X ∩ Y
set_difference(set1, set2): X − Y
set_symmetric_difference(set1, set2):  X ∆  Y
set_cartesian_product(set1, set2): X x Y
set_power_set(set1):  P(X)

For example, if your sets are 
X = {"a", "ab", "abc", "abcd"}
Y = {"a", "bb", "ccc", "dddd"} 

Your output would be something like (with or without the quotes)
X = {a,ab,abc,abcd}
Y = {a,bb,ccc,dddd}
XUY = {a,ab,abc,abcd, bb, ccc, dddd}
X∩Y = {a}
X−Y = {ab, abc, abcd}
X∆Y = {ab, abc, abcd, bb, ccc, dddd}
XxY = {[a,a], [ab,a], [abc,a], [abcd,a], [a,bb], [ab,bb], [abc,bb], [abcd,bb],  [a,ccc], [ab,ccc], [abc,ccc], [abcd,ccc], [a,dddd], [ab,dddd], [abc,dddd], [abcd,dddd]}
P(X) = {{}, {a}, {ab}, {abc}, {abcd}, {a,ab}, {a,abc}, {a,abcd}, {ab,abc}, {ab, abcd}, {abc, abcd}, {a,ab,abc}, {a,ab,abcd}, {a,abc,abcd}, {ab,abc,abcd}, {a,ab,abc,abcd}}

Note: notation is very important when detailing with sets and tuples. Sets should be enclosed in curly braces. Tuples (ordered pairs, triples, etc.) should be enclosed in round parentheses or square brackets. To help achieve the proper notation, one strick is to convert the answer to a string, and then use the replace function as needed.

[3] Consider the following sums of series:

counting(n) = 1 + 2 + … + n
squares(n) = 1^2 + 2^2 + 3^2 + … + n^2
cubes(n) = 1^3 + 2^3 + 3^3 + … + n^3
geomteric_series(a, r, n) = a + ar + ar^2 + … + ar^n
arithemetic_series(n, a, d) = a + ad + 2ad + … + nad 

[a] Write functions to compute each sum by looping over the terms
[b] Write functions to compute each term by using a closed-form formula available in slides or online
[c] Compare the results (since there may be roundoff error, do not look for equality but rather a difference less than 1)

[4] Consider a pair of matrices

[a] Code the generation of random n x n integer matrices and call it twice. 
[b] Code the sum of the matrices using loops, list comprehension,  and numpy.matrix.sum(), and compare the results
[c] Code the product of the matrices using loops, list comprehension,  and numpy.multiply(), and compare the results.

[5] Define a function generate_binary_number(binary_numbers) that takes a list of the string representation of some fractional binary numbers in the range [0,1) and uses Cantor's diagonalization argument to find a number not already in that list. Be mindful of the case where there are more elements in the list than digits in each number, and assume implicit trailing zeros as needed. 

For example, for the following list,

0.010011 # 0 x 1/2 + 1 x 1/4 + 0 x 1/8 + 0 x 1/16 + 1 x 1/32 + 1 x 1/64 
0.101010 # 1 x 1/2 + 0 x 1/4 + 1 x 1/8 + 0 x 1/16 + 1 x 1/32 + 0 x 1/64 
0.111000
0.000111
0.111111
0.111000

the generated number would be: 0.110001

[6] Define a  function that derives a closed-form formula for the summation s(n, p) = 1p + 2p + … + np, using the method demonstrated in class, i.e. derive the coefficients of a polynomial of degree p+1 that gives s(n, p)

Follow these steps for a given p. (In class, we did it for p = 2)
Define a Python function s(n, p) that uses a loop to find s(n, p) = 1p + 2p + … + np
Generate the "vector" (one-dimensional matrix) b by finding for each row j = 0, 1, 2, …, p, the value of s(j, p)
Generate the "matrix" A by finding for each row j = 0, 1, 2, …, p, the values jp+1, jp, jp-1,..., j0. 
Solve the system of linear equations Ax = b. The solution vector x will be the coefficients of the polynomial.
Test the polynomial to see that it matches s(n, p)

To solve a system of linear equations, use the module numpy.linalg. See the example about halfway down on https://numpy.org/doc/stable/reference/generated/numpy.linalg.solve.html 

We are using the approach above as it gives programming experience with functions, polynomials, summations, and matrices. Optionally, you can also try to generate the closed-form formula using https://en.wikipedia.org/wiki/Faulhaber%27s_formula and compare that result with the one obtained as described above.

Test your function for the cases p = 1, 2, 3, 4, 5. (For cases 1, 2 and 3, compare to the formulas from class.) 


def calc_sum(n, p):
     return sum([i**p for i in range(n+1)])
    s = 0
    for i in range(n + 1):
        s += i ** p
    return s


 Generate the "vector" (one-dimensional matrix) b by finding for each row j = 0, 1, 2, …, p, the value of s(j, p)
def generate_vector(p):
    # return [sum_power(j,p) for j in range(p+1)]
    vector = []
    for j in range(p + 1):
        vector.append(calc_sum(j, p))
    return vector


 Generate the "matrix" A by finding for each row j = 0, 1, 2, …, p, the values jp+1, jp, jp-1,..., j0.
def calc_matrix_A(p):
    A = []
    for j in range(p + 2):
        row = []
        for k in range(p + 1, -1, -1):
            row.append(j ** k)
        A.append(row)
    return A


def calc_vector_b(p):
    b = []
    for j in range(p + 2):
        b.append(calc_sum(j, p))
    return b


 represent the summation as a readable polynomial
def string_summation(n, p):
    s = ""
    for i in range(1, n + 1):
        s += f"{i}^{p}" + (" + " if (i < n) else "")
    return s


 represent the formula in a readable format
def string_formula(formula, p):
    s = ""
    term = p + 1
    for coef in formula:
        # fraction = elem.as_integer_ratio()
        # print(str(fraction[0])+"/"+str(fraction[1]) + "n^"+ str(i), end = "")
        s += str(round(coef, 3)) + "n^" + str(term) + (" + " if term > 0 else "")
        term -= 1
    return s


 Solve the system of linear equations Ax = b. The solution vector x will be the coefficients of the polynomial.

def derive_formula(p):
    a = calc_matrix_A(p)
    print("A:")
    print(a)
    b = calc_vector_b(p)
    print("b:")
    print(b)
    x = np.linalg.solve(a, b)
    print("x:")
    print(x)
    return x


 Calculate the sum using the formula that we derived
def calc_sum_with_formula(coefs, n):
    result = 0
    term = len(coefs) - 1
    for coef in coefs:
        result += coef * (n ** term)
        term -= 1
    return result


 Solve the system of linear equations Ax = b. The solution vector x will be the coefficients of the polynomial.
 Test the polynomial to see that it matches s(n, p)


def question_6():
    # values = [1,4,9]
    # sums = [0]
    # for value in values:
    #     sums.append(sums[-1] + value)
    # print(sums)
    #
    # vector = generate_vector(3)
    # print(vector)

    dr = 5  # digits for rounding
    n = int(input("Please enter the number of terms (n) in each summation: "))
    m = int(input("Please enter the maximum power (p) to test: "))
    for p in range(1, m + 1):
        coefs = derive_formula(p)
        sum_with_formula = round(calc_sum_with_formula(coefs, n), dr)
        sum_with_loop = round(calc_sum(n, p), dr)
        print("Case n =", n, ", p =", p, ":")
        print("Sum with formula", string_formula(coefs, p), "=", sum_with_formula)
        print("Sum with loop", string_summation(n, p), "=", sum_with_loop)
        print("Match" if (sum_with_loop == sum_with_formula) else "No match")
        print()

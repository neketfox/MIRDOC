Low Level Routines
---

In these routines a big parameter can also be used wherever a flash is specified, but not vice versa. Further information may be gleaned from the (lightly) commented source code. An asterisk after
the name indicates that the function does not take a mip parameter if MR_GENERIC_MT is defined in
mirdef.h.

## void absol* (flash x, flash y)

Gives absolute value of a big or flash number.

**Parameters:**

←|x The number whose absolute value is to be computed<br />
→y = |x|

## void add (big x, big y, big z)

Adds two big numbers.

**Parameters:**

←x<br />
→y<br />
→z = x + y

**Example:**
```
add(x, x, x); // This doubles the value of x
```
## int big_to_bytes (int max, big x, char * ptr, BOOL justify)

Converts a positive big number into a binary octet string. Error checking is carried out to ensure that the
function does not write beyond the limits of ptr if `max > 0`. If `max = 0`, no checking is carried out. If `max > 0 and justify = TRUE`, the output is right-justified, otherwise leading zeros are supressed.

**Parameters:**

←max Maximum number of octets to be written in ptr<br />
←x The original big number<br />
→ptr Destination of the binary octet string<br />
→justify If TRUE, the output is right-justified, otherwise leading zeros are supressed.

**Returns:**

The number of bytes generated in ptr. If justify = TRUE then the return value is max.

**Precondition:**

max must be greater than 0 if justify = TRUE

## void bigbits (int n, big x)

Generates a big random number of given length. Uses the built-in simple random number generator initialised
by irand().

**Parameters:**

←n The desired length of the random big number<br />
→x The random number

## mr_small brand (void)

Generates random integer number.

**Returns:**

A random integer number.

**Precondition:**

First use must be preceded by an initial call to irand().

> This generator is not cryptographically strong. For cryptographic applications, use the strong_rng() routine.

## void bytes_to_big (int len, char * ptr, big x)

Converts a binary octet string to a big number. Binary to big conversion.

**Parameters:**

←len Length of ptr<br />
←ptr Byte array of the binary octet string<br />
→x Big result

**Example:**
```
#include <stdio.h>
#include "miracl.h"

int main()
{
int i, len;
miracl *mip = mirsys(100, 0);
big x, y;
char b[200]; // b needs space allocated to it
x = mirvar(0); // all big variables need to be "mirvar"ed
y = mirvar(0);

expb2(100, x);
incr(x, 3, x); // x = 2^100 + 3

len = big_to_bytes(200, x, b, FALSE);
// Now b contains big number x in raw binary
// It is len bytes in length

// now print out the raw binary number b in hex
for (i = 0; i < len; i++) printf("%02x", b[i]);
printf("n");

// now convert it back to big format, and print it out again
bytes_to_big(len, b, y);
mip->IOBASE = 16;
cotnum(y, stdout);

return 0;
}
```
## int cinnum (flash x, FILE * filep)

Inputs a flash/big number from the keyboard or a file, using as number base the current value of the instance variable miracl::IOBASE. Flash numbers can be entered using either a slash '/' to indicate numerator and denominator, or with a radix point.

**Parameters:**

→x Big/flash number

←filep File descriptor. For input from the keyboard specify stdin, otherwise as the descriptor of
some other opened file.

> To force input of a fixed number of bytes, set the instance variable miracl::INPLEN to the required number, just before calling cinnum().

**Example:**
```
mip->IOBASE = 256;
mip->INPLEN = 14; // this inputs 14 bytes from fp and
cinnum(x, fp); // converts them into big number x
```
## int cinstr (flash x, char * string)

Inputs a flash/big number from a character string, using as number base the current value of the instance
variable miracl::IOBASE. Flash numbers can be input using a slash '/' to indicate numerator and denominator,
or with a radix point.

**Parameters:**

→x<br />
←string

**Returns:**

The number of input characters.

**Example:**
```
// input large hex number into big x
mip->IOBASE = 16;
cinstr(x, "AF12398065BFE4C96DB723A");
```
## int compare* (big x, big y)

Compares two big numbers.

**Parameters:**

←x<br />
→y

**Returns:**

+1 if x > y; 0 if x = y; -1 if x < y

## void convert (int n, big x)

Converts an integer number to big number format.

**Parameters:**

←n<br />
→x

## void copy* (flash x, flash y)

Copies a big/flash number to another.

**Parameters:**

←x<br />
→y= x

**Parameters:**If x and y are the same variable, no operation is performed.

## int cotnum (flash x, FILE * filep)

Outputs a big/flash number to the screen or to a file, using as number base the value currently assigned to
the instance variable miracl::IOBASE. A flash number will be converted to radix-point representation if
the instance variable miracl::RPOINT = ON. Otherwise it will output as a fraction.

**Parameters:**

←x Big/flash number to be output<br />
→filep File descriptor. If stdout then output will be to the screen, otherwise to the file opened with
descriptor filep.

**Returns:**

Number of output characters.

**Example:**
```
// This outputs x in hex, to the file associated with fp
mip->IOBASE = 16;
cotnum(x, fp);
```
## int cotstr (flash x, char * string)

Outputs a big/flash number to the specified string, using as number base the value currently assigned to the
instance variable miracl::IOBASE. A flash number will be converted to radix-point representation if the
instance variable miracl::RPOINT = ON. Otherwise it will be output as a fraction.

**Parameters:**

←x<br />
→string

**Returns:**

Number of output characters.

> There is nothing to prevent this routine from overflowing the limits of the user supplied
> character array string, causing obscure runtime problems. It is the programmer's responsibility to
> ensure that string is big enough to contain the number output to it. Alternatively use the internally
> declared instance string miracl::IOBUFF, which is of size miracl::IOBSIZ. If this array overflows a
> MIRACL error will be flagged.

## void decr (big x, int n, big z)

Decrements a big number by an integer amount.

**Parameters:**

←x<br />
←n<br />
→z = x − n

## void divide (big x, big y, big z)

Divides one big number by another: z = x/y, x = x (mod y). The quotient only is returned if x and z
are the same, the remainder only if y and z are the same.

**Parameters:**

←→x<br />
→y<br />
→z

**Precondition:**

Parameters x and y must be different, and y must be non-zero.

> See also: **normalise()**

## BOOL divisible (big x, big y)

Tests a big number for divisibility by another.

**Parameters:**

←x<br />
→y

**Returns:**

TRUE if y divides x exactly, otherwise FALSE.

**Precondition:**

The parameter y must be non-zero.

## void* ecp_memalloc (int num)

Reserves space for a number elliptic curve points in one heap access. Individual points can subsequently
be initialised from this memory by calling epoint_init_mem().

**Parameters:**

←num The number of elliptic curve points to reserve space for.

**Returns:**

A pointer to the allocated memory.

## void ecp_memkill (char * mem, int num)

Deletes and sets to zero the memory previously allocated by ecp_memalloc().

**Parameters:**

→mem Pointer to the memory to be erased and deleted<br />
←num The size of the memory in elliptic curve points

**Precondition:**

Must be preceded by a call to ecp_memalloc().

## int exsign* (flash x)

Extracts the sign of a big/flash number.

**Parameters:**

←x A big/flash number

**Returns:**

The sign of x, i.e. -1 if x is negative, +1 if x is zero or positive.

## miracl* get_mip ()

Gets the current Miracl Instance Pointer.

**Returns:**

The mip (Miracl Instance Pointer) for the current thread.

**Precondition:**

This function does not exist if MR_GENERIC_MT is defined.

## int getdig (big x, int i)

Extracts a digit from a big number.

**Parameters:**

←x A big number<br />
→i The position of the digit to be extracted from x

**Returns:**

The value of the requested digit.

> Returns rubbish if required digit does not exist.

## unsigned int igcd* (unsigned int x, unsigned int y)

Calculates the Greatest Common Divisor of two integers using Euclid's Method.

**Parameters:**

←x<br />
←y

**Returns:**

The GCD of x and y.

## void incr (big x, int n, big z)

Increments a big number.

**Parameters:**

←x<br />
←n<br />
→z = x + n

**Example:**
```
incr(x, 2, x); // This increments x by 2
```
## BOOL init_big_from_rom (big x, int len, const mr_small * rom, int romsize, int * romptr)

Initialises a big variable from ROM memory.

**Parameters:**

→rx A big number<br />
←len Length of the big number in computer words<br />
←rom Address of ROM memory which stores up to romsize computer words<br />
←romsize<br />
←→romptr A pointer into ROM. This pointer is incremented internally as ROM memory is accessed
to fill x

**Returns:**

TRUE if successful, or FALSE if an attempt is made to read beyond the end of the ROM.

## BOOL init_point_from_rom (epoint * P, int len, const mr_small * rom, int romsize, int * romptr)

Initialises an elliptic curve point from ROM memory.

**Parameters:**

→P An elliptic curve point<br />
←len Length of the two big coordinates of P, in computer words<br />
←rom Address of ROM memory which stores up to romsize computer words<br />
←romsize<br />
←→romptr A pointer into ROM. This pointer is incremented internally as ROM memory is accessed
to fill P

**Returns:**

TRUE if successful, or FALSE if an attempt is made to read beyond the end of the ROM.

## int innum (flash x, FILE * filep)

Inputs a big/flash number from a file or the keyboard, using as number base the value specified in the
initial call to mirsys(). Flash numbers can be entered using either a slash '/' to indicate numerator and
denominator, or with a radix point.

**Parameters:**

→x A big/flash number<br />
←filep A file descriptor. For input from the keyboard specify stdin, otherwise the descriptor of
some other opened file.

**Returns:**

The number of characters input.

**Precondition:**

The number base specified in mirsys() must be less than or equal to 256. If not use cinnum() instead.

> For fastest inputting of ASCII text to a big number, and if a full-width base is possible, use mirsys(...,256) initially. This has the same effect as specifying mirsys(...,0), except that now ASCII bytes may be input directly via innum(x, fp) without the time-consuming change of base implicit in the use of cinnum().

## void insign* (int s, flash x)

Forces a big/flash number to a particular sign.

**Parameters:**

←s The sign the big/flash is to take<br />
→x = s|x|

**Example:**
```
insign(PLUS, x); // force x to be positive
```
## int instr (flash x, char * string)

Inputs a big or flash number from a character string, using as number base the value specified in the
initial call to mirsys(). Flash numbers can be entered using either a slash '/' to indicate numerator and
denominator, or with a radix point.

**Parameters:**

→x<br />
←string

**Returns:**

The number of characters input.

**Precondition:**

The number base specified in mirsys() must be less than or equal to 256. If not use cinstr() instead.

## void irand (mr_unsign32 seed)

Initialises internal random number system. Long integer types are used internally to yield a generator with
maximum period.

**Parameters:**

←seed A seed used to start off the random number generator.

## void lgconv (long n, big x)

Converts a long integer to big number format.

**Parameters:**

←n<br />
→x
## void mad (big x, big y, big z, big w, big q, big r)

Multiplies, adds and divides big numbers. The initial product is stored in a double-length internal variable
to avoid the possibility of overflow at this stage.

**Parameters:**

←x<br />
←y<br />
←z<br />
←w<br />
→q = (xy + z)/w<br />
→r The remainder

> If w and q are not distinct variables then only the remainder is returned; if q and r are not distinct then only the quotient is returned. The addition of z is not done if x and z (or y and z) are the same.

**Precondition:**

Parameters w and r must be distinct. The value of w must not be zero.

**Example:**
```
mad(x, x, x, w, x, x,); // x = x^2 / w
```
## void* memalloc (int num)

Reserves space for big/flash variables in one heap access. Individual big/flash variables can subsequently
be initialised from this memory by calling mirvar_mem().

**Parameters:**

←num The number of big/flash variables to reserve space for.

**Returns:**

A pointer to the allocated memory

## void memkill (char * mem, int len)

Deletes and sets to zero the memory previously allocated by memalloc().

**Parameters:**

→mem A pointer to the memory to be erased and deleted<br />
←len The size of that memory in bigs

**Precondition:**

Must be preceded by a call to memalloc()

## void mirexit (void)

Cleans up after the current instance of MIRACL, and frees all internal variables. A subsequent call to
mirsys() will re-initialise the MIRACL system.

**Precondition:**

Must be called after mirsys().

## void mirkill* (big x)

Securely kills off a big/flash number by zeroising it, and freeing its memory.

**Parameters:**

←x.

## miracl* mirsys (int nd, mr_small nb)

Initialises the MIRACL system for the current program thread, as described below. Must be called before
attempting to use any other MIRACL routines

1. The error tracing mechanism is initialised.
2. The number of computer words to use for each big/flash number is calculated from nd and nb.
3. Sixteen big work variables (four of them double length) are initialised.
4. Certain instance variables are given default initial values.
5. The random number generator is started by calling irand(0L).

**Parameters:**

←nd The number of digits to use for each big/flash variable. If negative, it is taken as indicating the
size of big/flash numbers in 8-bit bytes<br />
→nb The number base

**Returns:**

The Miracl Instance Pointer, via which all instance variables can be accessed, or NULL if there was
not enough memory to create an instance

**Precondition:**

The number base nb should normally be greater than 1 and less than or equal to MAXBASE. A base of
0 implies that the 'full-width' number base should be used. The number of digits nd must be less than
a certain maximum, depending on the underlying type mr_utype and on whether or not MR_FLASH
is defined

**Example:**
```
// This initialises the MIRACL system to use 500 decimal digits for each
// big or flash number
miracl *mip = mirsys(500, 10);
```
## flash mirvar (int iv)

Initialises a big/flash variable by reserving a suitable number of memory locations for it. This memory may
be released by a subsequent call to the function mirkill().

**Parameters:**

←iv An integer initial value for the big/flash number

**Returns:**

A pointer to the reserved memory

**Example:**
```
flash x;
x = mirvar(8); // Creates a flash variable x = 8
```
## flash mirvar_mem (char * mem, int index)

Initialises memory for a big/flash variable from a pre-allocated byte array mem. This array may be created
from the heap by a call to memalloc(), or in some other way. This is quicker than multiple calls to mirvar().

**Parameters:**

←mem A pointer to the pre-allocated array<br />
←index An index into that array. Each index should be unique

**Returns:**

An initialised big/flash variable

**Precondition:**

Sufficient memory must have been allocated and pointed to by mem.

## void multiply (big x, big y, big z)

Multiplies two big numbers.

**Parameters:**

←x<br />
←y<br />
→z = xy

## void negify* (flash x, flash y)

Negates a big/flash number.

**Parameters:**

←x<br />
→y = - x

> negify(x,x) is valid and sets x = - x

## mr_small normalise (big x, big y)

Multiplies a big number such that its most significant word is greater than half the number base. If such
a number is used as a divisor by divide(), the division will be carried out faster. If many divisions by the
same divisor are required, it makes sense to normalise the divisor just once beforehand.

**Parameters:**

←x<br />
→y = nx

**Returns:**

n, the normalising multiplier

> Use with care. Used internally.

## int numdig (big x)

Determines the number of digits in a big number.

**Parameters:**

←x

**Returns:**

The number of digits in x.

## int otnum (flash x, FILE * filep)

Outputs a big/flash number to the screen or to a file, using as number base the value specified in the initial
call to mirsys(). A flash number will be converted to radix-point representation if the instance variable
miracl::RPOINT = ON. Otherwise it will be output as a fraction.

**Parameters:**

←x A big/flash number<br />
←filep A file descriptor. If stdout then output will be to the screen, otherwise to the file opened with descriptor filep

**Returns:**

Number of output characters

**Precondition:**

The number base specified in mirsys() must be less than or equal to 256. If not, use cotnum() instead.

## int otstr (flash x, char * string)

Outputs a big or flash number to the specified string, using as number base the value specified in the initial
call to mirsys(). A flash number will be converted to radix-point representation if the instance variable
miracl::RPOINT = ON. Otherwise it will be output as a fraction.

**Parameters:**

←x A big/flash number<br />
→string A representation of x

**Returns:**

Number of output characters

**Precondition:**

The number base specified in mirsys() must be less than or equal to 256. If not, use cotstr() instead

> There is nothing to prevent this routine from overflowing the limits of the user supplied
> character array string, causing obscure runtime problems. It is the programmer's responsibility to
> ensure that string is big enough to contain the number output to it. Alternatively use the internally
> declared instance string miracl::IOBUFF, which is of size miracl::IOBSIZ. If this array overflows a
> MIRACL error will be flagged.

## void premult (big x, int n, big z)

Multiplies a big number by an integer.

**Parameters:**

←x<br />
←n<br />
→z = nx

## void putdig (int n, big x, int i)

Sets a digit of a big number to a given value.

**Parameters:**

←n The new value for the digit<br />
→x A big number<br />
←i A digit position

**Precondition:**

The digit indicated must exist.

## int remain (big x, int n)

Finds the integer remainder, when a big number is divided by an integer.

**Parameters:**

←x<br />
←n

**Returns:**

The integer remainder.

## void set_io_buffer_size (int len)

Sets the size of the input/output buffer. By default this is set to 1024, but programs that need to handle very
large numbers may require a larger I/O buffer.

**Parameters:**

←len The size of I/O buffer required

> Destroys the current contents of the I/O buffer.

## void set_user_function (BOOL(*)(void) user)

Supplies a user-specified function, which is periodically called during some of the more time-consuming
MIRACL functions, particularly those involved in modular exponentiation and in finding large prime numbers.
The supplied function must take no parameters and return a BOOL value. Normally this should be
TRUE. If FALSE then MIRACL will attempt to abort its current operation. In this case the function should
continue to return FALSE until control is returned to the calling program. The user-supplied function
should normally include only a few instructions, and no loops, otherwise it may adversely impact the speed
of MIRACL functions

Once MIRACL is initialised, this function may be called multiple times with a new supplied function. If
no longer required, call with a NULL parameter.

**Parameters:**

←user A pointer to a user-supplied function, or NULL if not required

**Example:**
```
// Windows Message Pump
static BOOL idle()
{
MSG msg;
if (PeekMessage(&msg, NULL, 0, 0, PM_NOREMOVE))
{
if (msg.message != WM_QUIT)
{
if (PeekMessage(&msg, NULL, 0, 0, PM_REMOVE))
{
// do a Message Pump
TranslateMessage(&msg);
DispatchMessage(&msg);
}
}
else
return FALSE;
}
return TRUE;
}
...
set_user_function(idle);
```
## int size* (big x)

Tries to convert big number to a simple integer. Also useful for testing the sign of big/flash variable as in:
if (size(x) < 0) ...

**Parameters:**

←x A big number

**Returns:**

The value of x as an integer. If this is not possible (because x is too big) it returns the value plus or
minus MR_TOOBIG.

## int subdiv (big x, int n, big z)

Divides a big number by an integer

**Parameters:**

←x<br />
←n<br />
→z = x/n

**Returns:**

The integer remainder

**Precondition:**

The value of n must not be zero.

## BOOL subdivisible (big x, int n)

Tests a big number for divisibility by an integer.

**Parameters:**

←x<br />
←n

**Returns:**

TRUE if n divides x exactly, otherwise FALSE

**Precondition:**

The value of n must not be zero.

## void subtract (big x, big y, big z)

Subtracts two big numbers.

**Parameters:**

←x<br />
←y<br />
→z = x − y

## void zero* (flash x)

Sets a big/flash number to zero.

**Parameters:**

→x



------------------------------------------------
------------------------------------------------
------------------------------------------------


Advanced Arithmetic Routines
---

In these routines a big parameter can also be used wherever a flash is specified, but not vice versa. Further information may be gleaned from the (lightly) commented source code. An asterisk after
the name indicates that the function does not take a mip parameter if MR_GENERIC_MT is defined in
mirdef.h.

## void bigdig (int n, int b, big x)

Generates a big random number of given length. Uses the built-in simple random number generator initialised
by irand().

**Parameters:**

←n<br />
←b<br />
→x A big random number n digits long to base b

**Precondition:**

The base b must be printable, that is 2 <= b <= 256

**Example:**
```
// This generates a 100 decimal digit random number
bigdig(100, 10, x);
```
## void bigrand (big w, big x)

Generates a big random number. Uses the built-in simple random number generator initialised by irand().

**Parameters:**

←w<br />
→x A big random number in the range 0 <= x < w

## void brick_end* (brick * b)

Cleans up after an application of the Comb method.

**Parameters:**

←b A pointer to the current instance.

## BOOL brick_init (brick * b, big g, big n, int window, int nb)

Initialises an instance of the Comb method for modular exponentiation with precomputation. Internally
memory is allocated for 2w big numbers which will be precomputed and stored. For bigger w more space
is required, but the exponentiation is quicker. Try w = 8.

**Parameters:**

←→b A pointer to the current instance<br />
←g The fixed generator<br />
←n The modulus<br />
←window The window size w<br />
←nb The maximum number of bits to be used in the exponent

**Returns:**

TRUE if successful, otherwise FALSE

> If MR_STATIC is defined in mirdef.h, then the g parameter in this function is replaced by an mr_small pointer to a precomputed table. In this case the function returns a void.

## void crt (big_chinese * c, big * u, big x)

Applies the Chinese Remainder Theorem.

**Parameters:**

←c A pointer to the current instance<br />
←u An array of big remainders<br />
→x The big number which yields the given remainders u when it is divided by the big moduli specified
in a prior call to crt_init()

**Precondition:**

The routine crt_init() must be called first.

## void crt_end* (big_chinese * c)

Cleans up after an application of the Chinese Remainder Theorem.

**Parameters:**

←c A pointer to the current instance.

## BOOL crt_init (big_chinese * c, int r, big * moduli)

Initialises an instance of the Chinese Remainder Theorem. Some internal workspace is allocated.

**Parameters:**

→c A pointer to the current instance<br />
←r The number of co-prime moduli<br />
←moduli An array of at least two big moduli

**Returns:**

TRUE if successful, otherwise FALSE.

## int egcd (big x, big y, big z)

Calculates the Greatest Common Divisor of two big numbers.

**Parameters:**

←x<br />
←y<br />
←z = gcd(x,y)

**Returns:**

GCD as integer, if possible, otherwise MR_TOOBIG.

## void expb2 (int n, big x)

Calculates 2 to the power of an integer as a big.

**Parameters:**

←n<br />
→x = 2n

**Example:**
```
// This calculates and prints out the largest known prime number
// (on a true 32-bit computer with lots of memory!)
expb2(1398269, x);
decr(x, 1, x);
mip->IOBASE = 10;
cotnum(x, stdout);
```
### void expint (int b, int n, big x)

Calculates an integer to the power of an integer as a big.

**Parameters:**

←b<br />
←n<br />
→x = bn

### void fft_mult (big x, big y, big z)

Multiplies two big numbers, using the Fast Fourier Method. See [Pollard71].

**Parameters:**

←x<br />
←y<br />
→z = xy

> Should only be used on a 32-bit computer when x and y are ver large, at least 1000 decimal digits.

### void gprime (int maxp)

Generates all prime numbers up to a certain limit into the instance array miracl::PRIMES, terminated by
zero. This array is used internally by the routines isprime() and nxprime().

**Parameters:**

←maxp A positive integer indicating the maximum prime number to be generated. If maxp = 0 the
miracl::PRIMES array is deleted.

### int hamming (big x)

Calculates the hamming weight of a big number (in fact the number of 1's in its binary representation).

**Parameters:**

←x

**Returns:**

Hamming weight of x.

### mr_small invers* (mr_small x, mr_small y)

Calculates the inverse of an integer modulus a co-prime integer.

**Parameters:**

←x<br />
←y

**Returns:**

x−1 (mod y)

> Result unpredictable if x and y not co-prime.

### BOOL isprime (big x)

Tests whether or not a big number is prime using a probabilistic primality test. The number is assumed
to be prime if it passes this test miracl::NTRY times, where miracl::NTRY is an instance variable with a
default initialisation in routine mirsys().

**Parameters:**

←x

**Returns:**

TRUE if x is (almost certainly) prime, otherwise FALSE

> This routine first test divides x by the list of small primes stored in the instance array miracl::PRIMES.
> The testing of larger primes will be significantly faster in many cases if this list is increased. See
> **gprime()**. By default only the small primes less than 1000 are used.

### int jac (mr_small x, mr_small n)

Calculates the value of the Jacobi symbol. See [Reisel].

**Parameters:**

←x<br />
←n

**Returns:**

The value of (x | n) as +1 or -1, or 0 if symbol undefined

> See also: **jack**

### int jack (big U, big V)

Calculates the value of the Jacobi symbol. See [Reisel].

**Parameters:**

←U<br />
←V

**Returns:**

The value of (U | V) as +1 or -1, or 0 if symbol undefined

> See also: **jac**

### int logb2 (big x)

Calculates the approximate integer log to the base 2 of a big number (in fact the number of bits in it).

**Parameters:**

←x

**Returns:**

Number of bits in x

### void lucas (big p, big r, big n, big vp, big v)

Performs Lucas modular exponentiation. Uses Montgomery arithmetic internally. This function can be
speeded up further for particular moduli, by invoking special assembly language routines to implement
Montgomery arithmetic. See powmod().

**Parameters:**

←p The base<br />
←r The exponent<br />
←n The modulus<br />
→vp = Vr−1(p) (mod n)<br />
→v = Vr(p) (mod n)

> Only v is returned if v and vp are not distinct. The "sister" Lucas function Ur(p) can, if required, be calculated as Ur(p) * [pVr(p) − 2Vr−1(p)]/(p2 − 4) (mod n)

**Precondition:**

The value of n must be odd.

### BOOL multi_inverse (int m, big * x, big n, big * w)

Finds the modular inverses of many numbers simultaneously, exploiting Montgomery's observation that
x−1 = y(xy)−1, y−1 = x(xy)−1. This will be quicker, as modular inverses are slow to calculate, and this
way only one is required.

**Parameters:**

←m The number of inverses required<br />
←x An array of m numbers whose inverses are required<br />
←n The modulus<br />
→w The resulting array of inverses

**Returns:**

TRUE if successful, otherwise FALSE

**Precondition:**

The parameters x and w must be distinct.

### BOOL nroot (big x, int n, big w)

Extracts lower approximation to a root of a big number.

**Parameters:**

←x A big number<br />
←n A positive integer<br />
→w = [nx]

**Returns:**

TRUE if the root is exact, otherwise FALSE

**Precondition:**

The value of n must be positive. If x is negative, then n must be odd

> See also: **sqroot, nres_sqroot**

### BOOL nxprime (big w, big x)

Finds next prime number.

**Parameters:**

←w<br />
←x The next prime number greater than w

**Returns:**

TRUE if successful, otherwise FALSE

> See also: **nxsafeprime**

### BOOL nxsafeprime (int type, int subset, big w, big p)

Finds next safe prime number greater than w. A safe prime number p is defined here to be one for which
q = (p − 1)/2 (type=0) or q = (p + 1)/2 (type=1) is also prime.

**Parameters:**

←type The type of safe prime as above<br />
←subset If subset = 1, then the search is restricted so that the value of the prime q is congruent to 1
mod 4. If subset = 3, then the search is restricted so that the value of q is congruent to 3 mod 4.
If subset = 0 then there is no condition on q: it can be either 1 or 3 mod 4<br />
←w<br />
→p

**Returns:**

TRUE if successful, otherwise FALSE

> See also: **nxprime**

### void pow_brick (brick * b, big e, big w)

Carries out a modular exponentiation, using the precomputed values stored in the brick structure.

**Parameters:**

←b A pointer to the current instance<br />
←e A big exponent<br />
→w = ge (mod n), where g and n are specified in the initial call to brick_init()

**Precondition:**

Must be preceded by a call to brick_init().

### void power (big x, long n, big z, big w)

Raises a big number to an integer power.

**Parameters:**

←x A big number<br />
←n A positive integer<br />
←z A big number<br />
→w = xn (mod z)

**Precondition:**

The value of n must be positive.

### int powltr (int x, big y, big n, big w)

Raises an int to the power of a big number modulus another big number. Uses Left-to-Right binary
method, and will be somewhat faster than powmod() for small x. Uses Montgomery arithmetic internally
if the modulus n is odd.

**Parameters:**

←x<br />
←y<br />
←n<br />
→w = xy (mod n)

**Returns:**

The result expressed as an integer, if possible. Otherwise the value MR_TOOBIG

**Precondition:**

The value of y must be positive. The parameters x and n must be distinct.

### void powmod (big x, big y, big n, big w)

Raises a big number to a big power modulus another big. Uses a sophisticated 5-bit sliding window technique,
which is close to optimal for popular modulus sizes (such as 512 or 1024 bits). Uses Montgomery
arithmetic internally if the modulus n is odd.

This function can be speeded up further for particular moduli, by invoking special assembly language
routines (ir your compiler allows it). A KCM Modular Multiplier will be automatically invoked if MR_-
KCM has been defined in mirdef.h and has been set to an appropriate size. Alternatively a Comba modular
multiplier will be used if MR_COMBA is so defined, and the modulus is of the specified size. Experimental
coprocessor code will be called if MR_PENTIUM is defined. Only one of these conditionals should be
defined.

**Parameters:**

←x<br />
←y<br />
←n<br />
→w = xy (mod n)

**Precondition:**

The value of y must be positive. The parameters x and n must be distinct.

### void powmod2 (big x, big y, big a, big b, big n, big w)

Calculates the product of two modular exponentiations. This is quicker than doing two separate exponentiations,
and is useful for certain cryptographic protocols. Uses 2-bit sliding window.

**Parameters:**

←x<br />
←y<br />
←a<br />
←b<br />
←n<br />
→w = xy ab (mod n)

**Precondition:**

The values of y and b must be positive. The parameters n and w must be distinct. The modulus n must
be odd.

### void powmodn (int n, big * x, big * y, big p, big w)

Calculates the product of n modular exponentiations. This is quicker than doing n separate exponentiations,
and is useful for certain cryptographic protocols. Extra memory is allocated internally for this function.

**Parameters:**

←n<br />
←x<br />
←y<br />
←p<br />
→w = x[0]y[0]x[1]y[1] · · · x[n − 1]y[n−1) (mod p)

**Precondition:**

The values of y[ ] must be positive. The parameters p and w must be distinct. The modulus p must be
odd. The underlying number base must be a power of 2.

### void scrt (small_chinese * c, mr_utype * u, big x)

Applies Chinese Remainder Theorem (for small prime moduli).

**Parameters:**

←c A pointer to the current instance of the Chinese Remainder Theorem<br />
←u An array of remainders<br />
→x The big number which yields the given integer remainders u[ ] when it is divided by the integer
moduli specified in a prior call to scrt_init()

**Precondition:**

The routine scrt_init() must be called first.

### void scrt_end* (small_chinese * c)

Cleans up after an application of the Chinese Remainder Theorem.

**Parameters:**

←c A pointer to the current instance of the Chinese Remainder Theorem.

### BOOL scrt_init (small_chinese * c, int r, mr_utype * moduli)

Initialises an instance of the Chinese Remainder Theorem. Some internal workspace is allocated.

**Parameters:**

→c A pointer to the current instance<br />
←r The number of co-prime moduli<br />
←moduli An array of at least two integer moduli

**Returns:**

TRUE if successful, otherwise FALSE.

### void sftbit (big x, int n, big z)

Shifts a big integer left or right by a number of bits.

**Parameters:**

←x<br />
←n If positive shifts to the left, if negative shifts to the right<br />
→z = x shifted by n bits

### mr_small smul* (mr_small x, mr_small y, mr_small n)

Multiplies two integers mod a third.

**Parameters:**

←x<br />
←y<br />
←n

**Returns:**

xy (mod n)

### mr_small spmd* (mr_small x, mr_small n, mr_small m)

Raises an integer to an integer power modulo a third.

**Parameters:**

←x<br />
←n<br />
←m

**Returns:**

xn (mod m)

### mr_small sqrmp* (mr_small x, mr_small m)

Calculates the square root of an integer modulo an integer prime number.

**Parameters:**

←x<br />
←m A prime number

**Returns:**

x (mod m), or 0 if root does not exist

**Precondition:**

p must be prime, otherwise the result is unpredictable

> See also: **sqroot**

### BOOL sqroot (big x, big p, big w)

Calculates the square root of a big integer mod a big integer prime.

**Parameters:**

←x<br />
←p<br />
→w =x (mod p) if the square root exists, otherwise w = 0. Note that the "other" square root
may be found by subtracting w from p

**Returns:**

TRUE if the square root exists, FALSE otherwise

**Precondition:**

The number p must be prime

> This routine is particularly efficient if p = 3 (mod 4).

### int trial_division (big x, big y)

Dual purpose trial division routine. If x and y are the same big variable then trial division by the small
prime numbers in the instance array miracl::PRIMES is attempted to determine the primality status of the
big number. If x and y are distinct then, after trial division, the unfactored part of x is returned in y.

**Parameters:**

←x<br />
←→y

**Returns:**

If x and y are the same, then a return value of 0 means that the big number is definitely not prime, a
return value of 1 means that it definitely is prime, while a return value of 2 means that it is possibly
prime (and that perhaps further testing should be carried out). If x and y are distinct, then a return value
of 1 means that x is smooth, that it is completely factored by trial division (and y is the largest prime
factor). A return value of 2 means that the unfactored part y is possibly prime.

### int xgcd (big x, big y, big xd, big yd, big z)

Calculates extended Greatest Common Divisor of two big numbers. Can be used to calculate modular
inverses. Note that this routine is much slower than a mad() operation on numbers of similar size.

**Parameters:**

←x<br />
→y<br />
→xd<br />
→yd<br />
→z = gcd(x, y) = (x * xd) + (y * yd)

**Returns:**

GCD as integer, if possible, otherwise MR_TOOBIG

**Precondition:**

If xd and yd are not distinct, only xd is returned. The GCD is only returned if z distinct from both xd
and yd

**Example:**
```
xgcd(x, p, x, x, x,); // x = 1/x mod p (p is prime)
```





----------------------------------------------------------
----------------------------------------------------------
----------------------------------------------------------


Montgomery Arithmetic Routines
---

In these routines a big parameter can also be used wherever a flash is specified, but not vice versa. Further information may be gleaned from the (lightly) commented source code. An asterisk after
the name indicates that the function does not take a mip parameter if MR_GENERIC_MT is defined in
mirdef.h.

## void nres (big x, big y)

Converts a big number to n-residue form.

**Parameters:**

←x<br />
→y the n-residue form of x

**Precondition:**

Must be preceded by call to prepare_monty()

> See also: **redc**

## void nres_dotprod (int m, big * x, big * y, big w)

Finds the dot product of two arrays of n-residues. So-called "lazy" reduction is used, in that the sum of
products is only reduced once with respect to the Montgomery modulus. This is quicker---nearly twice as fast.

**Parameters:**

←m<br />
←x An array of m n-residues<br />
←y An array of m n-residues<br />
→w =Σxiyi (mod n), where n is the current Montgomery modulus

**Precondition:**

Must be preceded by call to prepare_monty().

## void nres_double_modadd (big x, big y, big w)

Adds two double length bigs modulo pR, where R = 2n and n is the smallest multiple of the word-length
of the underlying MIRACL type, such that R > p. This is required for lazy reduction.

**Parameters:**

←x<br />
←y<br />
→w = a + b (mod pR)

## void nres_double_modsub (big x, big y, big w)

Subtracts two double length bigs modulo pR, where R = 2n and n is the smallest multiple of the wordlength
of the underlying MIRACL type, such that R > p. This is required for lazy reduction.

**Parameters:**

←x<br />
←y<br />
→w = a − b (mod pR)

## void nres_lazy (big a0, big a1, big b0, big b1, big r, big i)

Uses the method of lazy reduction combined with Karatsuba's method to multiply two zzn2 variables.
Requires just 3 multiplications and two modular reductions.

**Parameters:**

←a0<br />
←a1<br />
←b0<br />
←b1<br />
→r = the "real part" of (a0 + a1i)(b0 + b1i)<br />
→i = the "imaginary part" of (a0 + a1i)(b0 + b1i)

## void nres_lucas (big p, big r, big vp, big v)

Modular Lucas exponentiation of an n-residue.

**Parameters:**

←p An n-residue<br />
←r A big exponent<br />
→vp = Vr−1(p) (mod n) where n is the current Montgomery modulus<br />
→v = Vr(p) (mod n) where n is the current Montgomery modulus

> Only v is returned if v and vp are the same big variable.

**Precondition:**

Must be preceded by call to prepare_monty() and conversion of the first parameter to n-residue form.
Note that the exponent is not converted to n-residue form

> See also: **lucas**

## void nres_modadd (big x, big y, big w)

Modular addition of two n-residues.

**Parameters:**

←x<br />
←y<br />
→w = x + y (mod n), where n is the current Montgomery modulus

**Precondition:**

Must be preceded by a call to prepare_monty().

## int nres_moddiv (big x, big y, big w)

Modular division of two n-residues.

**Parameters:**

←x<br />
←y<br />
→w = x/y (mod n), where n is the current Montgomery modulus

**Returns:**

GCD of y and n as an integer, if possible, or MR_TOOBIG. Should be 1 for a valid result

**Precondition:**

Must be preceded by call to prepare_monty() and conversion of parameters to n-residue form. Parameters
x and y must be distinct.

## void nres_modmult (big x, big y, big w)

Modular multiplication of two n-residues. Note that this routine will invoke a KCM Modular Multiplier if
MR_KCM has been defined in mirdef.h and set to an appropriate size for the current modulus, or a Comba
fixed size modular multiplier if MR_COMBA is defined as exactly the size of the modulus.

**Parameters:**

←x<br />
←y<br />
→w = xy (mod n), where n is the current Montgomery modulus

**Precondition:**

Must be preceded by call to prepare_month() and conversion of parameters to n-residue form.

### void nres_modsub (big x, big y, big w)

Modular subtraction of two n-residues.

**Parameters:**

←x<br />
←y<br />
→w = x − y (mod n), where n is the current Montgomery modulus

**Precondition:**

Must be preceded by a call to prepare_monty().

### BOOL nres_multi_inverse (int m, big * x, big * w)

Finds the modular inverses of many numbers simultaneously, exploiting Montgomery's observation that
x−1 = y(xy)−1, y−1 = x(xy)−1. This will be quicker, as modular inverses are slow to calculate, and this
way only one is required.

**Parameters:**

←m The number of inverses required<br />
←x An array of m n-residues whose inverses are wanted<br />
→w An array with the inverses of za x

**Returns:**

TRUE if successful, otherwise FALSE

**Precondition:**

The parameters x and w must be distinct.

### void nres_negate (big x, big w)

Modular negation.

**Parameters:**

←x An n-residue number<br />
→w = −x (mod n), where n is the current Montgomery modulus

**Precondition:**

Must be preceded by a call to prepare_monty().

### void nres_powltr (int x, big y, big w)

Modular exponentiation of an n-residue.

**Parameters:**

←x<br />
←y<br />
→w = xy (mod n), where n is the current Montgomery modulus

**Precondition:**

Must be preceded by call to prepare_monty(). Note that the small integer x and the exponent are not
converted to n-residue form.

### void nres_powmod (big x, big y, big w)

Modular exponentiation of an n-residue.

**Parameters:**

←x An n-reside number, the base<br />
←y A big number, the exponent<br />
→w = xy (mod n), where n is the current Montgomery modulus

**Precondition:**

Must be preceded by call to prepare_monty() and conversion of the first parameter to n-residue form.
Note that the exponent is not converted to n-residue form

> See also: **nres_powltr, nres_powmod2**

**Example:**
```
prepare_monty(n);
...
nres(x, y); // convert to n-residue form
nres_powmod(y, e, z);
redc(z, w); // convert back to normal form
```
### void nres_powmod2 (big x, big y, big a, big b, big w)

Calculates the product of two modular exponentiations involving n-residues.

**Parameters:**

←x An n-residue number<br />
←y A big integer<br />
←a An n-residue number<br />
←b A big integer<br />
→w = xy ab (mod n), where n is the current Montgomery modulus

**Precondition:**

Must be preceded by call to prepare_monty() and conversion of the appropriate parameters to n-residue
form. Note that the exponents are not converted to n-residue form

> See also: **nres_powltr, nres_powmod**

### void nres_powmodn (int n, big * x, big * y, big w)

Calculates the product of n modular exponentiations involving n-residues. Extra memory is allocated
internally by this function.

**Parameters:**

←n The number of n-residue numbers<br />
←x An array of n n-residue numbers<br />
←y An array of n big integers<br />
→w = x[0]y[0]x[1]y[1] · · · x[n − 1]y[n−1) (mod p), where p is the current Montgomery modulus

**Precondition:**

Must be preceded by call to prepare_monty() and conversion of the appropriate parameters to n-residue
form. Note that the exponents are not converted to n-residue forms.

### void nres_premult (big x, int k, big w)

Multiplies an n-residue by a small integer.

**Parameters:**

←x<br />
←k<br />
→w = kx (mod n), where n is the current Montgomery modulus

**Precondition:**

Must be preceded by call to prepare_monty() and conversion of the first parameter to n-residue form.
Note that the small integer is not converted to n-residue form

> See also: **nres_modmult**

### BOOL nres_sqroot (big x, big w)

Calculates the square root of an n-residue mod a prime modulus.

**Parameters:**

←x<br />
→w =x (mod n), where n is the current Montgomery modulus

**Returns:**

TRUE if the square root exists, otherwise FALSE

**Precondition:**

Must be preceded by call to prepare_monty() and conversion of the first parameter to n-residue form.

### mr_small prepare_monty (big n)

Prepares a Montgomery modulus for use. Each call to this function replaces the previous modulus (if any).

**Parameters:**

←n A big number which is to be the Montgomery modulus

**Returns:**

None

**Precondition:**

The parameter n must be positive and odd. Allocated memory is freed when the current instance of
MIRACL is terminated by a call to mirexit().

### void redc (big x, big y)

Converts an n-residue back to normal form.

**Parameters:**

←x an n-residue<br />
→y the normal form of the n-residue x

**Precondition:**

Must be preceded by call to prepare_monty()

> See also: **nres**




----------------------------------------------------------
----------------------------------------------------------
----------------------------------------------------------




ZZn2 Arithmetic Routines
---

In these routines a big parameter can also be used wherever a flash is specified, but not vice versa. Further information may be gleaned from the (lightly) commented source code. An asterisk after
the name indicates that the function does not take a mip parameter if MR_GENERIC_MT is defined in
mirdef.h.

## void zzn2_add (zzn2 * x, zzn2 * y, zzn2 * w)

Adds two zzn2 variables.

**Parameters:**

←x<br />
←y<br />
→w = x + y

## BOOL zzn2_compare* (zzn2 * x, zzn2 * y)

Compares two zzn2 variables for equality.

**Parameters:**

←x<br />
←y

**Returns:**

TRUE if x = y, otherwise FALSE.

## void zzn2_conj (zzn2 *x, zzn2 *w)

Finds the conjugate of a zzn2.

**Parameters:**

←x<br />
→w If x = a + bi, then w = a - bi

## void zzn2_copy* (zzn2 * x, zzn2 * w)

Copies one zzn2 to another.

**Parameters:**

←x<br />
→w = x

## void zzn2_from_big (big x, zzn2 * w)

Creates a zzn2 from a big integer. This is converted internally into n-residue format.

**Parameters:**

←x<br />
→w = x

## void zzn2_from_bigs (big x, big y, zzn2 * w)

Creates a zzn2 from two big integers. These are converted internally into n-residue format.

**Parameters:**

←x<br />
←y<br />
→w = x + yi

## void zzn2_from_int (int i, zzn2 * w)

Converts an integer to zzn2 format.

**Parameters:**

←i<br />
→w = i

> See also: **zzn2_from_ints**

## void zzn2_from_ints (int i, int j, zzn2 * w)

Creates a zzn2 from two integers.

**Parameters:**

←i<br />
←j<br />
→w = i + j i

> See also: **zzn2_from_int**

## void zzn2_from_zzn (big x, zzn2 * w)

Creates a zzn2 from a big already in n-residue format.

**Parameters:**

←x<br />
→w = x

### void zzn2_from_zzns (big x, big y, zzn2 * w)

Creates a zzn2 from two bigs already in n-residue format.

**Parameters:**

←x<br />
←y<br />
→w = x + y i

> See also: **zzn2_from_zzn**

### void zzn2_imul (zzn2 * x, int y, zzn2 * w)

Multiplies a zzn2 variable by an integer.

**Parameters:**

←x<br />
←y<br />
→w = xy

### void zzn2_inv (zzn2 * w)

In-place inversion of a zzn2 variable.

**Parameters:**

←→w = 1 / w

### BOOL zzn2_isunity (zzn2 * x)

Tests a zzn2 value for equality to one.

**Parameters:**

←x

**Returns:**

TRUE if x is one, otherwise FALSE.

### BOOL zzn2_iszero* (zzn2 * x)

Tests a zzn2 value for equality to zero.

**Parameters:**

←x

**Returns:**

TRUE if x is zero, otherwise FALSE.

### void zzn2_mul (zzn2 * x, zzn2 * y, zzn2 * w)

Multiplies two zzn2 variables. If x and y are the same variable, a faster squaring method is used.

**Parameters:**

←x<br />
←y<br />
→w = xy

### void zzn2_negate (zzn2 * x, zzn2 * w)

Negates a zzn2

**Parameters:**

←x<br />
→w = -x

### void zzn2_sadd (zzn2 * x, big y, zzn2 * w)

Adds a big in n-residue format to a zzn2.

**Parameters:**

←x<br />
←y<br />
→w = x + y

### void zzn2_smul (zzn2 * x, big y, zzn2 * w)

Multiplies a zzn2 variable by a big in n-residue.

**Parameters:**

←x<br />
←y<br />
→w = xy

### void zzn2_ssub (zzn2 * x, big y, zzn2 * w)

Subtracts a big in n-residue format from a zzn2.

**Parameters:**

←x<br />
←y<br />
→w = x - y

### void zzn2_sub (zzn2 * x, zzn2 * y, zzn2 * w)

Subtracts two zzn2 variables.

**Parameters:**

←x<br />
←y<br />
→w = x - y

### void zzn2_timesi (zzn2 * u)

In-place multiplication of a zzn2 by i, the imaginary square root of the quadratic non-residue.

**Parameters:**

←→u If u = a + bi then on exit u = i2b + ai

### void zzn2_zero* (zzn2 * w)

Sets a zzn2 variable to zero.

**Parameters:**

→w = 0

## Encryption Routines <a id="encryption"></a>

**Functions**
- mr_unsign32 aes_decrypt* (aes *a, char *buff)
- mr_unsign32 aes_encrypt* (aes *a, char *buff)
- void aes_end* (aes *a)
- void aes_getreg* (aes *a, char *ir)
- BOOL aes_init* (aes *a, int mode, int nk, char *key, char *iv)
- void aes_reset* (aes *a, int mode, char *iv)
- void shs256_hash* (sha256 *sh, char hash[32])
- void shs256_init* (sha256 *sh)
- void shs256_process* (sha256 *sh, int byte)
- void shs384_hash* (sha384 *sh, char hash[48])
- void shs384_init* (sha384 *sh)
- void shs384_process* (sha384 *sh, int byte)
- void shs512_hash* (sha512 *sh, char hash[64])
- void shs512_init* (sha512 *sh)
- void shs512_process* (sha512 *sh, int byte)
- void shs_hash* (sha *sh, char hash[20])
- void shs_init* (sha *sh)
- void shs_process* (sha *sh, int byte)
- void strong_bigdig (csprng *rng, int n, int b, big x)
- void strong_bigrand (csprng *rng, big w, big x)
- void strong_init* (csprng *rng, int rawlen, char *raw, mr_unsign32 tod)
- void strong_kill* (csprng *rng)
- int strong_rng* (csprng *rng)

## mr_unsign32 aes_decrypt* (aes * a, char * buff)

Decrypts a 16 or n byte input buffer in situ. If the mode of operation is as a block cipher (MR_ECB or
MR_CBC) then 16 bytes will be decrypted. If the mode of operation is as a stream cipher (MR_CFBn,
MR_OFBn or MR_PCFBn) then n bytes will be decrypted.

**Parameters:**

←a Pointer to an initialised instance of an aes structured defined in miracl.h<br />
←→buff Pointer to the buffer of bytes to be decrypted

**Returns:**

If MR_CFBn and MR_PCFBn modes then n byte(s) that were shifted off the end of the input register
as result of decrypting the n input byte(s), otherwise 0

**Precondition:**

Must be preceded by call to aes_init().

## mr_unsign32 aes_encrypt* (aes * a, char * buff)

Encrypts a 16 or n byte input buffer in situ. If the mode of operation is as a block cipher (MR_ECB or
MR_CBC) then 16 bytes will be encrypted. If the mode of operation is as a stream cipher (MR_CFBn,
MR_OFBn or MR_PCFBn) then n bytes will be encrypted.

**Parameters:**

←a Pointer to an initialised instance of an aes structure defined in miracl.h<br />
←→buff Pointer to the buffer of bytes to be encrypted

**Returns:**

In MR_CFBn and MR_PCFBn modes the n byte(s) that were shifted off the end of the input register
as result of encrypting the n input byte(s), otherwise 0

**Precondition:**

Must be preceded by a call to aes_init().

## void aes_end* (aes * a)

Ends an AES encryption session, and de-allocates the memory associated with it. The internal session key
data is destroyed.

**Parameters:**

←→a Pointer to an initialised instance of an aes structured defined in miracl.h

## void aes_getreg* (aes * a, char * ir)

Reads the current contents of the input chaining register associated with this instance of the AES. This is
the register initialised by the IV in the calls to aes_init() and aes_reset().

**Parameters:**

←a Pointer to an instance of the aes structured, defined in miracl.h<br />
→ir A character array to hold the extracted 16-byte data

**Precondition:**

Must be preceded by a call to aes_init().

## BOOL aes_init* (aes * a, int mode, int nk, char * key, char * iv)

Initialises an Encryption/Decryption session using the Advanced Encryption Standard (AES). This is a
block cipher system that encrypts data in 128-bit blocks using a key of 128, 192 or 256 bits. See [Stinson]
for more background on block ciphers.

**Parameters:**

→a Pointer to an instance of the aes structure defined in miracl.h<br />
←mode The mode of operation to be used: MR_ECB (Electronic Code Book), MR_CBC (Cipher
Block Chaining), MR_CFBn (Cipher Feed-Back where n is 1, 2 or 4), MR_PCFBn (error Propagating
Cipher Feed-Back where n is 1, 2 or 4) or MR_OFBn (Output Feed-Back where n is 1,
2, 4, 8 or 16). The value of n indicates the number of bytes to be processed in each application.
For more information on Modes of Operation, see [Stinson]. MR_PCFBn is an invention of our
own [Scott93]<br />
←nk The size of the key in bytes. It can be either 16, 24 or 32<br />
←key A pointer to the key<br />
←iv A pointer to the Initialisation Vector (IV). A 16-byte initialisation vector should be specified for
all modes other than MR_ECB, in which case it can be NULL

**Returns:**

TRUE if successful, otherwise FALSE.

## void aes_reset* (aes * a, int mode, char * iv)

Resets the AES structure.

**Parameters:**

←a Pointer to an instance of the aes structure defined in miracl.h<br />
←mode an Indication of the new mode of operation<br />
←iv A pointer to a (possibly new) initialisation vector

## void shs256_hash* (sha256 * sh, char hash[32])

Generates a 32 byte (256 bit) hash value into the provided array.

**Parameters:**

←sh Pointer to the current instance<br />
→hash Pointer to array to be filled

## void shs256_init* (sha256 * sh)

Initialises an instance of the Secure Hash Algorithm (SHA-256). Must be called before new use.

**Parameters:**

→sh Pointer to an instance of a structure defined in miracl.h

## void shs256_process* (sha256 * sh, int byte)

Processes a single byte. Typically called many times to provide input to the hashing process. The hash
value of all the processed bytes can be retrieved by a subsequent call to shs256_hash().

**Parameters:**

←sh Pointer to the current instance<br />
←byte Character to be processed

### void shs384_hash* (sha384 * sh, char hash[48])

Generates a 48 byte (384 bit) hash value into the provided array.

**Parameters:**

←sh Pointer to the current instance<br />
→hash Pointer to array to be filled

### void shs384_init* (sha384 * sh)

Initialises an instance of the Secure Hash Algorithm (SHA-384). Must be called before new use.

**Parameters:**

! sh Pointer to an instance of a structure defined in miracl.h

**Precondition:**

The SHA-384 algorithm is only available if 64-bit data-type is defined.

### void shs384_process* (sha384 * sh, int byte)

Processes a single byte. Typically called many times to provide input to the hashing process. The hash
value of all the processed bytes can be retrieved by a subsequent call to shs384_hash().

**Parameters:**

←sh Pointer to the current instance<br />
←byte Character to be processed

### void shs512_hash* (sha512 * sh, char hash[64])

Generates a 64 byte (512 bit) hash value into the provided array.

**Parameters:**

←sh Pointer to the current instance<br />
→hash Pointer to array to be filled

### void shs512_init* (sha512 * sh)

Initialises an instance of the Secure Hash Algorithm (SHA-512). Must be called before new use.

**Parameters:**

→sh Pointer to an instance of a structure defined in miracl.h

**Precondition:**

The SHA-512 algorithm is only available if 64-bit data-type is defined.

### void shs512_process* (sha512 * sh, int byte)

Processes a single byte. Typically called many times to provide input to the hashing process. The hash
value of all the processed bytes can be retrieved by a subsequent call to shs512_hash().

**Parameters:**

←sh Pointer to the current instance<br />
←byte Character to be processed

### void shs_hash* (sha * sh, char hash[20])

Generates a twenty byte (160 bit) hash value into the provided array.

**Parameters:**

←sh Pointer to the current instance<br />
→hash Pointer to array to be filled
### void shs_init* (sha * sh)

Initialises an instance of the Secure Hash Algorithm (SHA-1). Must be called before new use.

**Parameters:**

→sh Pointer to an instance of a structure defined in miracl.h

### void shs_process* (sha * sh, int byte)

Processes a single byte. Typically called many times to provide input to the hashing process. The hash
value of all the processed bytes can be retrieved by a subsequent call to shs_hash().

**Parameters:**

←sh Pointer to the current instance<br />
←byte Character to be processed

### void strong_bigdig (csprng * rng, int n, int b, big x)

Generates a big random number of given length from the cryptographically strong generator rng.

**Parameters:**

←rng A pointer to the random number generator<br />
←n<br />
←b<br />
→x Big random number n digits long to base b

**Precondition:**

The base b must be printable, that is 2 <= b <= 256

### void strong_bigrand (csprng * rng, big w, big x)

Generates a cryptographically strong random big number x using the random number generator rng wuch
that 0 <= x < w.

**Parameters:**

←rng A pointer to the current instance<br />
←w<br />
→x

### void strong_init* (csprng * rng, int rawlen, char * raw, mr_unsign32 tod)

Initialises the cryptographically strong random number generator rng. The array raw (of length rawlen)
and the time-of-day value tod are the two sources used together to seed the generator. The former might be
provided from random keystrokes, the latter from an internal clock. Subsequent calls to strong_rng() will
provide random bytes.

**Parameters:**

→rng<br />
←rawlen<br />
←raw An array of length rawlen<br />
←tod A 32-bit time-of-day value

### void strong_kill* (csprng * rng)

Kills the internal state of the random number generator rng.

**Parameters:**

←rng A pointer to a random number generator

### int strong_rng* (csprng * rng)

Generates a sequence of cryptographically strong random bytes.

**Parameters:**

←rng A pointer to a random number generator

**Returns:**

A random byte.



----------------------------------------------------------
----------------------------------------------------------
----------------------------------------------------------



Elliptic Curve Routines
---

In these routines a big parameter can also be used wherever a flash is specified, but not vice versa. Further information may be gleaned from the (lightly) commented source code. An asterisk after
the name indicates that the function does not take a mip parameter if MR_GENERIC_MT is defined in
mirdef.h.

## void ebrick2_end* (ebrick2 * B)

Cleans up after an application of the Comb for GF(2m) elliptic curves.

**Parameters:**

←→B A pointer to the current instance

## BOOL ebrick2_init (ebrick2 * B, big x, big y, big a2, big a6, int m, int a, int b, int c, int window, int nb)

Initialises an instance of the Comb method for GF(2m) elliptic curve multiplication with precomputation.
The field is defined with respect to the trinomial basis tm+ta+1 or the pentanomial basis tm+ta+tb+tc+1.
Internally memory is allocated for 2w elliptic curve points which will be precomputed and sotred. For
bigger w more space is required, but the exponentiation is quicker. Try w = 8.

**Parameters:**

←B A pointer to the current instance<br />
←x x coordinate of the fixed point<br />
←y y coordinate of the fixed point<br />
←a2 The a2 coefficient of the curve y2 + xy = x3 + a2x2 + a6<br />
←a6 the a6 coefficient of the curve y2 + xy = x3 + a2x2 + a6<br />
←m<br />
←a<br />
←b<br />
←c<br />
←window The size w of the window<br />
←nb The maximum number of bits to be used in the exponent

**Returns:**

TRUE if successful, otherwise FALSE

> If MR_STATIC is defined in mirdef.h, then the x and y parameters in this function are replaced by a single mr_small * pointer to a precomputed table. In this case the function returns a void.

## void ebrick_end* (ebrick * B)

Cleans up after an application of the Comb for GF(p) elliptic curves.

**Parameters:**

←→B A pointer to the current instance

## BOOL ebrick_init (ebrick * B, big x, big y, big a, big b, big n, int window, int nb)

Initialises an instance of the Comb method for GF(p) elliptic curve multiplication with precomputation.
Internally memory is allocated for 2w elliptic curve points which will be precomputed and stored. For
bigger w more space is required, but the exponentiation is quicker. Try w = 8.

**Parameters:**

→B A pointer to the current instance<br />
←x x coordinate of the fixed point<br />
←y y coordinate of the fixed point<br />
←a The a coefficient of the curve y2 = x3 + ax + b<br />
←b The b coefficient of the curve y2 = x3 + ax + b<br />
←n The modulus<br />
←window The size w of the window<br />
←nb The maximum number of bits to be used in the exponent

**Returns:**

TRUE if successful, otherwise FALSE

> If MR_STATIC is defined in mirdef.h, then the x and y parameters in this function are replaced by a single mr_small * pointer to a precomputed table. In this case the function returns a void.

## big ecurve2_add (epoint * p, epoint * pa)

Adds two points on a GF(2m) elliptic curve using the special rule for addition. Note that if pa = p, then a
different duplication rule is used. Addition is quicker if p is normalised.

**Parameters:**

←p<br />
←→pa = pa + p

**Returns:**

An ephemeral pointer to the sline slope if curve is super-singular

**Precondition:**

The input points must actually be on the current active curve.

## BOOL ecurve2_init (int m, int a, int b, int c, big a2, big a6, BOOL check, int type)

Initialises the internal parameters of the current active GF(2m) elliptic curve. The curve is assumed to be
of the form y2 + xy = x3 + Ax2 + B. The field is defined with respect to the trinomial basis tm + ta + 1
or the pentanomial basis tm+ta+tb+tc+1. This routine can be called subsequently with the parameters
of a different curve.

**Parameters:**

←m<br />
←a<br />
←b<br />
←c<br />
←a2 The A coefficient on the elliptic curve equation<br />
←a6 The B coefficient on the elliptic curve equation<br />
←check If TRUE a check is made that the specified basis is irreducible. If FALSE, this basis validity
check, which is time-consuming, is supressed<br />
←type Either MR_PROJECTIVE or MR_AFFINE, specifying whether projective or affine coordinates
should be used internally. Normally the former is faster

**Returns:**

TRUE if parameters make sense, otherwise FALSE

> Allocated memory will be freed when the current instance of MIRACL is terminated by a call to mirexit(). Only one elliptic curve, GF(p) or GF(2m) may be active within a single MIRACL instance.

## void ecurve2_mult (big e, epoint * pa, epoint * pt)

Multiplies a point on a GF(2m) elliptic curve by an integer. Uses the addition/subtraction method.

**Parameters:**

←e<br />
←pa<br />
→pt = e × pa

**Precondition:**

The point pa must be on the active curve.

## void ecurve2_mult2 (big e, epoint * p, big ea, epoint * pa, epoint * pt)

Calculates the point e × p + ea × pa on a GF(2m) elliptic curve. This is quicker than doing two separate
multiplications and an addition. Useful for certain cryptosystems.

**Parameters:**

←e<br />
←p<br />
←ea<br />
←pa<br />
→pt = e × p + ea × pa

**Precondition:**

The points p and pa must be on the active curve.

## void ecurve2_multi_add (int m, epoint ** x, epoint ** w)

Simultaneously adds pairs of points on the active GF(2m) curve. This is much quicker than adding them individually, but only when using affine coordinates.

**Parameters:**

←m<br />
←x<br />
→w w[i] = w[i] + x[i] for i = 0 to m - 1

> Only useful when using affine coordinates.

> See also: **ecurve2_init**

## void ecurve2_multn (int n, big * y, epoint ** x, epoint * w)

Calculates the point x[0]y[0] + x[1]y[1] + . . . + x[n − 1]y[n − 1]) on a GF(2m) elliptic curve, for n >= 2.

**Parameters:**

←n<br />
←y an array of n big numbers<br />
←x an array of n elliptic curve points<br />
→w = x[0]y[0] + x[1]y[1] + . . . + x[n − 1]y[n − 1])

**Precondition:**

The points must be on the active curve. The y[] values must all be positive. The underlying number
base must be a power of 2.

## big ecurve2_sub (epoint * p, epoint * pa)

Subtracts two points on a GF(2m) elliptic curve. Actually negates p and adds it to pa. Subtraction is quicker
if p is normalised.

**Parameters:**

←p<br />
←→ pa = pa − p

**Returns:**

An ephemeral pointer to the sline slope

**Precondition:**

The input points must actually be on the current active curve.

## big ecurve_add (epoint * p, epoint * pa)

Adds two points on a GF(p) elliptic curve using the special rule for addition. Note that if pa = p, then a
different duplication rule is used. Addition is quicker if p is normalised.

**Parameters:**

←p<br />
←→pa = pa + p

**Returns:**

An ephemeral pointer to the sline slope

**Precondition:**

The input points must actually be on the current active curve.

## void ecurve_init (big a, big b, big p, int type)

Initialises the internal parameters of the current active GF(p) elliptic curve. The curve is assumed to be
of the form y2 = x3 + Ax + B (mod p), the so-called Weierstrass model. This routine can be called
subsequently with the parameters of a different curve.

**Parameters:**

←a The A coefficient of the elliptic curve<br />
←b The B coefficient of the elliptic curve<br />
←p The modulus<br />
→type Either MR_PROJECTIVE or MR_AFFINE, specifying whether projective or affine coordinates
should be used internally. Normally the former is faster

> Allocated memory will be freed when the current instance of MIRACL is terminated by a call to mirexit(). Only one elliptic curve, GF(p) or GF(2m) may be active within a single MIRACL instance.

## void ecurve_mult (big e, epoint * pa, epoint * pt)

Multiplies a point on a GF(p) elliptic curve by an integer. Uses the addition/subtraction method.

**Parameters:**

←e<br />
←pa<br />
→pt = e × pa

**Precondition:**

The point pa must be on the active curve.

## void ecurve_mult2 (big e, epoint * p, big ea, epoint * pa, epoint * pt)

Calculates the point e × p + ea × pa on a GF(p) elliptic curve. This is quicker than doing two separate
multiplications and an addition. Useful for certain cryptosystems.

**Parameters:**

←e<br />
←p<br />
←ea<br />
←pa<br />
→pt = e × p + ea × pa

**Precondition:**

The points p and pa must be on the active curve.

## void ecurve_multi_add (int m, epoint ** x, epoint ** w)

Simultaneously adds pairs of points on the active GF(p) curve. This is much quicker than adding them
individually, but only when using affine coordinates.

**Parameters:**

←m<br />
←x<br />
→w w[i] = w[i] + x[i] for i = 0 to m - 1

> Only useful when using affine coordinates.

> See also: **ecurve_init, nres_multi_inverse**

## void ecurve_multn (int n, big * y, epoint ** x, epoint * w)

Calculates the point x[0]y[0] + x[1] * y[1] + . . . + x[n − 1]y[n − 1] on a GF(p) elliptic curve, for n >= 2.

**Parameters:**

←n<br />
←y An array of n big numbers<br />
←x An array of n elliptic curve points<br />
→w = x[0]y[0] + x[1]y[1] + . . . + x[n − 1]y[n − 1]

**Precondition:**

The points must be on the active curve. The y[] values must all be positive. The underlying number
base must be a power of 2.

## big ecurve_sub (epoint * p, epoint * pa)

Subtracts two points on a GF(p) elliptic curve. Actually negates p and adds it to pa. Subtraction is quicker
if p is normalised.

**Parameters:**

←p<br />
←→pa = pa − p

**Returns:**

An ephemeral pointer to the sline slope

**Precondition:**

The input points must actually be on the current active curve.

## BOOL epoint2_comp (epoint * a, epoint * b)

Compares two points on the current active GF(2m) elliptic curve.

**Parameters:**

←a<br />
←b

**Returns:**

TRUE if the points are the same, otherwise FALSE.

## void epoint2_copy* (epoint * a, epoint * b)

Copies one point to another on a GF(2m) elliptic curve.

**Parameters:**

←a<br />
←b = a

## int epoint2_get (epoint * p, big x, big y)

Normalises a point and extracts its (x,y) coordinates on the active GF(2m) elliptic curve.

**Parameters:**

←p<br />
→x<br />
→y

**Returns:**

The least significant bit of y. Note that it is possible to reconstruct a point from its x coordinate and
just the least significant bit of y. Often such a 'compressed' description of a point is useful

**Precondition:**

The point p must be on the active curve

> If x and y are not distinct variables on entry then only the value of x is returned.

**Example:**
```
i = epoint2_get(p, x, x); // extract x coordinate and lsb of y/x
```
## void epoint2_getxyz (epoint * p, big x, big y, big z)

Extracts the raw (x,y,z) coordinates of a point on the active GF(2m) elliptic curve.

**Parameters:**

←p<br />
→x<br />
→y<br />
→z

**Precondition:**

The point p must be on the active curve

> If any of x, y, z is NULL then that coordinate is not returned.

## BOOL epoint2_norm (epoint * p)

Normalises a point on the current active GF(2m) elliptic curve. This sets the z coordinate to 1. Point
addition is quicker when adding a normalised point. This function does nothing if affine coordinates are
being used (in which case there is no z coordinate).

**Parameters:**

←p A point on the current active elliptic curve

**Returns:**

TRUE if successful, otherwise FALSE.

## BOOL epoint2_set (big x, big y, int cb, epoint * p)

Sets a point on the current active GF(2m) elliptic curve (if possible).

**Parameters:**

←x The x coordinate of the point<br />
←y The y coordinate of the point

←cb If x and y are not distinct variables then x only is passed to the function, and cb is taken as the
least significant bit of y. In this case the full value of y is reconstructed internally. This is known
as 'point decompression' (and is a bit time-consuming, requiring the extraction of a modular
square root)<br />
→p = (x,y)

**Returns:**

TRUE if the point exists on the current active elliptic curve, otherwise FALSE

**Example:**
```
p = epoint_init();
epoint2_set(x, x, 1, p); // decompress p
```
## BOOL epoint_comp (epoint * a, epoint * b)

Compares two points on the current active GF(p) elliptic curve.

**Parameters:**

←a<br />
←b

**Returns:**

TRUE if the points are the same, otherwise FALSE.

## void epoint_copy* (epoint * a, epoint * b)

Copies one point to another on a GF(p) elliptic curve.

**Parameters:**

←a<br />
←b = a

## void epoint_free* (epoint * p)

Frees memory associated with a point on a GF(p) elliptic curve.

**Parameters:**

←p

## int epoint_get (epoint * p, big x, big y)

Normalises a point and extracts its (x,y) coordinates on the active GF(p) elliptic curve.

**Parameters:**

←p<br />
→x<br />
→y

**Returns:**

The least significant bit of y. Note that it is possible to reconstruct a point from its x coordinate and
just the least significant bit of y. Often such a 'compressed' description of a point is useful

**Precondition:**

The point p must be on the active curve

> If x and y are not distinct variables on entry then only the value of x is returned.

**Example:**
```
i = epoint_get(p, x, x); // extract x coordinate and lsb of y
```
## void epoint_getxyz (epoint * p, big x, big y, big z)

Extracts the raw (x,y,z) coordinates of a point on the active GF(p) elliptic curve.

**Parameters:**

←p<br />
→x<br />
→y<br />
→z

**Precondition:**

The point p must be on the active curve

> If any of x, y, z is NULL then that coordinate is not returned.

## epoint* epoint_init (void)

Assigns memory to a point on a GF(p) elliptic curve, and initialises it to the 'point at infinity'

**Returns:**

A pointer to an elliptic curve point (in fact a pointer to a structure allocated from the heap)

> It is the C programmer's responsibility to ensure that all elliptic curve points initialised by a call to this function are ultimately freed by a call to epoint_free(). If not a memory leak will result.

## epoint* epoint_init_mem (char * mem, int index)

Initialises memory for an elliptic curve point from a pre-allocated byte array mem. This array may be
created from the heap by a call to ecp_memalloc(), or in some other way. This is quicker than multiple
calls to epoint_init().

**Parameters:**

←mem<br />
←index An index into mem. Each index should be unique

**Returns:**

An initialised elliptic curve point

**Precondition:**

Sufficient memory must have been allocated and pointed to by mem.

## BOOL epoint_norm (epoint * p)

Normalises a point on the current active GF(p) elliptic curve. This sets the z coordinate to 1. Point addition is quicker when adding a normalised point. This function does nothing if affine coordinates are being used (in which case there is no z coordinate).

**Parameters:**

←p A point on the current active elliptic curve

**Returns:**

TRUE if successful, otherwise FALSE.

## BOOL epoint_set (big x, big y, int cb, epoint * p)

Sets a point on the current active GF(p) elliptic curve (if possible).

**Parameters:**

←x The x coordinate of the point<br />
←y The y coordinate of the point<br />
←cb If x and y are not distinct variables then x only is passed to the function, and cb is taken as the
least significant bit of y. In this case the full value of y is reconstructed internally. This is known
as 'point decompression' (and is a bit time-consuming, requiring the extraction of a modular
square root)<br />
→p = (x,y)

**Returns:**

TRUE if the point exists on the current active elliptic curve, otherwise FALSE

**Example:**
```
p = epoint_init();
epoint_set(x, x, 1, p); // decompress p
```
## BOOL epoint_x (big x)

Tests to see if the parameter x is a valid coordinate of a point on the curve. It is faster to test an x coordinate
first in this way, rather than trying to directly set it on the curve by calling epoint_set(), as it avoids an
expensive modular square root.

**Parameters:**

←x The integer coordinate x

**Returns:**

TRUE if x is the coordinate of a curve point, otherwise FALSE.

## int mul2_brick (ebrick2 * B, big e, big x, big y)

Carries out a GF(2m) elliptic curve multiplication using the precomputed values stored in the ebrick structure.

**Parameters:**

←B A pointer to the current instance<br />
←e A big exponent<br />
→x The x coordinate of e × G, where G is specified in the initial call to ebrick2_init()<br />
→y The y coordinate of e × G, where G is specified in the initial call to ebrick2_init()

**Returns:**

The least significant bit of y

> If x and y are not distinct variables, only x is returned.

**Precondition:**

Must be preceded by a call to ebrick2_init().

## int mul_brick (ebrick * B, big e, big x, big y)

Carries out a GF(p) elliptic curve multiplication using the precomputed values stored in the ebrick structure.

**Parameters:**

←B A pointer to the current instance<br />
←e A big exponent<br />
→x The x coordinate of eG (mod n), where G and n are specified in the initial call to ebrick_init()<br />
→y The y coordinate of eG (mod n), where G and n are specified in the initial call to ebrick_init()

**Returns:**

The least significant bit of y

> If x and y are not distinct variables, only x is returned.

**Precondition:**

Must be preceded by a call to ebrick_init().

## BOOL point_at_infinity* (epoint * p)

Tests if an elliptic curve point is the 'point at infinity'.

**Parameters:**

←p An elliptic curve point

**Returns:**

TRUE if p is the point at infinity, otherwise FALSE

**Precondition:**

The point must be initialised.    



------------------------------------------------
------------------------------------------------
------------------------------------------------



Floating Slash Routines
---

In these routines a big parameter can also be used wherever a flash is specified, but not vice versa. Further information may be gleaned from the (lightly) commented source code. An asterisk after
the name indicates that the function does not take a mip parameter if MR_GENERIC_MT is defined in
mirdef.h.

## void build (flash x, int(*)(_MIPT_ big, int) gen)

Uses supplied generator of regular continued fraction expansion to build up a flash number x, rounded if
necessary.

**Parameters:**

→x The flash number created<br />
←gen The generator function

**Example:**
```
int phi(flash w, int n)
{
// rcf generator for golden ratio //
return 1;
}
...
build(x, phi);
...
// This will calculate the golden ratio (1 + sqrt(5)) / 2 in x -- very quickly!
```
## void dconv (double d, flash w)

Converts a double to flash format.

**Parameters:**

←d<br />
→w The flash equivalent of d

## void denom (flash x, big y)

Extracts the denominator of a flash number.

**Parameters:**

←x<br />
→y The denominator of x

## void facos (flash x, flash y)

Calculates arc-cosine of a flash number, using fasin().

**Parameters:**

←x<br />
→y = arccos(x)

**Precondition:**

|x| must be less than or equal to 1.

## void facosh (flash x, flash y)

Calculates hyperbolic arc-cosine of a flash number.

**Parameters:**

←x<br />
→y = arccosh(x)

**Precondition:**

|x| must be greater than or equal to 1.

## void fadd (flash x, flash y, flash z)

Adds two flash numbers.

**Parameters:**

←x<br />
←y<br />
→z = x + y

## void fasin (flash x, flash y)

Calculates arc-sin of a flash number, using fatan().

**Parameters:**

←x<br />
→y = arcsin(x)

**Precondition:**

|x| must be less than or equal to 1.

## void fasinh (flash x, flash y)

Calculates hyperbolic arc-sin of a flash number.

**Parameters:**

←x<br />
→y = arcsinh(x)

## void fatan (flash x, flash y)

Calculates the arc-tangent of a flash number, using an O(n2.5) method based on Newton's iteration.

**Parameters:**

←x<br />
→y = arctan(x)

### void fatanh (flash x, flash y)

Calculates the hyperbolic arc-tangent of a flash number.

**Parameters:**

←x<br />
→y = arctanh(x)

**Precondition:**

x2 must be less than 1

### int fcomp (flash x, flash y)

Compares two flash numbers.

**Parameters:**

←x<br />
←y

**Returns:**

-1 if y > x, +1 if x > y and 0 if x = y

### void fconv (int n, int d, flash x)

Converts a simple fraction to flash format.

**Parameters:**

←n<br />
←d<br />
→x = n/d

### void fcos (flash x, flash y)

Calculates cosine of a given flash angle, using ftan().

**Parameters:**

←x<br />
→y = cos(x)

### void fcosh (flash x, flash y)

Calculates hyperbolic cosine of a given flash angle.

**Parameters:**

←x<br />
→y = cosh(x)

### void fdiv (flash x, flash y, flash z)

Divides two flash numbers.

**Parameters:**

←x<br />
←y<br />
→z = x/y

### double fdsize (flash w)

Converts a flash number to double format.

**Parameters:**

←w

**Returns:**

The value of the parameter x as a double

**Precondition:**

The value of x must be representable as a double.

### void fexp (flash x, flash y)

Calculates the exponential of a flash number using O(n2.5) method.

**Parameters:**

←x<br />
→y = ex

### void fincr (flash x, int n, int d, flash y)

Add a simple fraction to a flash number.

**Parameters:**

←x<br />
←n<br />
←d<br />
→y = x + n/d

**Example:**
```
// This subtracts two-thirds from the value of x
fincr(x, -2, 3, x);
```
### void flog (flash x, flash y)

Calculates the natural log of a flash number using O(n2.5) method.

**Parameters:**

←x<br />
→y = log(x)

### void flop (flash x, flash y, int * op, flash z)

Performs primitive flash operation. Used internally. See source listing comments for more details.

**Parameters:**

←x<br />
←y<br />
←op<br />
→z = Fn(x,y), where the function performed depends on the parameter op.

### void fmodulo (flash x, flash y, flash z)

Finds the remainder when one flash number is divided by another.

**Parameters:**

←x<br />
←y<br />
→z = x (mod y)

### void fmul (flash x, flash y, flash z)

Multiplies two flash numbers.

**Parameters:**

←x<br />
←y<br />
→z = xy

### void fpack (big n, big d, flash x)

Forms a flash number from big numerator and denominator.

**Parameters:**

←n<br />
←d<br />
→x = n/d

**Precondition:**

The denominator must be non-zero. Flash variable x and big variable d must be distinct. The resulting
flash variable must not be too big for the representation.

### void fpi (flash pi)

Calculates π using Gauss-Legendre O(n2 log n) method. Note that on subsequent calls to this routine, π is
immediately available, as it is stored internally. (This routine is disappointingly slow. There appears to be
no simple way to calculate a rational approximation to π quickly).

**Parameters:**

→pi =π

> Internally allocated memory is freed when the current MIRACL instance is ended by a call to mirexit().

### void fpmul (flash x, int n, int d, flash y)

Multiplies a flash number by a simple fraction.

**Parameters:**

←x<br />
←n<br />
←d<br />
→y = xn/d

### void fpower (flash x, int n, flash w)

Raises a flash number to an integer power.

**Parameters:**

←x<br />
←n<br />
→w = xn

### void fpowf (flash x, flash y, flash z)

Raises a flash number to a flash power.

**Parameters:**

←x<br />
←y<br />
→z = xy

### void frand (flash x)

Generates a random flash number.

**Parameters:**

→x A flash random number in the range 0 < x < 1

### void frecip (flash x, flash y)

Calculates reciprocal of a flash number.

**Parameters:**

←x<br />
→y = 1/x

### BOOL froot (flash x, int n, flash w)

Calculates n-th root of a flash number using Newton's O(n2) method.

**Parameters:**

←x<br />
←n<br />
→w = nx

**Returns:**

TRUE for exact root, otherwise FALSE.

### void fsin (flash x, flash y)

Calculates sine of a given flash angle. Uses ftan().

**Parameters:**

←x<br />
→y = sin(x)

### void fsinh (flash x, flash y)

Calculates hyperbolic sine of a given flash angle.

**Parameters:**

←x<br />
→y = sinh(x)

### void fsub (flash x, flash y, flash z)

Subtracts two flash numbers.

**Parameters:**

←x<br />
←y<br />
→z = x − y

### void ftan (flash x, flash y)

Calculates the tan of a given flash angle, using an O(n2.5) method.

**Parameters:**

←x<br />
→y = tan(x)

### void ftanh (flash x, flash y)

Calculates the hyperbolic tan of a given flash angle.

**Parameters:**

←x<br />
→y = tanh(x)

### void ftrunc (flash x, big y, flash z)

Separates a flash number to a big number and a flash remainder.

**Parameters:**

←x<br />
→y = int(x)<br />
→z The fractional remainder. If y is the same as z, only int(x) is returned

### void mround (big num, big den, flash z)

Forms a rounded flash number from big numerator and denominator. If rounding takes place the instance
variable EXACT is set to FALSE. EXACT Is initialised to TRUE in routine mirsys(). This routine is used
internally.

**Parameters:**

←num<br />
←den<br />
→z = R(num/dem)--- the flash number num/dem is rounded if necessary to fit the representation

**Precondition:**

The denominator must be non-zero.

### void numer (flash x, big y)

Extracts the numerator of a flash number.

**Parameters:**

←x<br />
→y the numerator of x


----------------------------------------------------------
----------------------------------------------------------
----------------------------------------------------------


MIRACL Structure Reference
---

## MIRACL Instance Pointer

`#include <miracl.h>`

## Field Documentation

`BOOL ERCON` - errors by default generate an error message and immediately abort the program. Alternatively by setting mip->ERCON=TRUE error control is left to the user.

`int ERNUM` - number of the last error that occurred.

`BOOL EXACT` - initialised to TRUE. Set to FALSE if any rounding takes place during flash arithmetic.

`int INPLEN` - length of input string. Must be used when inputting binary data.

`int IOBASE` - the 'printable' number base to be used for input and output. May be changed at will within a program. Must be greater than or equal to 2 and less than or equal to 256.

`int IOBSIZ` – size of I/O buffer.

`char* IOBUFF` – input/output buffer.

`int NTRY` - number of iterations used in probabilistic primality test by isprime(). Initialised to 6.

`int* PRIMES` – pointer to a table of small prime numbers.

`BOOL RPOINT` - if set to ON numbers are output with a radix point. Otherwise they are output as fractions (the default).

`BOOL TRACER` - if set to ON causes debug information to be printed out, tracing the progress of all subsequent calls to MIRACL routines. Initialised to OFF.
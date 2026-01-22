
# 🚀 Comprehensive Fortran Tutorial

*(From classic roots to modern scientific computing)*

Fortran (FORmula TRANslation) is one of the oldest high-level programming languages and is still **heavily used in high-performance computing (HPC), numerical methods, physics, climate modeling, finance, and engineering.** Modern Fortran (2003–2018) is expressive, safe, modular, and fast.

---

# 📚 Table of Contents

1. What is Fortran Today?
2. Installing a Fortran Compiler
3. Your First Fortran Program
4. Language Basics
5. Variables and Data Types
6. Input and Output
7. Control Flow
8. Arrays and Matrices
9. Procedures (Functions & Subroutines)
10. Modules and Code Organization
11. File I/O
12. Derived Types (Structs / OOP)
13. Modern Fortran OOP
14. Numerical Computing Examples
15. Parallelism (OpenMP & Coarrays)
16. Interfacing with C
17. Building Larger Projects
18. Scientific Libraries
19. Best Practices
20. Example Mini-Project

---

# 1️⃣ What is Fortran Today?

Modern Fortran supports:

* ✔️ Modules & namespaces
* ✔️ Dynamic memory
* ✔️ Object-oriented programming
* ✔️ Parallel programming
* ✔️ C interoperability
* ✔️ Extremely fast array math

It’s especially good for:

* Numerical simulation
* Linear algebra
* Signal processing
* Machine learning backends
* Weather, astronomy, physics

---

# 2️⃣ Installing a Compiler

### ✅ Windows

Install **MSYS2** or **WSL**, then:

```bash
pacman -S mingw-w64-x86_64-gcc-fortran
```

or inside WSL:

```bash
sudo apt install gfortran
```

### ✅ Linux

```bash
sudo apt install gfortran
```

### ✅ macOS

```bash
brew install gcc
```

Check:

```bash
gfortran --version
```

---

# 3️⃣ Your First Fortran Program

Create `hello.f90`

```fortran
program hello
    implicit none
    print *, "Hello, Fortran world!"
end program hello
```

Compile and run:

```bash
gfortran hello.f90 -o hello
./hello
```

---

# 4️⃣ Language Basics

### Key syntax rules

* Free-form source (.f90+)
* Case-insensitive
* Strong typing
* Whitespace mostly irrelevant

```fortran
implicit none   ! Forces explicit declarations (ALWAYS use this)
```

---

# 5️⃣ Variables and Data Types

### Intrinsic types

```fortran
integer         :: count
real            :: x
double precision :: y
logical         :: done
character(len=20) :: name
```

Preferred modern style:

```fortran
use iso_fortran_env
real(real64) :: pi
```

Constants:

```fortran
real, parameter :: PI = 3.1415926
```

---

# 6️⃣ Input and Output

### Print

```fortran
print *, "Value:", x
```

### Read

```fortran
read *, x
```

### Formatted I/O

```fortran
print '(A,F6.2)', "Temperature: ", temp
```

---

# 7️⃣ Control Flow

### Conditionals

```fortran
if (x > 0) then
    print *, "Positive"
else if (x < 0) then
    print *, "Negative"
else
    print *, "Zero"
end if
```

### Loops

```fortran
do i = 1, 10
    print *, i
end do
```

### While loop

```fortran
do while (x < 100)
    x = x * 1.5
end do
```

---

# 8️⃣ Arrays and Matrices

### Static

```fortran
real :: a(10)
real :: m(3,3)
```

### Dynamic

```fortran
real, allocatable :: arr(:)
allocate(arr(100))
```

### Array math (🔥 powerful)

```fortran
b = sin(a) + sqrt(c)
m = matmul(a, b)
```

### Slicing

```fortran
a(1:10:2)
m(:,2)
```

---

# 9️⃣ Procedures

### Functions

```fortran
real function square(x)
    real, intent(in) :: x
    square = x*x
end function
```

### Subroutines

```fortran
subroutine normalize(v)
    real, intent(inout) :: v(:)
    v = v / norm2(v)
end subroutine
```

### Explicit interfaces (important!)

```fortran
module math_utils
contains
    real function cube(x)
        real, intent(in) :: x
        cube = x**3
    end function
end module
```

---

# 🔟 Modules and Code Organization

```fortran
module physics
    implicit none
    real, parameter :: G = 6.67430e-11
contains
    real function kinetic(m, v)
        real, intent(in) :: m, v
        kinetic = 0.5 * m * v**2
    end function
end module
```

Usage:

```fortran
use physics
```

---

# 1️⃣1️⃣ File I/O

### Write

```fortran
open(10, file="data.txt", status="replace")
write(10,*) x, y, z
close(10)
```

### Read

```fortran
open(20, file="data.txt", status="old")
read(20,*) x, y, z
close(20)
```

---

# 1️⃣2️⃣ Derived Types (Structs)

```fortran
type :: Particle
    real :: mass
    real :: position(3)
    real :: velocity(3)
end type
```

Usage:

```fortran
type(Particle) :: p
p%mass = 1.0
p%position = [0.0, 1.0, 0.0]
```

---

# 1️⃣3️⃣ Object-Oriented Fortran

```fortran
type :: Vector
    real :: v(3)
contains
    procedure :: magnitude
end type

real function magnitude(this)
    class(Vector), intent(in) :: this
    magnitude = sqrt(sum(this%v**2))
end function
```

---

# 1️⃣4️⃣ Numerical Computing Examples

### Numerical integration

```fortran
real function integrate(f, a, b, n)
    interface
        real function f(x)
            real, intent(in) :: x
        end function
    end interface

    real, intent(in) :: a, b
    integer, intent(in) :: n
    real :: h, x
    integer :: i

    h = (b - a) / n
    integrate = 0.0

    do i = 0, n-1
        x = a + i*h
        integrate = integrate + f(x)
    end do

    integrate = integrate * h
end function
```

---

# 1️⃣5️⃣ Parallel Fortran

### OpenMP

```fortran
!$omp parallel do
do i = 1, n
    a(i) = sin(b(i))
end do
!$omp end parallel do
```

Compile:

```bash
gfortran -fopenmp main.f90
```

### Coarrays

```fortran
real :: x[*]
if (this_image() == 1) x = 42
sync all
```

---

# 1️⃣6️⃣ Interfacing with C

```fortran
use iso_c_binding

interface
    function c_sqrt(x) bind(C, name="sqrt")
        import :: c_double
        real(c_double) :: c_sqrt
        real(c_double), value :: x
    end function
end interface
```

---

# 1️⃣7️⃣ Building Larger Projects

Recommended structure:

```
src/
  math.f90
  physics.f90
  main.f90
```

Compile:

```bash
gfortran src/*.f90 -O3 -march=native -o sim
```

Use **CMake** for big systems.

---

# 1️⃣8️⃣ Scientific Libraries

* **BLAS / LAPACK** – Linear algebra
* **FFTW** – Fourier transforms
* **NetCDF** – Climate data
* **PETSc** – Parallel solvers
* **HDF5** – Scientific storage

---

# 1️⃣9️⃣ Best Practices

✔️ Always use `implicit none`
✔️ Prefer modules over globals
✔️ Use `real64` precision
✔️ Enable optimization
✔️ Use array syntax
✔️ Write unit tests
✔️ Avoid legacy `COMMON` and `GOTO`
✔️ Profile with `gprof` or `perf`

---

# 2️⃣0️⃣ Mini-Project: N-Body Gravity Simulation

```fortran
module nbody
use iso_fortran_env
implicit none

type :: Body
    real(real64) :: mass
    real(real64) :: pos(3)
    real(real64) :: vel(3)
end type

contains

subroutine step(bodies, dt)
    type(Body), intent(inout) :: bodies(:)
    real(real64), intent(in) :: dt
    integer :: i
    do i = 1, size(bodies)
        bodies(i)%pos = bodies(i)%pos + bodies(i)%vel * dt
    end do
end subroutine

end module
```

---

# 🎯 Where Fortran Shines

* Supercomputers
* Climate & astrophysics
* CFD & FEM
* Numerical finance
* Signal processing
* Machine learning kernels


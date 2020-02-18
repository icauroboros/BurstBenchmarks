# BurstBenchmarks
I was curious how well Burst/IL2CPP optimizes C# code against GCC/Clang with C, so I've ported five famous benchmarks, plus a raytracer, a minified flocking simulation, particle kinematics, a stream cipher, a hashing algorithm, and radix sort, with different workloads and made them identical between the two languages. C code compiled with all possible optimizations using `-DNDEBUG -Ofast -march=native -flto` compiler options. Benchmarks were done on Windows 10 w/ AMD FX-4300 (4GHz) using standalone build. Mono JIT and RyuJIT is included for fun.

Burst 1.1.2<br/>
GCC 8.1.0<br/>
Clang (LLVM 8.0.1)<br/>
IL2CPP and Mono JIT (Unity 2019.2.0f1)<br/>
RyuJIT (.NET Core 2.2.402)

## Integer math

|          | Fibonacci         | Sieve of Eratosthenes | Arcfour           | Seahash           | Radix             |
|----------|-------------------|-----------------------|-------------------|-------------------|-------------------|
| Burst    | 95,527,914 ticks  | 43,738,580 ticks      | 97,695,014 ticks  | 354,805,953 ticks | 65,637,438 ticks  |
| GCC      | 102,998,330 ticks | 47,215,659 ticks      | 84,442,342 ticks  | 351,778,149 ticks | 68,814,202 ticks  |
| Clang    | 92,297,296 ticks  | 47,017,112 ticks      | 100,679,342 ticks | 350,264,649 ticks | 64,345,662 ticks  |
| IL2CPP   | 152,131,662 ticks | 48,402,961 ticks      | 112,297,459 ticks | 352,931,629 ticks | 71,256,563 ticks  |
| Mono JIT | 199,301,525 ticks | 56,503,823 ticks      | 260,125,453 ticks | 479,177,648 ticks | 141,545,880 ticks |
| RyuJIT   | 109,374,001 ticks | 53,213,658 ticks      | 135,955,933 ticks | 354,541,127 ticks | 79,393,575 ticks  |

## Single-precision math

|          | Mandelbrot        | Pixar Raytracer     | Fireflies Flocking  | Polynomials       | Particle Kinematics |
|----------|-------------------|---------------------|---------------------|-------------------|---------------------|
| Burst    | 37,951,741 ticks  | 345,809,604 ticks   | 184,693,828 ticks   | 52,312,299 ticks  | 190,654,223 ticks   |
| GCC      | 28,761,058 ticks  | 144,083,652 ticks   | 128,894,075 ticks   | 39,252,467 ticks  | 111,352,855 ticks   |
| Clang    | 40,431,972 ticks  | 163,787,850 ticks   | 152,781,861 ticks   | 26,659,845 ticks  | 193,759,862 ticks   |
| IL2CPP   | 97,272,960 ticks  | 282,197,013 ticks   | 383,393,328 ticks   | 46,888,288 ticks  | 185,309,296 ticks   |
| Mono JIT | 313,936,473 ticks | 2,851,363,418 ticks | 1,034,860,724 ticks | 172,780,244 ticks | 377,228,204 ticks   |
| RyuJIT   | 65,437,241 ticks  | 823,220,147 ticks   | 284,619,392 ticks   | 63,997,704 ticks  | 199,732,371 ticks   |

## Double-precision math

|          | NBody             |
|----------|-------------------|
| Burst    | 136,450,555 ticks |
| GCC      | 202,893,162 ticks |
| Clang    | 112,909,174 ticks |
| IL2CPP   | 236,041,412 ticks |
| Mono JIT | 326,669,135 ticks |
| RyuJIT   | 136,455,343 ticks |

Notes
--------
Enabled Tiered Compilation in .NET Core negatively affects performance in the tests.

Suppressing the generation of static unwind tables for exception handling with GCC using `-fno-asynchronous-unwind-tables` leads to better performance in the recursive Fibonacci test: 84,983,484 ticks. All other tests remain unaffected.

Discussion
--------
Feel free to join the discussion in the [thread](https://forum.unity.com/threads/benchmarking-burst-against-gcc-machine-code-fibonacci-mandelbrot-nbody.715133/) on Unity forums.

Supporters
--------
These wonderful people make open-source better:
<p align="left"> 
  <img src="https://github.com/Rageware/Supporters/blob/master/burstbenchmarks-supporters.png" alt="supporters">
</p>

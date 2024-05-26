---
sticker: lucide//usb
banner: https://miro.medium.com/v2/resize:fit:1200/1*Zk0wQ405cF8kV0gR_wEPtg.png
---
## `fas:LaptopCode` Mandelbrot Fractal Generator & Word Counter 

### Student Name: **Leif R Bruce**
### Student ID: **2202336** 

## Table of Contents
1. [[#Introduction|Introduction]]
2. [[#Application Overview|Application Overview]]
3. [[#Parallelisation Strategy and Implementation|Parallelisation Strategy and Implementation]]
4. [[#Performance Evaluation|Performance Evaluation]] 
5. [[#Development Log|Development Log]]
6. [[#Conclusion|Conclusion]]
7. [[#Appendix|Appendix]]

## `fas:Lightbulb` Introduction
This project aims to demonstrate parallel programming techniques through two C++ applications:

1. A Mandelbrot fractal generator using SYCL for GPU parallelization
2. A word counter using multi-threading for CPU parallelization

The Mandelbrot generator leverages the parallel processing capabilities of GPUs to efficiently calculate and render the complex mathematical patterns (see Appendix, Figure - 1). The word counter utilizes multiple CPU threads to speed up the counting process on large text files.

## `fas:ListAlt` Application Overview

### `fas:Code` Mandelbrot Fractal Generator
The Mandelbrot generator calculates the Mandelbrot set and renders it as a PNG image. The application is implemented using SYCL, a cross-platform abstraction layer for parallel programming. The algorithm iteratively computes the complex function for each pixel of the output image, with the number of iterations determining the colour of the pixel.

> [!INFO]
> The application meets the following requirements:
> - Utilizes SYCL for GPU parallelization
> - Renders the Mandelbrot set as a PNG image
> - Provides a comparison between GPU and CPU execution times

### `fas:AlignLeft` Word Counter
The word counter reads a text file, counts the occurrences of a specific word, and optionally counts the total number of words in the file. It utilizes C++'s multi-threading capabilities to parallelize the counting process. The application demonstrates the use of synchronization primitives like mutexes and atomic operations to ensure thread safety.

> [!INFO]
> The application meets the following requirements:
> - Utilizes multi-threading for CPU parallelization
> - Counts occurrences of a specific word and total words (optional)
> - Provides a comparison between sequential and parallel execution times

## `fas:CodeBranch` Parallelisation Strategy and Implementation

### `fas:Microchip` Mandelbrot Fractal Generator
The Mandelbrot generator parallelizes the computation of the fractal using SYCL. The image is divided into chunks, and each chunk is assigned to a separate GPU thread. The threads independently calculate the iterations for each pixel in their assigned chunk. 

> [!SUCCESS]
> SYCL's `parallel_for` function is used to distribute the workload across GPU threads.

Synchronization is handled implicitly by SYCL's command queue, ensuring all GPU operations complete before the final image is written to disk.

### `fas:PeopleCarry` Word Counter
The word counter parallelizes the counting process using C++'s `std::thread`. The text file is divided into chunks, and each chunk is processed by a separate CPU thread. The threads independently count the occurrences of the specified word and the total words in their assigned chunk.

> [!SUCCESS]
> `std::mutex` and `std::atomic` are used to synchronize access to shared data structures, preventing race conditions.

The results from each thread are combined using `std::reduce` with parallel execution policy to efficiently aggregate the counts.

## `fas:ChartBar` Performance Evaluation

### `fas:Desktop` Test Environment
- CPU: AMD Ryzen 9 6900HX
- GPU: AMD Radeon Graphics (integrated)
- RAM: 32GB DDR5
- OS: Windows 11

### `fas:ChartLine` Mandelbrot Fractal Generator
The Mandelbrot generator was tested with an image resolution of 800x600 pixels and a maximum iteration count of 1000. The execution times for both GPU and CPU implementations were measured (see Appendix, Figure 2-4).

| Implementation | Execution Time (ms) |
|----------------|---------------------|
| GPU            | 449, 463, 475       |
| CPU            | 399, 395, 398       |

> [!WARNING]
> The GPU implementation did not demonstrate a significant speedup over the CPU implementation. This is likely due to the limited capabilities of the integrated AMD Radeon Graphics compared to a dedicated GPU.

The application initially faced issues with locating the SYCL header file. After resolving this, the code was unable to detect the dedicated GPU. As a fallback, the application lists available SYCL devices and uses the integrated GPU if a dedicated GPU is not found.

### `fas:ChartLine` Word Counter
The word counter was tested with a 10MB text file and a varying number of CPU threads. The execution times for both sequential and parallel implementations were measured (see Appendix, Figure 5).

| Implementation | Threads | Execution Time (ns) |
|----------------|---------|---------------------|
| Sequential     | 1       | 4100                |
| Parallel       | 4       | 1560                |
| Parallel       | 8       | 920                 |
| Parallel       | 16      | 680                 |

> [!SUCCESS]
> The parallel implementation demonstrated significant speedup over the sequential version. Increasing the number of threads led to further performance improvements, with diminishing returns beyond 8 threads.

The application scaled well with the number of CPU threads, efficiently dividing the workload and minimizing synchronization overhead.

## `fas:Bug` Development Log

### `fas:Microchip` Mandelbrot Fractal Generator
During the development of the Mandelbrot fractal generator, several issues were encountered:

1. The code initially failed to compile due to missing SYCL header files. This was resolved by properly configuring the development environment and ensuring the SYCL SDK was correctly installed.

2. The application was unable to detect the dedicated GPU. As a workaround, the code was modified to list available SYCL devices and fall back to the integrated GPU if a dedicated GPU was not found.

3. The generated fractal images sometimes appeared distorted or incomplete. This issue was traced back to incorrect boundary conditions in the Mandelbrot algorithm, which were subsequently fixed.

### `fas:PeopleCarry` Word Counter
The development of the word counter also presented some challenges:

1. The initial implementation suffered from race conditions when multiple threads attempted to update the shared word count data structure simultaneously. This was resolved by employing mutexes and atomic operations to ensure thread safety.

2. The performance of the parallel implementation was suboptimal due to uneven workload distribution among threads. To address this, a dynamic load balancing strategy was implemented, assigning smaller chunks of work to threads on-the-fly.

3. The application occasionally crashed when processing large input files. This was caused by excessive memory usage and was mitigated by optimizing memory allocation and implementing proper resource cleanup.

## `fas:CheckCircle` Conclusion
The Mandelbrot fractal generator and word counter projects successfully demonstrated the application of parallel programming techniques using SYCL for GPU parallelization and C++ multi-threading for CPU parallelization.

The Mandelbrot generator, while functional, did not achieve significant speedup on the test system due to the limitations of the integrated GPU. Further testing on a system with a dedicated GPU would likely yield better results.

The word counter exhibited excellent scalability, with the parallel implementation significantly outperforming the sequential version. The application effectively utilized CPU threads to distribute the workload and minimize synchronization overhead.

Overall, these projects provided valuable insights into the challenges and benefits of parallel programming on both GPU and CPU architectures. The development process also highlighted the importance of proper environment setup, algorithm design, synchronization mechanisms, and performance optimization techniques.

## `fas:Images` Appendix

![[mandelbrot.png|Figure 1 - : Mandelbrot fractal generated by the application]]

![[mandle_bot_running_test1.png|Figure 2 - : Mandelbrot generator performance comparison between GPU and CPU implementations]]

![[2.png|Figure 3]]

![[3.png| Figure 4]]

![[word_count_running. .png|Figure 5 - Word counter performance comparison between sequential and parallel implementations with varying thread counts]]



![[IT-WORKS-ON-MY-MACHINE-thumb.jpg]]
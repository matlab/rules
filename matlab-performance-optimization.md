# MATLAB Performance Optimization

This file contains comprehensive performance optimization guidelines for MATLAB development. Use these rules when writing performance-critical MATLAB code, optimizing existing code, and building responsive applications.

## License

Based on official [MathWorks documentation](https://www.mathworks.com/help/matlab/performance-and-memory.html) and community best practices.

This work is licensed under a [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).

---

## Profiling and Benchmarking

### Measure Before Optimizing

- Always profile code before attempting optimization to identify actual bottlenecks
- Use `timeit` for accurate function benchmarking; it handles warm-up runs and returns the median of multiple measurements
- Use `tic`/`toc` for timing code sections within larger programs, but run multiple iterations for reliable measurements
- Use the MATLAB Profiler (`profile on`, `profile viewer`) for line-by-line analysis, but note it disables some JIT optimizations

### Benchmarking Best Practices

- Wrap code in a function handle for `timeit`: `t = timeit(@() myFunction(input))`
- Perform 3-5 warm-up runs before measuring with `tic`/`toc` to allow JIT compilation
- Run timing tests multiple times and use the median (more robust than mean)
- Ensure code executes for at least 0.1 seconds for reliable measurements
- Close unnecessary applications and maintain consistent test conditions

### Profiler Usage

- Start profiling with `profile on`, stop with `profile off`, view with `profile viewer`
- Look for functions consuming more than 10% of total time
- Examine self-time (excluding child calls) to find actual bottlenecks
- Use flame graphs in the Profile Summary to visualize call hierarchies

---

## Vectorization Guidelines

### Core Vectorization Principles

- Replace explicit loops with array operations whenever possible
- Use element-wise operators (`.*`, `./`, `.^`) for array computations
- Leverage built-in functions that operate on entire arrays: `sum`, `mean`, `max`, `min`, `cumsum`, `diff`
- Use logical indexing instead of `find()` when you need values rather than indices

### Implicit Expansion (R2016b+)

- Use implicit expansion instead of `repmat` or `bsxfun` for broadcasting operations
- Two arrays have compatible sizes when each dimension is either identical or one of them equals 1
- Replace legacy `bsxfun` calls: use `A - mean(A)` instead of `bsxfun(@minus, A, mean(A))`
- Implicit expansion is faster and uses less memory than `repmat`

### Page-wise Operations (R2020b+)

- Use `pagemtimes`, `pagetranspose`, `pageinv` for batch matrix operations on 3-D arrays
- Page-wise functions can be 30-40x faster than equivalent loops over slices
- Page dimensions support implicit expansion for flexible batch operations

### When Not to Vectorize

- Avoid vectorization when it requires creating very large temporary arrays that exceed memory
- Do not sacrifice significant code readability for marginal performance gains
- Recognize that modern MATLAB (R2015b+) optimizes many loop patterns well
- Accept that some algorithms with sequential dependencies cannot be vectorized

### Avoid arrayfun/cellfun for Performance

- `arrayfun` and `cellfun` are typically not faster than explicit loops
- Use them for code brevity only, not performance optimization
- Exception: `arrayfun` on `gpuArray` can create efficient GPU kernels

---

## Memory Management

### Pre-allocation

- Always pre-allocate arrays before loops: `result = zeros(n, 1)`
- Use `NaN(m, n)` when distinguishing uninitialized elements from zeros is important
- Pre-allocate directly with target type: `A = zeros(100, 'int8')` not `A = int8(zeros(100))`
- Pre-allocate cell arrays with `cell(m, n)` and string arrays with `strings(m, n)`

### Avoid Growing Arrays in Loops

- Never use patterns like `result = [result, newValue]` inside loops
- Each concatenation forces memory reallocation and data copying
- Pre-allocation can improve execution speed by 10-25x or more

### Memory-Efficient Data Types

- Use `single` instead of `double` when 7 decimal digits of precision is sufficient (halves memory)
- Use `logical` for boolean data (8x less memory than `double`)
- Use appropriate integer types (`uint8`, `int16`, `uint32`) for whole numbers
- Specify output class in file operations: `fread(fid, n, 'uint8=>uint8')`

### Copy-on-Write and In-Place Operations

- MATLAB uses copy-on-write; data is only copied when modified
- Enable in-place optimization by using the same variable for input and output: `x = processData(x)`
- In-place optimization only works in functions, not scripts or command line
- In-place optimization does not apply inside `try`/`catch` blocks or with global/persistent variables

### Large Data Strategies

- Use `matfile` to access parts of MAT-file variables without loading entire files (requires v7.3 format)
- Avoid using the `end` keyword with `matfile`; it loads the entire variable into memory
- Use tall arrays for data too large to fit in memory
- Use datastores for incremental processing of large file collections

---

## Parallel Computing

### When to Parallelize

- Parallelize when loop iterations are independent and each takes significant time (>100ms)
- Avoid parallelization for short iterations (<1ms) where overhead exceeds computation time
- Verify that data transfer overhead is less than computation time saved
- Profile serial code first; do not parallelize already-fast vectorized code

### parfor Best Practices

- Use `parfor` for loops with independent iterations of roughly uniform duration
- Run the outer loop in parallel to minimize per-iteration overhead
- Understand variable classifications: sliced, broadcast, and reduction variables
- Avoid large broadcast variables; use `parallel.pool.Constant` for repeated large data
- Cannot nest `parfor` inside `parfor`; cannot use `break`, `return`, or modify loop iterator

### parfeval for Asynchronous Execution

- Use `parfeval` when you need intermediate results, progress updates, or early termination
- Retrieve results with `fetchOutputs` (blocking) or `fetchNext` (as-completed order)
- Use `cancel(future)` to stop pending computations
- Chain operations with `afterEach` and `afterAll` callbacks

### backgroundPool for Responsive Apps

- Use `backgroundPool` with `parfeval` to keep apps responsive during calculations
- Background workers cannot directly update UI components; update UI in `afterEach`/`afterAll` callbacks
- Support cancellation by storing and managing Future objects
- Some functions (file I/O, Java) are not supported in thread-based workers

### Parallel Pool Management

- Use `parpool('Threads')` for lower overhead and shared memory (but limited function support)
- Use `parpool('Processes')` for full MATLAB language support and robustness
- Allocate minimum 4 GB RAM per worker
- Use `ticBytes`/`tocBytes` to measure data transfer overhead

### GPU Computing

- Use `gpuArray` for large arrays with highly parallel operations
- Keep data on GPU as long as possible; minimize CPU-GPU transfers
- Use `single` precision for better GPU performance
- Use `gather` only for final results that need to return to CPU

---

## App Designer Performance

### Startup Optimization

- Use a lightweight default tab that displays first
- Defer non-essential initialization to user-triggered callbacks
- Implement lazy loading for tree nodes and large datasets
- Distribute components across multiple tabs rather than concentrating in one

### Responsive UI Design

- Use `backgroundPool` with `parfeval` for long computations
- Update UI in `afterEach`/`afterAll` callbacks, not from worker threads
- Use `drawnow limitrate` in loops for smooth animations without blocking
- Implement cancellation support for long-running operations

### Efficient Callback Design

- Prefer `ValueChangedFcn` over `ValueChangingFcn` for heavy operations
- Share callbacks between related components to reduce code duplication
- Implement debouncing for rapidly-firing events
- Wrap callbacks in `try`/`catch` for graceful error handling

### Graphics Performance

- Reuse plot objects by updating `XData`/`YData` instead of recreating plots
- Use `animatedline` for streaming data visualization
- Disable unnecessary axes interactions and toolbars for static plots
- Set fixed axis limits (`XLimMode = 'manual'`) to prevent auto-scaling overhead

### Memory in Apps

- Store data in appropriate property types (`single`, integers when applicable)
- Clear temporary variables and cached data when no longer needed
- Implement pagination for large table displays
- Copy properties to local variables before loops to avoid repeated property access overhead

### Timer Objects

- Use `timer` objects for periodic updates with configurable `ExecutionMode` and `Period`
- Always clean up timers in `CloseRequestFcn`: `stop(timer)` then `delete(timer)`
- Use `'BusyMode', 'drop'` to skip callbacks if previous execution is still running
- Reuse graphics objects in timer callbacks instead of creating new plots

### uihtml for Custom Components

- Use `uihtml` to embed high-performance web-based visualizations (D3.js, Chart.js)
- Communicate between MATLAB and JavaScript via `Data` property and events
- All supporting files must be local; cannot link to external URLs or CDNs
- Use for custom interactive widgets, rich text editors, or third-party JavaScript libraries

---

## JIT Compiler Optimization

### JIT-Friendly Code Patterns

- Use functions instead of scripts for better JIT optimization
- Prefer local functions over nested functions when variable sharing is not needed
- Keep loop bounds constant; define bounds before the loop
- Maintain consistent data types within variables throughout execution
- Use short-circuit operators (`&&`, `||`) instead of element-wise (`&`, `|`) for scalar conditions

### Patterns That Prevent JIT Optimization

- Avoid `eval` and dynamic code execution; use direct operations or function handles instead
- Avoid `clear all` in code; clear specific variables if needed
- Avoid functions that query MATLAB state: `exist`, `whos`, `inputname`, `dbstack`
- Avoid changing the MATLAB path during execution (`cd`, `addpath`, `rmpath`)
- Minimize use of global and persistent variables

### Data Structure Performance

- Use string arrays instead of cell arrays of character vectors (2-40x faster, less memory)
- Use struct of arrays instead of array of structs for large datasets
- Access table columns (`T.columnName`) rather than individual cells (`T{i, 'column'}`) in loops
- Pre-allocate tables; never grow tables row by row in loops

### MEX Files and Code Generation

- Consider MEX files only when pure MATLAB cannot meet performance requirements
- Use MATLAB Coder for automatic C/C++ code generation with optimizations
- Generated code leverages BLAS, LAPACK, and FFTW libraries automatically
- MEX provides greatest benefit for complex iterative algorithms, less benefit for built-in operations

---

## File I/O Performance

### MAT-File Versions

- Use default v7 format for general use with mixed data types (faster for structs, cells, tables)
- Use v7.3 format (`save(..., '-v7.3')`) only when variables exceed 2 GB or partial loading is needed
- Consider disabling compression for faster saves when disk space is not a concern

### Partial Loading with matfile

- Use `matfile` objects to read/write portions of variables without loading entire files
- Requires v7.3 format for efficient partial operations
- Avoid the `end` keyword; use `size(m, 'varName')` to get dimensions
- Read large chunks at once rather than many small reads

### Text and CSV Files

- Convert frequently-used CSV files to MAT or Parquet format for faster subsequent access
- Use `detectImportOptions` and specify `VariableTypes` to avoid auto-detection overhead
- Read only needed columns with `SelectedVariableNames`
- Use `textscan` with explicit format specifiers for fastest text parsing

### Parquet Files

- Prefer Parquet over CSV for large datasets (10-12x smaller, 10-150x faster reads)
- Use row filters with `parquetread` for efficient data subsetting
- Column-oriented storage allows reading only needed columns
- Use `parquetDatastore` for processing multiple Parquet files

### Binary I/O

- Read/write entire arrays in single `fread`/`fwrite` operations
- Avoid `fseek` in loops; use the skip parameter of `fread` instead
- Binary format is approximately 10x faster than text format

### Network I/O

- Copy large files locally before processing; network access can be 10-30x slower
- Write files locally first, then copy to network location
- Avoid v7.3 format over network when possible (compression can bottleneck on single core)

### Caching Strategies

- Use `memoize` for automatic caching of expensive function calls
- Use persistent variables for manual caching within locked functions
- Clear old data before loading new data to reduce peak memory usage

---

## Quick Reference: Common Anti-Patterns

| Anti-Pattern | Better Approach |
|--------------|-----------------|
| Growing arrays in loops | Pre-allocate before loop |
| Using `find()` to get values | Use logical indexing directly |
| `repmat` for broadcasting | Use implicit expansion |
| `eval` for dynamic code | Use function handles or dynamic field access |
| `arrayfun` for speed | Use explicit loops or vectorization |
| Array of structs | Struct of arrays |
| Scalar table indexing in loops | Extract column, then index array |
| `clear all` in code | Clear specific variables |
| Global variables | Pass as function arguments |
| Nested `parfor` | Parallelize outer loop only |

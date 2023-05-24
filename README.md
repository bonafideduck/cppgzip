# cppgzip
**Experiment in converting gzip to  c++**

As a security architect, I see many issues where c uses
character buffers, and unchecked buffer manipulations.
One often suggested fix for this is to use the safer variations
like `memcpy_s()`, but this still depends upon someone placing
the proper destination size.

I want to instead wrap all buffers with 
[std::basic_string_view](https://en.cppreference.com/w/cpp/string/basic_string_view)
and use `std::` functions for the manipulations.  The end result
will be my understanding of the difficulty, the usability,
and the performance impact of such a conversion.

## Status

I'm still working on the conversion.

## Building

The initial build and setup is done with the `bootstrap` command. 
This will pull down a c-based czip twice.  The first pull is
to the *c/* directory. It is a standard dftp.gnu.org distribution.
The second to the *cpp/* directory. This is pulled from git.sv.gnu.org
and then modified to be c++. 

## Testing

I haven't gotten to that, but it will probably be simply a call
to "make test" timed several times.

## Conversion Notes

These were the changes needed to get the existing code compiling with c++.

1. Remove `register` statements.
2. Change function prototype from like this:
    ```
    void fu(i) 
      int i;
    {
        ...
    }
    ```
    to this:
    ```
    void fu(
        int i
    ) {
        ...
    }
    ```
    I did not investigate to see if there was a compiler flag to also allow this.
3. Add a space before the `O` in `"option not valid in "OPTIONS_VAR`.
4. Add various `extern "C" {...}` wrappers.
5. Add `#define alignas _Alignas` because the older libraries don't support it in C++.


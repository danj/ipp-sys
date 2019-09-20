[![crates.io](https://meritbadge.herokuapp.com/ipp-sys)](https://crates.io/crates/ipp-sys)

# ipp-sys - Bindings to Intel Integrated Performance Primitives (Intel IPP).

This directory contains several crates:

 - `ipp-sys`: toplevel convenience crate that depends on `ipp-headers-sys` and `link-*` crates
 - `ipp-headers-sys`: provide FFI headers from `ipp.h` (made with `bindgen`)
 - `ipp-sys-build-help`: helper for build.rs in the `link-*` crates
 - `link-ippcore`: link ippcore library
 - `link-ippvm`: link ippvm library
 - `link-ippcv`: link ippcv library
 - `link-ippdc`: link ippdc library
 - `link-ippcp`: link ippcp library
 - `link-ippi`: link ippi library
 - `link-ipps`: link ipps library

Typically, you can just depend on `ipp-sys`. The `link-*` crates do not provide
any rust code, but only serve to link the relevant IPP library. There are
multiple `link-*` crates because [the Cargo manifest `links` field supports only
linking only a single library per
crate](https://doc.rust-lang.org/cargo/reference/manifest.html#the-links-field-optional).

Link dependencies are done according to the [intel-integrated-performance-primitives-intel-ipp-library-dependencies-by-domain](https://software.intel.com/en-us/articles/intel-integrated-performance-primitives-intel-ipp-library-dependencies-by-domain)

## What is this crate?

This crate provides wrappers of the Intel Integrated Performance Primitives
(Intel IPP) library made with rust-bindgen.

## Version support

Use the `2017`, `2018`, or `2019` cargo feature to use IPP 2017, 2018, or
2019 respectively.

## Download IPP

Get IPP from https://software.intel.com/en-us/intel-ipp

## Usage

Compile IPP statically into your app by setting environment variable
`IPP_STATIC=1`.

You must set the `IPPROOT` environment variable at compilation time. This is
used by the `build.rs` files to find your IPP installation. Typically, this is
set using a tool provided by Intel with IPP and run as follows.

On Linux:

```
source /opt/intel/compilers_and_libraries_2019/linux/ipp/bin/ippvars.sh -arch intel64 -platform linux
```

On Mac:

```
source /opt/intel/compilers_and_libraries_2019/mac/bin/compilervars.sh -arch intel64 -platform mac
```

On Windows:

```
"C:\Program Files (x86)\IntelSWTools\compilers_and_libraries_2019\windows\ipp\bin\ippvars.bat" intel64
```

In `Cargo.toml`, include `ipp-sys` as a dependency with a feature to select
the IPP version used:

```
[dependencies]
ipp-sys = { version = "0.4", features=["2019"] }
```

Now, you can use `ipp_sys` in your crate. You might like to check that you
compiled the correct version by doing something like:

```rust
// Get the version from IPP at runtime.
let linked_version_major = unsafe{ (*ipp_sys::ippGetLibVersion()).major };
let linked_version_minor = unsafe{ (*ipp_sys::ippGetLibVersion()).minor };

// Compare the runtime major version with the compile-time major version.
assert_eq!( linked_version_major as i32, ipp_sys::IPP_VERSION_MAJOR as i32);
// And compare the minor version, too.
assert_eq!( linked_version_minor as i32, ipp_sys::IPP_VERSION_MINOR as i32);
```

## License

The `ipp-sys` Rust crates are licensed under either of

* Apache License, Version 2.0,
  (./LICENSE-APACHE or http://www.apache.org/licenses/LICENSE-2.0)
* MIT license (./LICENSE-MIT or http://opensource.org/licenses/MIT)
  at your option.

Please consult documentation for the [Intel
IPP](https://software.intel.com/en-us/intel-ipp) library for its license.

## Contribution

Unless you explicitly state otherwise, any contribution intentionally
submitted for inclusion in the work by you, as defined in the Apache-2.0
license, shall be dual licensed as above, without any additional terms or
conditions.

The canonical source code repository and issue tracker are on the [ipp-sys
github page](https://github.com/astraw/ipp-sys).

## Code of conduct

Anyone who interacts with ipp-sys in any space including but not limited to
this GitHub repository is expected to follow our [code of
conduct](https://github.com/astraw/ipp-sys/blob/master/code_of_conduct.md).

## A note about this file (the top-level `README.md`)

This file is automatically generated from the `ipp-sys` docstring by running the
following command:

    cargo readme --project-root ipp-sys --template ../README.tpl  > README.md

# MS TPM 2.0 Reference Implementation #

[![Build Status](https://travis-ci.org/Microsoft/ms-tpm-20-ref.svg?branch=master)](https://travis-ci.org/Microsoft/ms-tpm-20-ref)

This is the official TCG reference implementation of the [TPM 2.0 Specification](https://trustedcomputinggroup.org/tpm-library-specification). The project contains complete source code of the reference implementation with a Microsoft Visual Studio solution and Linux autotools build scripts.

See the definition of the `SPEC_VERSION`, `SPEC_YEAR` and `SPEC_DAY_OF_YEAR` values in the [TpmTypes.h](TPMCmd/tpm/include/TpmTypes.h) header for the exact revision/date of the TPM 2.0 specification, which the given source tree snapshot corresponds to.

## Visual Studio build ##

Before building the Visual Studio solution:

1. Uncomment and update the definitions of the following macros in the [VendorString.h](TPMCmd/tpm/include/VendorString.h) header:
 - MANUFACTURER
 - VENDOR_STRING_1
 - FIRMWARE_V1 and FIRMWARE_V2

2. Setup the underlying cryptographic library:

### OpenSSL library ###

1. Create `TPMCmd/lib` folder and place a static OpenSSL library (`libcrypto.lib`) built for the `x86` architecture there. For the `x64` architecture use the `TPMCmd/lib/x64` folder.

    The static libs can be either static libraries proper, or import libraries accompanying the corresponding DLLs. In the latter case you'll need to ensure that ther is a matching copy of the OpenSSL DLL in the standard Windows search path, so that it is available when you run the simulator executable (e.g. copy it into the same folder where `simulator.exe` is located).

    Recommended version of OpenSSL is `1.1.1d`.

2. Create `TPMCmd/OsslInclude/openssl` folder and copy there the contents of the `openssl/include/openssl` folder of the OpenSSL source tree used to build the static library used on the step 2).

3. Build the solution with either Debug or Release as the active configuration.

### Wolfcrypt library (wolfSSL) ###

1. WolfSSL is included as a submodule. Initialize and update the submodule to fetch the project and checkout the appropriate commit.

	    > git submodule init
	    > git submodule update

    The current commit will point the minimum recommended version of wolfSSL. Moving to a more recent tag or commit should also be supported but might not be tested. 

2. Build the solution with either WolfDebug or WolfRelease as the active configuration, either from inside the Visual Studio or with the following command line:

        > msbuild TPMCmd\simulator.sln /p:Configuration=WolfDebug
		
## Linux build

Follows the common `./bootstrap && ./configure && make` convention.

Note that autotools scripts require the following prerequisite packages: `autoconf-archive`, `pkg-config`, and sometimes `build-essential` and `automake`. Their absence is not automatically detected. The build also needs `gcc` and `libssl-dev` packages.

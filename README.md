libtfm
======

This is a trivial port of the Tom's Fast Math (TFM) bignum library to
the Cryptech environment.  At least for now, this just checks the
package out from GitHub, verifies that the SHA-256 digest of the
commit we're using matches a known value, and builds the library with
the options we want.

See tomsfastmath/doc/tfm.pdf for API details.

In theory, the need for most (perhaps all) of this will go away when
more of the bignum math is implemented in Verilog.  Part of the reason
for using the TFM library is that its extremely modular structure make
it easy for us to link in only the functions we need.

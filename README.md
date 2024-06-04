This is a script to repack the package cache used by pipx tests
to minimize space use.

The procedure:

1. Run `nox -e refresh_packages_cache` to create the initial cache.

2. Build wheels for all sdists, except for:

   - `shell-functools-*.tar.gz`

   Basically, unpack `.tar.gz`, `python -m build -w`, move the wheel
   back into the cache directory.

3. Remove all wheels for `pip`, `setuptools` and `wheel` (but be careful
   not to remove `setuptools_scm-*.whl`).

4. Run the script to create the shim directory, e.g.:

   `./pipx-repack.wheels /tmp/pipx/.pipx_tests/package_cache /tmp/pipx-1.6.0-test-shim`

5. Add the remaining sdists (as listed above) and repack them manually
   (ideally you can just copy the file from the earlier shim version).

6. `tar -cf pipx-1.6.0-test-shim.tar pipx-1.6.0-test-shim`

   It's a good idea to test it now (put it in `DISTDIR`, remove `.xz`
   from ebuild, update version).

7. `xz -9e pipx-1.6.0-test-shim.tar` and voilà!

Bitcoin Cash Release Process
===========================


## Before Release

1. Check configuration
    - Check features planned for the release are implemented and documented
      (or more informally, that the Release Manager agrees it is feature complete)
    - Check that finished tasks / tickets are marked as resolved

2. Verify tests passed
    - Any known issues or limitations should be documented in release notes
    - Known bugs should have tickets
    - Run `arc lint --everything` and check there is no linter error
    - Ensure that bitcoind and bitcoin-qt run with no issue on all supported platforms.
      Manually test bitcoin-qt by sending some transactions and navigating through the menus.

3. Update the documents / code which needs to be updated every release
    - Check that [release-notes.md](doc/release-notes.md) is complete, and fill in any missing items.
    - Update [bips.md](/doc/bips.md) to account for changes since the last release.
    - (major releases) Update [`BLOCK_CHAIN_SIZE`](/src/qt/intro.cpp) to the current size plus
      some overhead.
    - Regenerate manpages (run `contrib/devtools/gen-manpages.sh`, or for out-of-tree builds run
      `BUILDDIR=$PWD/build contrib/devtools/gen-manpages.sh`).
    - Update seeds as per [contrib/seeds/README.md](/contrib/seeds/README.md).
    - Update [`src/chainparams.cpp`](/src/chainparams.cpp) m_assumed_blockchain_size and m_assumed_chain_state_size with the current size plus some overhead.

4. Add git tag for release
    a. Create the tag: `git tag vM.m.r` (M = major version, m = minor version, r = revision)
    b. Push the tag to Github:
        ```
        git push <github remote> master
        git push <github remote> vM.m.r
        ```

5. Increment version number for the next release in:
    - `doc/release-notes.md` (and copy existing one to versioned `doc/release-notes/*.md`)
    - `configure.ac`
    - `CMakeLists.txt`
    - `contrib/seeds/makeseeds.py` (only after a new major release)

## Release

6. Create Gitian Builds (see [gitian-building.md](/doc/gitian-building.md))

7. Verify matching Gitian Builds, gather signatures

8. Verify IBD bith with and without `-checkpoints=0 -assumevalid=0`

9. Upload Gitian Builds to [bitcoinabc.org](https://download.bitcoinabc.org/)

10. Create a [GitHub release](https://github.com/Bitcoin-ABC/bitcoin-abc/releases):
    `contrib/release/github-release.sh -a <path to release binaries> -t <release tag> -o <file containing your Github OAuth token>`

11. Create [Ubuntu PPA packages](https://launchpad.net/~bitcoin-abc/+archive/ubuntu/ppa):
    `contrib/release/debian-packages.sh <name-or-email-for-gpg-signing>`

12. Notify maintainers of AUR and Docker images to build their packages.
    They should be given 1-day advance notice if possible.

## After Release

13. Update version number on www.bitcoinabc.org

14. Publish signed checksums (various places, e.g. blog, reddit/r/BitcoinABC)

15. Announce Release:
    - [Reddit](https://www.reddit.com/r/BitcoinABC/)
    - Twitter @Bitcoin_ABC
    - Public slack channels friendly to Bitcoin Cash announcements
      (eg. #abc-announce on BTCforks,  #hardfork on BTCchat)


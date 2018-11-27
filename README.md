# cockroachdb/libedit

This repository is [CockroachDB]'s fork of [libedit], a BSD-licensed terminal
line editing library, similar to the [GNU readline library] used by Bash.

It exists for two reasons: to provide a home for CockroachDB-specific patches,
as necessary, and to ensure that libedit submodule in
[cockroachdb/cockroach][cockroachdb] has a company-controlled remote to point
at.

It is critical that any commit in this repository that is referenced from
[cockroachdb/cockroach][cockroachdb] remains available here in perpetuity. For
every referenced commit, there must be at least one named branch or tag here
that has the commit as an ancestor, or else the commit will be garbage collected
by GitHub.

Note that our libedit upstream, <https://www.thrysoee.dk/editline/>, is an
autotool-ized port of the [libedit that lives inside of NetBSD][netbsd-libedit].
Patches that are generally applicable should be upstreamed by sending them over
the NetBSD mailing list.

**To add a patch to an existing release branch:**

  1. In your [cockroachdb/cockroach][cockroachdb] clone, change into the
     submodule's directory, `c-deps/libedit`:

     ```shell
     $ cd $GOPATH/src/github.com/cockroachdb/cockroach
     $ cd c-deps/libedit
     ```

  2. Create a feature branch named after yourself, like `pmattis/range-del`.

  3. Commit your patch.

  4. Push your changes to the named feature branch. External contributors
     will need to fork this repository and push to their fork instead.

  5. Make a commit in [cockroachdb/cockroach][cockroachdb] that updates the
     submodule ref.

     ```shell
     $ cd $GOPATH/src/github.com/cockroachdb/cockroach
     $ git add c-deps/libedit
     $ git commit -m "libedit: upgrade to..."
     ```

  6. Open a pull request against [cockroachdb/cockroach][cockroachdb] and wait
     for review.

  7. *Before* your downstream PR has merged but after it has been LGTM'd and
     passed tests, push your changes to the release branch in this repository.
     You do not need to open a PR against this repository directly; the review
     of your downstream PR suffices.

     ```shell
     $ git checkout crl-release-X.X
     $ git merge --ff-only FEATURE-BRANCH
     $ git push origin crl-release-X.X
     ```

     **Important:** don't force push! If your push is rejected, either someone
     else merged an intervening change or you didn't base your feature branch
     off the tip of the release branch. Rebase your feature branch, update your
     downstream PR with the new commit SHA, and verify tests still pass. Then
     try your push again.

  8. From the GitHub web UI, verify that the exact submodule SHA that landed in
     [cockroachdb/cockroach][cockroachdb] is on the appropriate release branch.
     If it is, delete your feature branch.

     ```shell
     $ git push -d origin FEATURE-BRANCH
     ```

**To create a new release branch:**

  1. Follow step one above.

  2. Unpack the upstream tarball on top of this repository. Upstream
     unfortunately does not provide a public source control repository.

  3. Create and push a new branch with the desired start point:

     ```shell
     $ git checkout -b crl-release-X.X upstream/RELEASE-BRANCH
     $ git push origin crl-release-X.X
     ```

  4. Protect the branch from force-pushes in the repository settings. This is
     crucial in ensuring that we don't break commit references in
     [cockroachdb/cockroach][cockroachdb]'s submodules.

  5. Follow steps three through six above.

[CockroachDB]: https://github.com/cockroachdb/cockroach
[libedit]: https://www.thrysoee.dk/editline/
[netbsd-libedit]: http://cvsweb.netbsd.org/bsdweb.cgi/src/lib/libedit/?sortby=date#dirlist
[GNU readline library]: https://tiswww.case.edu/php/chet/readline/rltop.html
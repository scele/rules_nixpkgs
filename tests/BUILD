package(default_testonly = 1)

[
    # All of these tests use the "hello" binary to see
    # whether different invocations of `nixpkgs_package`
    # produce a valid bazel repository.
    sh_test(
        name= "run-{0}".format(test),
        srcs = ["test_bin.sh"],
        args=["$(location @{0}//:bin)".format(test)],
        data = ["@{0}//:bin".format(test)],
        timeout = "short",
    ) for test in [
        "hello",
        "expr-test",
        "attribute-test",
        "expr-attribute-test",
        "nix-file-test",
        "nix-file-deps-test",
        "nixpkgs-git-repository-test",
    ]
] \
+ \
[
    # These tests use the nix package generated by ./output.nix

    # Checks whether the `:include` filegroup of `nixpkgs_package`
    # repositories works as intended
    # (that the expected number of files are inside the target)
    sh_test(
        name = "run-test-include",
        srcs = ["test_output.sh"],
        data = ["@output-filegroup-test//:include"],
        args = ["2", "$(locations @output-filegroup-test//:include)"],
        timeout = "short",
    ),

    # Checks whether specifying a manual filegroup in the
    # `nixpkgs_package` BUILD file works as well.
    sh_test(
        name = "run-test-manual-filegroup",
        srcs = ["test_output.sh"],
        data = ["@output-filegroup-manual-test//:manual-filegroup"],
        args = [
            "3",
            "$(locations @output-filegroup-manual-test//:manual-filegroup)"],
        timeout = "short",
    )
]

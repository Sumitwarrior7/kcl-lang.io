# Share Your Package to ghcr.io

This article will guide you on how to use kcl package management tool to push your kcl package to an OCI Registry for publication. The kcl package management tool uses [ghcr.io](https://ghcr.io) as the default OCI Registry, and you can change the default OCI Registry by modifying the configuration file. For information on how to modify the configuration file, see [kcl mod oci registry](https://github.com/kcl-lang/kpm/blob/main/docs/kpm_oci.md#kpm-registry)

Here is a simple step-by-step guide on how to use kcl package management tool to push your kcl package to ghcr.io.

## Step 1: Install KCL CLI

First, you need to install KCL CLI on your computer. You can follow the instructions in the [KCL CLI installation documentation](https://kcl-lang.io/docs/user_docs/getting-started/install).

## Step 2: Create a ghcr.io token

If you are using the default OCI Registry, to push a kcl package to ghcr.io, you need to create a token for authentication. You can follow the instruction.

- [Creating a ghcr.io access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-personal-access-token-classic)

## Step 3: Log in to ghcr.io

After installing KCL CLI and creating a ghcr.io token, you need to log in to ghcr.io. You can do this using the following command:

```shell
kcl registry login -u <USERNAME> -p <TOKEN> ghcr.io
```

Where `<USERNAME>` is your GitHub username, `<TOKEN>` is the token you created in step 2

For more information on how to log in to ghcr.io, see [kcl registry login](https://www.kcl-lang.io/docs/tools/cli/package-management/command-reference/login).

## Step 4: Push your kcl package

Now, you can use kpm to push your kcl package to ghcr.io.

### 1. A valid kcl package

First, you need to make sure that what you are pushing conforms to the specifications of a kcl package, i.e., it must contain valid kcl.mod and kcl.mod.lock files.

If you don't know how to get a valid kcl.mod and kcl.mod.lock, you can use the `kcl mod init` command.

```shell
# Create a new kcl package named my_package
kcl mod init my_package
```

The `kcl mod init my_package` command will create a new kcl package `my_package` for you and create the `kcl.mod` and `kcl.mod.lock` files for this package.

If you already have a directory containing kcl files `exist_kcl_package`, you can use the following command to convert it into a kcl package and create valid `kcl.mod` and `kcl.mod.lock` files for it.

```shell
# In the exist_kcl_package directory
pwd
/home/user/exist_kcl_package

# Run the `kcl mod init` command to create the `kcl.mod` and `kcl.mod.lock` files
kcl mod init
```

For more information on how to use `kcl mod init`, see [kcl mod init](https://kcl-lang.io/docs/tools/cli/package-management/command-reference/init).

### 2. Pushing the KCL Package

You can use the following command in the root directory of your `kcl` package:

```shell
# In the root directory of the exist_kcl_package package
pwd
/home/user/exist_kcl_package

# Pushing the KCL Package to Default OCI Registry
kcl mod push
```

After completing these steps, you have successfully pushed your KCL Package to the default OCI Registry.
For more information on how to use `kcl mod push`, see [kcl mod push](https://kcl-lang.io/docs/tools/cli/package-management/command-reference/push).

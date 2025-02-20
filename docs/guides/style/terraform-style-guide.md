---
category: Style Guides
excerpt: The style guide for Terraform that is used by all Gruntwork-owned code repositories that contain Terraform code.
tags: ["terraform", "code", "style"]
toc_max_heading_level: 4
---

# Terraform Style Guide

This is Gruntwork’s style guide for Terraform. It aims to help us ensure that the code we write is
clear, readable, idiomatic Terraform code. The conventions detailed in this guide are our preferences and should be
thought of as guidelines rather than hard rules.

## Starting point

We follow [the official HashiCorp style guide](https://www.terraform.io/docs/configuration/style.html) for Terraform.

All of these are enforced using `terraform fmt`. All Gruntwork Terraform repos should enforce this using pre-commit
hooks; please add if missing.

On top of the official guide, Gruntwork follows some additional conventions.

## Additional conventions

### General

#### 2 space indentations

Block contents should be indented with 2 spaces.

#### 120 column limit

We follow a 120 column line-length limit, except for description strings in `variable` and `output` blocks, where single
line strings are preferred.

#### Block Labels, Variables, and Outputs should be snake case

The label for blocks should be in snake case. E.g. `example_instance` , not `ExampleInstance` or `example-instance`.

Labels are the strings that follow block names. For example, in the following, `aws_instance` and `example_instance`
are labels for the `resource` block.

```hcl
resource "aws_instance" "example_instance" {
  # Omitted for brevity
}
```

This includes variables and outputs as well:

```hcl
variable "vpc_id" {}
output "instance_name" {}
```

#### Module folder conventions

Each module repo should have the following 3 folders:

- `modules`: Terraform modules that are designed to be consumed by users. The intention is that users should pull the
  modules in the `modules` folder in their terraform code using `module` blocks.

- `examples`: Folder that contains top level Terraform modules that provide an example of how to use the modules in the
  `modules` folder. The `examples` folder often has subfolders `for-learning-and-testing` and `for-production` that contain
  corresponding example code. See [Testing: Terratest](#testing-terratest) for more info on how these examples should be organized.

- `test`: Terratest Go files for testing the code in the repo. See [Testing: Terratest](#testing-terratest) for specific conventions around Terratest.

Additionally, each module in `modules` should be organized with the following files:

- `variables.tf`: All `variable` blocks should go in here and they specify the inputs.

- `outputs.tf`: All `output` blocks should go in here and they specify the outputs.

- `main.tf`: All other logic should be added here.

- `dependencies.tf` (optional): Any external references that are pulled in by a `data` source block should go in here.
  This allows consumers of the module to quickly scan for what resources need to already exist to deploy the module.

Any nonstandard file structure should be called out in the README (e.g., if `main.tf` is split up into multiple smaller
terraform files).

#### `variables.tf` conventions

Each variable block should always define a `description` and `type`, even if it is of the `string` type (the default), in that order. E.g.:

```hcl
variable "example" {
  description = "This is an example"
  type        = string
  default     = "example"  # NOTE: this is optional
}
```

##### Complex types

Prefer concrete objects ([object type](https://www.terraform.io/docs/configuration/types.html#structural-types)) over
free form maps. However, for particularly large objects it is useful to support optional attributes. This is currently
[not supported in terraform](https://github.com/hashicorp/terraform/issues/22449), so workaround by using `any` type.

When using `any` type, always use comments to describe the supported attributes.
[Example](https://github.com/gruntwork-io/module-security/blob/da69690/modules/kms-master-key/variables.tf#L10).

#### `outputs.tf` conventions

Each output block should always define a `description`, before the `value`:

```hcl
output "greeting" {
  description = "This is a greeting for everyone."
  value       = "hello world!"
}
```

#### `main.tf` conventions

`main.tf` should (loosely) be organized by sections that correspond to components. There is no standard on grouping, but
as a rule of thumb each section should be focused on a specific component of the module. For example, an ECS service
module may consist of the following sections:

- The ECS service resource, and any locals logic for setting up the attributes of the resource.
- The ECS task definition resource, and any locals and template logic for setting up the attributes of the resource
  (e.g. the container definition).
- Any resources related to configuring ELBs to forward traffic to the ECS service (e.g., listeners and target groups).
- Any resources related to configuring IAM permissions for the ECS service.
- Any resources related to configuring network access (e.g., security group rules) for the ECS service.

There is no standard on ordering the sections, but as a rule of thumb the following sections should be placed first, in order:

- Version constraints for the module
- Provider blocks, if needed.
- The main component of the module (e.g., the `aws_ecs_service` resource for the ECS service module).
- All other sections.
- Any `data` blocks (at the bottom).

#### Conditionals

Use `()` to break up conditionals across multiple lines.

Examples:

```hcl
locals {
  elb_id = (
    var.elb_already_exists
    ? var.elb_id
    : module.elb.elb_id
  )

  excluded_child_account_ids = (
    var.config_create_account_rules
    ? []
    : [
      for account_name, account in module.organization.child_accounts
      : account.id if lookup(lookup(var.child_accounts, account_name, {}), "enable_config_rules", false) == false
    ]
  )
}
```

### Comments

This section lists the Gruntwork conventions around comments in Terraform code.

#### Prefer `#` over `//`

Use `#` for comment strings, not `//` or `/**/`.

#### Prefer `# -` over `# ~`

Delimit section header comment blocks with `# ----` instead of `# \~~~~`.

#### `variables.tf`

`variables.tf` files should clearly indicate required environment variables, and separate out required variables from
optional variables (with defaults) using block comments.

Example:

```hcl
## ---------------------------------------------------------------------------------------------------------------------
## ENVIRONMENT VARIABLES
## Define these secrets as environment variables
## ---------------------------------------------------------------------------------------------------------------------

## TF_VAR_master_password

## ---------------------------------------------------------------------------------------------------------------------
## MODULE PARAMETERS
## These variables are expected to be passed in by the operator
## ---------------------------------------------------------------------------------------------------------------------

variable "required_var" {
  description = "This variable must be set in order to create the resource."
  type        = string
}

## ---------------------------------------------------------------------------------------------------------------------
## OPTIONAL PARAMETERS
## These variables have defaults and may be overridden
## ---------------------------------------------------------------------------------------------------------------------

variable "optional_var" {
  description = "This variable has a sensible default so it is not necessary to set it explicitly for this module to work."
  type        = string
  default     = "Hello world"
}
```

#### `main.tf`

##### Section comments

Each section (as described in [ conventions](#maintf-conventions)) of `main.tf` should have block comments describing the component
managed in the section.

Example:

```hcl
## ---------------------------------------------------------------------------------------------------------------------
## ONE LINE SUMMARY DESCRIBING WHAT IS BEING MANAGED IN THIS SECTION IN ALL CAPS
## The rest of the comments should be in standard casing. This section should contain an overall description of the
## component that is being managed, and highlight any unconventional workarounds or configurations that are in place.
## ---------------------------------------------------------------------------------------------------------------------
```

### Testing: Terratest

Gruntwork uses [Terratest](https://terratest.gruntwork.io) to write tests for Terraform modules. Terratest is a Go
library that provides patterns and helper functions for testing infrastructure code.

#### Terratest best practices

Follow all the best practices listed in [Terratest best practices](https://terratest.gruntwork.io/docs/#testing-best-practices).

The rest of the items below are additional conventions on top of the documented best practices that Gruntwork follows
when writing tests using Terratest for terraform modules.

#### Code formatting

Terratest is a Go library, so each test will be written in Go. All Go source files should be formatted using `goimports`
and `go fmt`.

#### `examples` and `tests`

In many cases the individual modules in the `modules` folder are narrowly focused to a specific subset of the overall
infrastructure. This means that in many cases you will need to provide dependent resources externally to the module in
order to actually deploy them. The Terraform modules in the `examples` folder serves this purpose, specifying test
resources that are injected as dependencies to the modules.

As such, the tests should be written against the `examples` folder, as opposed to the `modules` folder directly. In
other words:

- Every module in `modules` should have a corresponding example module in `examples` that calls it. (NOTE: you can have
  a single example call multiple modules).
- Every example should have at least one test that calls it.
- Tests should not directly call modules in the `modules` folder. Always go through the `examples`.

#### Parallel

Every test should have the `t.Parallel` call in the test function unless there is a specific need to run tests serially,
e.g. manipulating process global resources, like environment variables. This is so that tests run as quickly as possible.

To facilitate this, every reference to a terraform example should use
[test_structure.CopyTerraformFolderToTemp](https://pkg.go.dev/github.com/gruntwork-io/terratest/modules/test-structure#CopyTerraformFolderToTemp)
to create a copy of the example module in a temp directory. Then as the test runs, any stateful changes to the example
module directory are isolated across tests, so that there’s no conflict on parallel runs.

#### Use TestStages for faster development

Use [test stages](https://terratest.gruntwork.io/docs/testing-best-practices/iterating-locally-using-test-stages/)
in the test code, unless you only have 1 or 2 steps in the test code (e.g. a `plan` verification test).

It’s very tedious to build and deploy resources over and over when you only want to tweak a validation step. TestStages
make it flexible and convenient to skip stages, making development much faster.

For each test stage you introduce, add a commented out series of `os.Setenv` calls to make it convenient to skip stages
as you develop.

```go
func TestJenkins(t *testing.T) {
        t.Parallel()

        // Uncomment the items below to skip certain parts of the test
        //os.Setenv("SKIP_build_ami", "true")
        //os.Setenv("SKIP_deploy_terraform", "true")
        //os.Setenv("SKIP_validate", "true")
        //os.Setenv("SKIP_cleanup", "true")
        //os.Setenv("SKIP_cleanup_ami", "true")

        defer test_structure.RunTestStage(t, "cleanup_ami", deleteAMI)
        defer test_structure.RunTestStage(t, "cleanup", destroyInfra)
        test_structure.RunTestStage(t, "build_ami", buildAMI)
        test_structure.RunTestStage(t, "deploy_terraform", deployInfra)
        test_structure.RunTestStage(t, "validate", validateInfra)
}
```

To use the stages, here’s an example workflow. The first time you run the test, you’ll want to skip only the `cleanup`
stages:

```go
// Uncomment the items below to skip certain parts of the test
//os.Setenv("SKIP_build_ami", "true")
//os.Setenv("SKIP_deploy_terraform", "true")
//os.Setenv("SKIP_validate", "true")
os.Setenv("SKIP_cleanup", "true")
os.Setenv("SKIP_cleanup_ami", "true")
```

Let’s say building and deploying were successful, but validation failed. Since resources were not cleaned up, we can run
only the `validate` stage. We skip the resource and time intensive `build` and `deploy` stages, and also continue to
skip the `cleanup` stages.:

```go
// Uncomment the items below to skip certain parts of the test
os.Setenv("SKIP_build_ami", "true")
os.Setenv("SKIP_deploy_terraform", "true")
//os.Setenv("SKIP_validate", "true")
os.Setenv("SKIP_cleanup", "true")
os.Setenv("SKIP_cleanup_ami", "true")
```

Once you’ve established that validation works, you can then run only the `cleanup` stages as below. Your workflow may vary.

```go
// Uncomment the items below to skip certain parts of the test
os.Setenv("SKIP_build_ami", "true")
os.Setenv("SKIP_deploy_terraform", "true")
os.Setenv("SKIP_validate", "true")
//os.Setenv("SKIP_cleanup", "true")
//os.Setenv("SKIP_cleanup_ami", "true")
```

When committing the final version of the test, all should be commented out so all stages run.

```go
// Uncomment the items below to skip certain parts of the test
//os.Setenv("SKIP_build_ami", "true")
//os.Setenv("SKIP_deploy_terraform", "true")
//os.Setenv("SKIP_validate", "true")
//os.Setenv("SKIP_cleanup", "true")
//os.Setenv("SKIP_cleanup_ami", "true")
```

#### Setup and Teardown pattern

In some cases you will want to write a group of tests that use a common resource, such as a Docker image or VPC. In this
case, you will want to setup the common resource once, run a bunch of tests, and then teardown the resource. To achieve
this, you can follow [the subtest pattern](https://blog.golang.org/subtests) of Go.

Use [table driven tests](https://dave.cheney.net/2019/05/07/prefer-table-driven-tests) where possible to make
the subtest routines maintainable. Briefly, this means that you group your test cases using a test struct that reflects
the unique parameters of the test cases. Then you can conveniently loop over the test cases in parallel, taking
advantage of uniformity and speed.

Note that the subtest pattern has gotchas when running tests in parallel:

- The main test function will not wait for the subtest to run if it uses `t.Parallel`. To avoid this, you need to wrap
  the parallel subtests in a synchronous, blocking subtest. In the example below, the `group` subtest is synchronous (no
  call to `t.Parallel`) and thus the main function will wait until that test finishes. The `group` test does not finish
  until all the subtests it spawns are finished, even if they are non-blocking and parallel, and thus the `tearDownVPC`
  call does not happen until all subtests are done.

- If you are using table driven tests, the range variable will be updated to the next iteration before it is used within
  the subtest. That is, in the example below, if we did not have the `testCase := testCase` line in the range block, the
  `testCase` reference used in the subtest after the `t.Parallel` call will correspond to the last `testCase` in the
  `testCases` list. To avoid this, we create a new variable in the scope of the range block so that it does not get
  updated during the loop.

Example:

```go
func TestECS(t *testing.T) {
    t.Parallel()

    defer tearDownVPC()
    deployVPC()

    // Wrap the parallel tests in a synchronous test group to ensure that the main test function (the one calling
    // `tearDownVPC` and `deployVPC`) waits until all the subtests are done before running the deferred function.
    t.Run("group", func(t *testing.T) {
        for _, testCase := range testCases {
            // To avoid the range variable from getting updated in the parallel tests, we bind a new name that is within
            // the scope of the for block.
            testCase := testCase
            t.Run(testCase.name, func(t *testing.T) {
                t.Parallel()
                testCase.testCode()
            })
        }
    })
}
```


<!-- ##DOCS-SOURCER-START
{"sourcePlugin":"local-copier","hash":"ebf33215ca11bf62867a076882927304"}
##DOCS-SOURCER-END -->

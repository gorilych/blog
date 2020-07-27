---
title:  "Pulumi for Terraform users"
date:   2020-07-26 00:52:28 +0700
hidden: true
tags: pulumi terraform
---
This article is intended for engineers familiar with IaC, modern IaC tools, and related terminology. As title suggests, it might be interesting for Terraform users - I'll try to compare Terraform and Pulumi. The article is based on personal experience and may appear opinionated.

# IaC approach

<figure>
  <img src="{{ site.url }}{{ site.baseurl }}/assets/pulumi-for-tf-users/iac.svg" alt="" object-fit="cover">
  <figcaption>IaC workflow</figcaption>
</figure> 

I am sure Terraform users have correct understanding of what IaC is and is not, but for the sake of article consistency, let me repeat it here.

Infrastructure as Code aims to provide a reliable, repeatable, and automatic way to create, adjust, or destroy infrastructure components or resources. Usually these are networking, compute, or storage resources in public clouds like AWS.

The major concept and the main difference with manual administration is that you describe infrastructure in a declarative way. **Code** part means that this description is machine-readable and a machine executes the code to operate infrastructure.

Another pillar in IaC is a concept of **State**  - description of operated resources with their actual conditions and characteristics. A state can be either stored somewhere or calculated each time (in ansible, f.e.). IaC requires state in its workflow because it should work with the same set of resources during infrastructure lifetime.

Usually we use **specific tool for IaC** (CloudFormation, Terraform, Pulumi, Ansible, etc). I would say it's another concept behind IaC, implicit though. Of course, you can also manage resources programmatically with cloud API using regular scripts. But with these requirements of having state and describe infrastructure declaratively you'll end up with writing your own IaC tool.

In short, IaC workflow is to write some code and use specific IaC tool to execute it, producing state and cloud resources as a result.

# What is Pulumi

[Pulumi](https://www.pulumi.com/docs/) is a modern IaC tool that essentially differs from others by allowing the use of real programming languages. This doesn't mean it uses an imperative approach though because a code describes a desired state and not API calls or procedures to perform.

Currently supported [language runtimes](https://www.pulumi.com/docs/intro/languages/) are Node.js, Python, .NET Core, and Go.

Let's outline the main terms of Pulumi's architecture (described in greater detail in [Architecture & Concepts](https://www.pulumi.com/docs/intro/concepts/)).

* [Project](https://www.pulumi.com/docs/intro/concepts/project/) - the code itself which describes cloud resources - directory containing collection of program files (`*.go`, `*.ts` etc) and a metadata file `Pulumi.yaml`
* [Stack](https://www.pulumi.com/docs/intro/concepts/stack/) - instance of a _Project_, collection of cloud resources deployed with particular _Configuration_ (see below). F.e., you may have separate stacks for production and staging environments.
* _Stack_ [Configuration](https://www.pulumi.com/docs/intro/concepts/config/) - YAML file with a set of values which are used to modify operation flow. This adds flexibility so you can re-use the same _Project_ for environments with distinct DNS names, different compute power and storage capacity and so on. Configuration file is named after a _Stack_ as `Pulumi.<stack-name>.yaml`.
* _Stack_ [State](https://www.pulumi.com/docs/intro/concepts/state/) - state of cloud resources in a _Stack_.
* _Stack_ [Output](https://www.pulumi.com/docs/intro/concepts/programming-model/#stack-outputs) - calculated values which are known after deployment, f.e. EC2 instance id, generated password for RDS, etc

These terms are close but not always interchangeable with Terraform ones. Pulumi's project is close to Terraform's root module. Terraform's workspace is a close concept for Pulumi's stack. Output and configuration can be considered the same.

Likewise in Terraform, there is a specific binary `pulumi` to execute Pulumi programs. `pulumi` is used to operate state backends (a place where state files are stored) and stacks.

# How Pulumi works

When you run a pulumi program it is executed with `pulumi` binary. Pulumi engine computes a desired state from resources registered by the program and then creates, updates, or deletes resources as needed.

Pulumi consists of a language host that is responsible for executing the code and deployment engine which is responsible for interacting with cloud API.

For the greater details refer to these articles:
* <https://www.pulumi.com/docs/intro/concepts/how-pulumi-works/>
* <https://www.pulumi.com/docs/intro/concepts/programming-model/>

# Programming model
# Code example
# Code reuse
# CLI usage
# Pulumi vs Terraform
# Conclusion
# References
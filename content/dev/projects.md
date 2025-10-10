---
title: "Projects"
date: 2025-10-10T10:22:21+01:00
draft: false
---

Here's some of the open-source projects I have worked on over the years:

# [`github.com/temporalio/terraform-provider-temporalcloud`](https://github.com/temporalio/terraform-provider-temporalcloud)

If you manage [Temporal Cloud](https://temporal.io/cloud) via Terraform, you're using my code!

I specifically added the following:

- the `temporalcloud_user` resource
- the `temporalcloud_namespace` data source
- the `endpoints` property to the `temporalcloud_namespace` resource
- the `temporalcloud_metrics_endpoint` resource
- the `allowed_account_id` provider option

# [`github.com/ConductorOne/baton-temporalcloud`](https://github.com/ConductorOne/baton-temporalcloud)

If you use [Baton](https://github.com/conductorone/baton) or [ConductorOne](https://www.conductorone.com) to manage access to Temporal Cloud, you're using my code!

I wrote the vast majority of the codebase and try to add on new features when I can—but it's a [full-fledged part of ConductorOne's product](https://www.conductorone.com/docs/baton/temporal-cloud/) and not my baby anymore!
How they grow up so fast!

# [`github.com/ejfrick/cuts`](https://github.com/ejfrick/cuts)

A lightweight Go library using generics that provides some utilities for working with slices. (That's why it _cuts_!)

I find myself using `Dedupe` and `DedupeFunc` a _lot_. I am also quite proud of `SnapTo`, and the `stringsnap` submodule—although wow are submodules a pain in the arse.

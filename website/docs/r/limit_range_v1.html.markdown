---
layout: "kubernetes"
page_title: "Kubernetes: kubernetes_limit_range_v1"
description: |-
  Limit Range sets resource usage limits (e.g. memory, cpu, storage) for supported kinds of resources in a namespace.
---

# kubernetes_limit_range_v1

Limit Range sets resource usage limits (e.g. memory, cpu, storage) for supported kinds of resources in a namespace.

Read more in [the official docs](https://kubernetes.io/docs/concepts/policy/limit-range/).


## Example Usage

```hcl
resource "kubernetes_limit_range_v1" "example" {
  metadata {
    name = "terraform-example"
  }
  spec {
    limit {
      type = "Pod"
      max = {
        cpu    = "200m"
        memory = "1024Mi"
      }
    }
    limit {
      type = "PersistentVolumeClaim"
      min = {
        storage = "24M"
      }
    }
    limit {
      type = "Container"
      default = {
        cpu    = "50m"
        memory = "24Mi"
      }
    }
  }
}
```

## Argument Reference

The following arguments are supported:

* `metadata` - (Required) Standard limit range's metadata. For more info see [Kubernetes reference](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api-conventions.md#metadata)
* `spec` - (Optional) Spec defines the limits enforced. For more info see [Kubernetes reference](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api-conventions.md#spec-and-status)

## Nested Blocks

### `spec`

#### Arguments

* `limit` - (Optional) The list of limits that are enforced.

### `limit`

#### Arguments

* `default` - (Optional) Default resource requirement limit value by resource name if resource limit is omitted.
* `default_request` - (Optional) The default resource requirement request value by resource name if resource request is omitted.
* `max` - (Optional) Max usage constraints on this kind by resource name.
* `max_limit_request_ratio` - (Optional) The named resource must have a request and limit that are both non-zero where limit divided by request is less than or equal to the enumerated value; this represents the max burst for the named resource.
* `min` - (Optional) Min usage constraints on this kind by resource name.
* `type` - (Optional) Type of resource that this limit applies to. e.g. `Pod`, `Container` or `PersistentVolumeClaim`

### `metadata`

#### Arguments

* `annotations` - (Optional) An unstructured key value map stored with the limit range that may be used to store arbitrary metadata. 

~> By default, the provider ignores any annotations whose key names end with *kubernetes.io*. This is necessary because such annotations can be mutated by server-side components and consequently cause a perpetual diff in the Terraform plan output. If you explicitly specify any such annotations in the configuration template then Terraform will consider these as normal resource attributes and manage them as expected (while still avoiding the perpetual diff problem). For more info info see [Kubernetes reference](http://kubernetes.io/docs/user-guide/annotations)

* `generate_name` - (Optional) Prefix, used by the server, to generate a unique name ONLY IF the `name` field has not been provided. This value will also be combined with a unique suffix. For more info see [Kubernetes reference](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api-conventions.md#idempotency)
* `labels` - (Optional) Map of string keys and values that can be used to organize and categorize (scope and select) the limit range. May match selectors of replication controllers and services.

~> By default, the provider ignores any labels whose key names end with *kubernetes.io*. This is necessary because such labels can be mutated by server-side components and consequently cause a perpetual diff in the Terraform plan output. If you explicitly specify any such labels in the configuration template then Terraform will consider these as normal resource attributes and manage them as expected (while still avoiding the perpetual diff problem). For more info info see [Kubernetes reference](http://kubernetes.io/docs/user-guide/labels)

* `name` - (Optional) Name of the limit range, must be unique. Cannot be updated. For more info see [Kubernetes reference](http://kubernetes.io/docs/user-guide/identifiers#names)
* `namespace` - (Optional) Namespace defines the space within which name of the limit range must be unique.

#### Attributes

* `generation` - A sequence number representing a specific generation of the desired state.
* `resource_version` - An opaque value that represents the internal version of this limit range that can be used by clients to determine when limit range has changed. For more info see [Kubernetes reference](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api-conventions.md#concurrency-control-and-consistency)
* `uid` - The unique in time and space value for this limit range. For more info see [Kubernetes reference](http://kubernetes.io/docs/user-guide/identifiers#uids)

## Import

Limit Range can be imported using its namespace and name, e.g.

```
$ terraform import kubernetes_limit_range_v1.example default/terraform-example
```

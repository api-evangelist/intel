# Intel GraphQL Schema

## Overview

This conceptual GraphQL schema models the Intel developer ecosystem spanning processor architectures, heterogeneous computing via oneAPI, AI/ML acceleration with Gaudi and OpenVINO, Intel Developer Cloud, memory and storage technologies, networking infrastructure, and platform-level specifications. The schema provides a unified graph representation of Intel's hardware and software portfolio for tooling, documentation, and developer portal use cases.

## Schema Source

Derived from public Intel developer documentation:

- Intel Developer Portal: https://developer.intel.com/
- Intel oneAPI Overview: https://www.intel.com/content/www/us/en/developer/tools/oneapi/overview.html
- Intel Developer Cloud: https://www.intel.com/content/www/us/en/developer/tools/devcloud/overview.html
- Intel OpenVINO Toolkit: https://docs.openvino.ai/
- Intel ARK (Product Database): https://ark.intel.com/
- Intel Trust Authority API: https://docs.trustauthority.intel.com/
- Intel GitHub Organization: https://github.com/intel

## Types

### Processor Architecture

| Type | Description |
|---|---|
| `Processor` | Individual Intel processor product with full specification linkage |
| `ProcessorFamily` | Grouping of related processors (e.g., Xeon Scalable, Core Ultra) |
| `CoreArchitecture` | Microarchitecture details including codename and process node |
| `CPUSpec` | Consolidated CPU specification including cores, cache, memory support |
| `CPUGeneration` | Intel CPU generation (e.g., 12th Gen, 13th Gen, Sapphire Rapids) |
| `Cores` | Performance and efficiency core breakdown |
| `Threads` | Thread count and per-core threading |
| `ClockSpeed` | Base and boost clock speeds with unit |
| `TDP` | Thermal design power with configurable range |
| `Cache` | L1/L2/L3 cache size and type |
| `InstructionSet` | Supported instruction set extensions |
| `SSEVersion` | Streaming SIMD Extensions version details |
| `AVXVersion` | Advanced Vector Extensions version with VNNI support flag |

### AI Accelerators

| Type | Description |
|---|---|
| `AI` | Top-level AI capability node linking models, accelerators, and frameworks |
| `Gaudi` | Intel Gaudi AI accelerator specification including HBM and tensor processor cores |
| `OpenVINO` | OpenVINO runtime with supported frameworks, devices, and model catalog |
| `Model` | Individual AI model including task, framework, and precision |
| `ModelOptimization` | Applied optimization result with accuracy and speedup metrics |
| `Quantization` | Model quantization run with precision, accuracy drop, and throughput gain |
| `Inference` | Inference execution record with device, batch size, and performance metrics |
| `Throughput` | Inference throughput value with unit and percentile |
| `Latency` | Inference latency with mean, p50, and p99 breakdown |
| `InferenceResult` | Complete inference job result with status and timing |

### oneAPI Developer Tools

| Type | Description |
|---|---|
| `oneAPI` | Top-level oneAPI version node linking toolkits, libraries, and DPCPP |
| `DPCPP` | Intel Data Parallel C++ compiler with extensions and flags |
| `VTune` | Intel VTune Profiler with analysis types and GPU profiling support |
| `Inspector` | Intel Inspector memory and thread analysis tool |
| `Advisor` | Intel Advisor vectorization and roofline analysis tool |
| `XTU` | Intel Extreme Tuning Utility for overclocking and power management |
| `Toolkit` | oneAPI toolkit package with components and OS support |
| `Library` | Individual oneAPI library categorized by domain |
| `SDK` | Intel SDK with language and platform support |

### Cloud and Containers

| Type | Description |
|---|---|
| `DevCloud` | Intel Developer Cloud service node with available instances and toolkits |
| `CloudInstance` | Provisioned cloud compute instance with processor, GPU, or Gaudi |
| `Container` | Pre-built container image with bundled toolkits |

### Memory Technologies

| Type | Description |
|---|---|
| `Memory` | Generic memory specification with type, speed, and channels |
| `DDR5` | DDR5 memory specification with RDIMM and LRDIMM support flags |
| `HBM` | High Bandwidth Memory specification with capacity and bandwidth |

### Storage Technologies

| Type | Description |
|---|---|
| `StorageSpec` | Generic storage specification with type, capacity, and interface |
| `NAND` | NAND flash specification with cell type and layer count |
| `Optane` | Intel Optane persistent memory and storage specification |

### Networking

| Type | Description |
|---|---|
| `Ethernet` | Intel Ethernet adapter with speed, RDMA, and SR-IOV support |
| `InfiniBand` | High-performance fabric adapter specification |
| `CXL` | Compute Express Link specification with version and bandwidth |

### Platform Specifications

| Type | Description |
|---|---|
| `Platform` | Generic platform node linking processors, memory, storage, and networking |
| `ServerPlatform` | Server platform with socket count, memory capacity, and CXL support |
| `WorkstationPlatform` | Workstation platform with GPU slot and Thunderbolt port details |
| `EmbeddedPlatform` | Embedded platform with operating temperature and interface list |
| `MobileSpec` | Mobile platform specification with battery life and Thunderbolt version |
| `IoTSpec` | IoT device specification with connectivity and security features |
| `EdgeComputing` | Edge computing node linking platform, AI acceleration, and latency |

### Developer and Authentication

| Type | Description |
|---|---|
| `Developer` | Intel developer account with projects and access tokens |
| `AccessToken` | API access token with scopes and expiry |
| `ProjectKey` | Project-scoped API key with secret |
| `Project` | Developer project grouping cloud instances and keys |

### Performance and Benchmarks

| Type | Description |
|---|---|
| `BenchmarkResult` | Benchmark run result with score, workload, and configuration |
| `FPGA` | FPGA specification with logic elements, DSP blocks, and toolchain |
| `GPUSpec` | Intel GPU specification with execution units, tensor cores, and VRAM |

## Operations

### Queries

```graphql
# Retrieve a specific processor by ID
query GetProcessor {
  processor(id: "intel-core-ultra-9-285k") {
    name
    modelNumber
    generation { name number }
    architecture { codename processNode }
    spec {
      cores { total performance efficiency }
      baseClockSpeed { value unit }
      tdp { value unit }
      l3Cache { size unit }
    }
    tdp { value unit }
    instructionSets { name category }
    avxVersions { version registersWidth vnniSupport }
  }
}

# List Gaudi accelerators available in Developer Cloud
query GaudiAccelerators {
  gaudiAccelerators {
    name
    generation
    hbmCapacity { capacityGB bandwidthGBps }
    tensorProcessorCores
    supportedFrameworks
    cloudAvailable
  }
}

# Find AI models optimized for a given task
query FindModels {
  aiModels(filter: { task: "object-detection", framework: "OpenVINO" }) {
    id
    name
    precision
    optimizations {
      technique
      speedup
      appliedQuantization { precision throughputGain }
    }
  }
}

# List available Developer Cloud instances with Gaudi
query DevCloudGaudiInstances {
  devCloudInstances(filter: { hasGaudi: true, status: RUNNING }) {
    name
    instanceType
    gaudi { name generation hbmCapacity { capacityGB } }
    region
    costPerHour
  }
}

# Get oneAPI toolkits
query OneAPIToolkits {
  oneAPIToolkits {
    name
    version
    targetUseCase
    components {
      name
      category
      languages
    }
  }
}

# Query memory specifications for a platform
query ServerMemory {
  platform(id: "intel-xeon-scalable-platform") {
    name
    memory {
      type
      speed { value unit }
      channels
      ecc
    }
    processors {
      name
      spec { memoryChannels maxMemory }
    }
  }
}
```

### Mutations

```graphql
# Create a developer project
mutation CreateProject {
  createProject(input: { name: "AI Inference Pipeline", description: "Edge inference with OpenVINO" }) {
    id
    name
    createdAt
  }
}

# Provision a Developer Cloud instance
mutation ProvisionInstance {
  provisionCloudInstance(input: {
    instanceType: "gaudi2.large"
    region: "us-east"
    projectId: "proj-abc123"
  }) {
    id
    instanceType
    gaudi { name generation }
    status
  }
}

# Submit an inference job
mutation RunInference {
  submitInferenceJob(input: {
    modelId: "resnet50-v1-fp16"
    device: "CPU"
    batchSize: 32
    precision: "FP16"
  }) {
    id
    jobId
    status
    inference {
      throughput { value unit }
      latency { mean p99 unit }
    }
  }
}

# Quantize a model
mutation QuantizeModel {
  quantizeModel(input: {
    modelId: "resnet50-v1-fp32"
    precision: INT8
    calibrationDataset: "imagenet-val"
  }) {
    id
    precision
    accuracyDrop
    throughputGain
    latencyReduction
    status
  }
}

# Generate an access token
mutation GenerateToken {
  generateAccessToken(projectId: "proj-abc123") {
    id
    name
    token
    scopes
    expiresAt
  }
}
```

### Subscriptions

```graphql
# Monitor inference job progress
subscription InferenceStatus {
  inferenceJobStatus(jobId: "job-xyz789") {
    jobId
    status
    inference {
      throughput { value unit }
      latency { mean unit }
    }
    completedAt
    errorMessage
  }
}

# Monitor cloud instance provisioning
subscription InstanceReady {
  cloudInstanceStatus(instanceId: "inst-123") {
    id
    status
    gaudi { name }
    createdAt
  }
}
```

## Enumerations

| Enum | Values |
|---|---|
| `ProcessorSegment` | `DESKTOP`, `LAPTOP`, `SERVER`, `WORKSTATION`, `EMBEDDED`, `IOT`, `MOBILE` |
| `PlatformSegment` | `SERVER`, `WORKSTATION`, `DESKTOP`, `MOBILE`, `EMBEDDED`, `EDGE`, `IOT` |
| `MemoryType` | `DDR4`, `DDR5`, `LPDDR5`, `HBM2`, `HBM2E`, `HBM3`, `OPTANE` |
| `StorageType` | `NAND_SSD`, `OPTANE_SSD`, `NVMe`, `SATA`, `HDD` |
| `QuantizationPrecision` | `FP32`, `FP16`, `BF16`, `INT8`, `INT4` |
| `JobStatus` | `QUEUED`, `RUNNING`, `COMPLETED`, `FAILED`, `CANCELLED` |
| `InstanceStatus` | `PROVISIONING`, `RUNNING`, `STOPPED`, `TERMINATED`, `ERROR` |
| `LibraryCategory` | `MATH`, `DEEP_LEARNING`, `SIGNAL_PROCESSING`, `COMPUTER_VISION`, `DATA_ANALYTICS`, `PARALLEL_COMPUTING`, `CRYPTOGRAPHY` |
| `NANDCellType` | `SLC`, `MLC`, `TLC`, `QLC` |
| `InstructionSetCategory` | `GENERAL`, `SIMD`, `VECTOR`, `AI`, `CRYPTO`, `MEMORY` |

## Related APIs

- **Intel Trust Authority API** — REST attestation service for confidential computing workloads. OpenAPI spec at `openapi/intel-trust-authority-api-openapi.yml`.
- **Intel oneAPI** — Heterogeneous programming toolkit. OpenAPI spec at `openapi/intel-oneapi-openapi.yml`.

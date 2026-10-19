
# LLM Inference with vLLM & Kubernetes Lab

A hands-on learning project exploring LLM inference, performance benchmarking, and Kubernetes orchestration using vLLM, Qwen2.5-1.5B-Instruct, Google Colab, Docker Desktop, and Kind.

## Overview

This project explores two separate environments:

1. **LLM inference and benchmarking:** Served Qwen2.5-1.5B-Instruct using vLLM on a Google Colab T4 GPU runtime.
2. **Kubernetes orchestration:** Practised Deployments, replicas, Services, port forwarding, and health probes using a local Kind cluster on Windows with Docker Desktop and NGINX.

The vLLM inference experiment and Kubernetes lab were performed separately. vLLM was not deployed inside the local Kubernetes cluster.

## Architecture

### 1. LLM Inference

```text
Google Colab (T4 GPU)
        |
        v
Qwen2.5-1.5B-Instruct
        |
        v
      vLLM
        |
        v
OpenAI-Compatible API
        |
        v
Inference Responses
```

### 2. Kubernetes Orchestration

```text
Windows Host
    |
    v
Docker Desktop
    |
    v
Kind Kubernetes Cluster
    |
    v
Deployment (web-demo)
    |
    v
ReplicaSet
    |
    +----------+
    |          |
    v          v
  Pod 1      Pod 2
  NGINX      NGINX
    \          /
     \        /
      v      v
    ClusterIP Service
           |
           v
    kubectl port-forward
           |
           v
    Browser (localhost:8080)
```

## Technology Stack

- Python
- vLLM
- Qwen2.5-1.5B-Instruct
- Google Colab
- NVIDIA T4 GPU
- Docker Desktop
- Kubernetes
- Kind
- kubectl
- NGINX
- YAML

## LLM Inference Experiments

The inference experiment used vLLM to serve Qwen2.5-1.5B-Instruct through an OpenAI-compatible API.

Experiments included:

- Model initialization and health checks
- Chat completion requests
- Streaming responses
- Sequential inference requests
- Concurrent request handling
- Throughput and latency measurements

### Concurrency Benchmark

Measured results from one trial per concurrency level:

| Concurrent Requests | Batch Wall Time | Mean Request Latency | Aggregate Throughput |
|---:|---:|---:|---:|
| 1 | 0.794 s | 0.791 s | 63.01 tokens/s |
| 2 | 0.776 s | 0.774 s | 128.89 tokens/s |
| 4 | 0.807 s | 0.800 s | 247.87 tokens/s |

Throughput increased from 63.01 to 247.87 tokens/s as concurrency increased from one to four requests in this experiment.

**Note:** These are single-trial measurements, not statistically validated performance guarantees. Results depend on hardware, prompt length, output length, model configuration, and runtime conditions.

## Kubernetes Lab

The local Kubernetes lab used NGINX to practise orchestration concepts without requiring a cloud cluster or local GPU.

### Tasks Completed

- Created a local Kind Kubernetes cluster.
- Verified node and system Pod health.
- Created a Deployment with one replica.
- Scaled the Deployment to two replicas.
- Created a ClusterIP Service.
- Verified Service EndpointSlices.
- Accessed NGINX through port forwarding.
- Configured HTTP readiness and liveness probes using YAML.
- Validated the Deployment rollout.

### Health Probes

**Readiness probe**
- HTTP GET `/`
- Checks whether the Pod should receive Service traffic.
- Configured to run every five seconds.

**Liveness probe**
- HTTP GET `/`
- Checks whether the application is responding.
- Configured with a ten-second initial delay and ten-second probe interval.

The Deployment finished with two available replicas and zero reported restarts in the captured output.

## Key Learnings

- How vLLM serves LLM inference requests.
- The difference between prefill and token decoding.
- How concurrency affects aggregate throughput.
- How Kubernetes Deployments and ReplicaSets manage replicas.
- How Services discover and route traffic to ready Pods.
- How readiness and liveness probes support application health management.
- How declarative YAML describes Kubernetes desired state.
- Why GPU scheduling requires GPU resources to be advertised by Kubernetes nodes.

## Limitations

- The project is a learning lab, not a production deployment.
- vLLM inference ran separately in Google Colab.
- Kubernetes orchestration was practised locally using NGINX.
- vLLM was not deployed on a GPU-enabled Kubernetes cluster.
- Benchmark measurements were collected from single trials and need repeated testing for stronger conclusions.

## Future Improvements

- Repeat benchmarks and calculate averages and percentiles.
- Compare different concurrency levels and output lengths.
- Collect time-to-first-token and inter-token latency.
- Add benchmark charts and reproducible result files.
- Explore vLLM deployment on a GPU-enabled Kubernetes environment.
- Add monitoring for request throughput, latency, and GPU utilization.

## Repository Contents

```text
llm-inference-vllm-kubernetes-lab/
├── README.md
├── notebooks/
│   └── vllm_inference.ipynb
├── images/
│   └── architecture.png
└── results/
    └── benchmark_results.csv
```

The notebook, architecture image, and CSV should be added when available. The structure above describes the intended repository layout.

## Disclaimer

This repository documents hands-on learning and experimentation. Benchmark results are environment-specific and should not be interpreted as production capacity estimates.

## Author

**Subham Patnaik**

GitHub: [Subham999100](https://github.com/Subham999100)

<!-- .github/copilot-instructions.md - guidance for AI coding agents working on this repo -->
# Copilot instructions — k8s_training-2025

This repository is a collection of Kubernetes training examples (manifests and small demos). The goal of these instructions is to help an AI coding agent be immediately productive editing, extending, and validating Kubernetes YAML and small helper scripts in this repo.

- Repo structure highlights (important directories)
  - `deployment/`, `configmap/`, `volumes/`, `services/`, `mongodb/`, `namespace/` — canonical example manifests (Deployment, Service, ConfigMap, Secrets, PVC/PV).
  - `outros_modos-de-deploy/` — alternative deployment examples (e.g., `nginx/` with `nginx.yaml` and `service.yaml`). Use these for examples of different service types.
  - `kind/` — cluster configuration for kind (Kubernetes in Docker). Useful when suggesting local test steps.
  - `self-healing_probes/`, `lifecycle-pod/`, `replicaset/`, `pilot/` — focused examples for liveness/readiness, lifecycle hooks, ReplicaSet, and single-pod demos.

- Big-picture architecture and intent
  - This repository is not a single microservice application. It's a curated set of independent Kubernetes manifest examples intended for learning and demoing features (scheduling, volumes, service types, namespaces, quotas, HPA). Treat each top-level folder as a standalone demo. Changes should avoid coupling unrelated demos.

- Developer workflows and quick commands
  - Preferred local testing: use `kind` to create a cluster and `kubectl apply -f <path>` to apply specific demo folders. Example: `kubectl apply -f deployment/` or `kubectl apply -f volumes/mongodb/`.
  - When suggesting commands, prefer explicit file paths (not `-f .`) and include namespace where shown in manifests (e.g., `kubectl apply -f namespace/ -n demo` only if manifests target that namespace).
  - If a `kind` cluster is referenced, recommend `kind create cluster --config kind/kind-k8s.yml` then `kubectl cluster-info --context kind-kind`.

- Project-specific conventions and patterns
  - YAML manifests follow plain Kubernetes resources (v1, apps/v1). Keep resource kinds and apiVersion consistent with the examples in the same folder. When creating new manifests, mimic the naming and labels style used (look at `labels_selectors/` for label conventions like `app: <name>` and color examples `blue/green`).
  - Secrets use plain `Secret` manifests (see `mongodb/` and `volumes/mongodb/`); do not hardcode production secrets — use placeholders and note they must be base64-encoded when appropriate.
  - PVC/PV examples live under `volumes/` and sometimes include provider-specific names (e.g., `momgodb-pv.yml` and `momgodb-pvc-scw.yml`). Preserve naming and storageClass patterns when editing.
  - Service types: the repo contains examples of `ClusterIP`, `NodePort`, `LoadBalancer`, and `ExternalName` under `services/` and `outros_modos-de-deploy/nginx/`. When proposing a Service change, pick the service type consistent with the demo intent.

- Integration points and assumptions
  - There are no application source code files here; integrations are purely Kubernetes-level (kubectl, kind). Don't add external service hooks (CI, registries) unless asked.
  - Some manifests reference specific namespaces. Confirm namespace presence before suggesting kubectl apply commands that target them.

- Editing rules for AI agents (specific and actionable)
  1. Limit the scope of changes to one demo folder per PR/commit. This repo is an educational catalog; keep diffs small and focused.
 2. Preserve existing labels and selectors; update both Deployment and Service selectors in sync. Refer to `labels_selectors/` for examples.
 3. When adding fields (e.g., resources, probes), follow the style used in `gerenciamneto_de_recusrsos_da_sua_apliacacao_e_dos_ambientes/hpa/` and `self-healing_probes/` examples.
 4. For secrets and sensitive values, use placeholders and document how to encode or create them via kubectl (e.g., `kubectl create secret generic mongo-secret --from-literal=...`).
 5. If you change an apiVersion or kind, make sure the rest of the manifest is compatible (fields names, spec structure).

- Examples pulled from the repo
  - Update a Deployment's probes: follow patterns in `self-healing_probes/api/deployment.yml` for liveness/readiness structure.
  - Create a Service matching a Deployment: see `services/service-nodeport.yml` and `outros_modos-deploy/nginx/service.yaml` for naming and selector patterns.

- What not to do
  - Do not introduce runtime application code or Dockerfiles—this repo contains only Kubernetes manifests and examples.
  - Avoid broad, repository-wide changes (renaming labels across all demos) without explicit user instructions.

- If parts are ambiguous, ask the user these quick questions
  - Which demo(s) should be changed together? (keep PR scope small)
  - Should changes be validated against `kind`? (I can provide cluster commands)

Keep suggestions short and local — propose a patch to one manifest at a time and reference the exact file path in your suggested edits.

# Day 4 — Declarative Kubernetes & Infrastructure as Code

This lesson marks the point where Kubernetes stops being a collection of commands and starts behaving like a **control system**.

Days 1–3 were about *what Kubernetes does*.

**Day 4 is about who is in charge.**

From this point on, you stop issuing instructions and start declaring reality.

---

## Why `kubectl` Commands Don’t Scale

Early on, it’s tempting to rely on commands like:

```bash
kubectl create deployment my-app --image=nginx --replicas=3
kubectl expose deployment my-app --port=80
```

They work, but they come with serious problems:

* No permanent record of what you did
* No way to review changes before applying
* No version control
* Hard to reproduce environments
* No audit trail

Imperative commands describe *actions*.
Production systems need **intent**.

---

## Infrastructure as Code (IaC)

Kubernetes is designed to be managed through **YAML manifests**.

Instead of telling the cluster *how* to do something, you describe *what the final state should be*.

That desired state is written as code, committed to Git, reviewed, versioned, and reproducible.

This is the foundation of **GitOps**:

* Git is the source of truth
* Cluster state converges toward what Git declares
* Changes are visible, reviewable, and reversible

---

## Declarative vs Imperative (The Mental Shift)

**Imperative**:

> “Create three pods now.”

**Declarative**:

> “There should always be three pods.”

Once declared, Kubernetes takes responsibility for enforcing that reality.

This is not a convenience feature. It is a control philosophy.

---

## The Universal Kubernetes YAML Structure

Every Kubernetes resource follows the same top-level structure:

```yaml
apiVersion:
kind:
metadata:
spec:
```

This consistency is intentional.

---

### `apiVersion`

Specifies which Kubernetes API group and version the resource belongs to.

Examples:

* `v1` → core resources (Pods, Services)
* `apps/v1` → Deployments, StatefulSets
* `networking.k8s.io/v1` → Ingress, NetworkPolicy

Different API versions imply different behavior, guarantees, and constraints.

---

### `kind`

Defines **what type of object** you are creating.

Examples:

* Deployment
* Service
* Pod
* ConfigMap

Changing the kind changes lifecycle behavior and blast radius.

---

### `metadata`

Defines identity and classification:

* `name` — unique within a namespace
* `labels` — used for selection and organization
* `annotations` — non-selective metadata
* `namespace` — scope of the resource

Metadata becomes critical for grouping, routing, and policy enforcement later.

---

### `spec`

The desired state.

Everything under `spec` is what Kubernetes will continuously attempt to enforce.

Declarative systems do not forget. If the spec is wrong, the system will faithfully maintain the wrong state.

---

## Writing Your First Deployment Manifest

Create `deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
  labels:
    app: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: nginx
        image: nginx:1.25
        ports:
        - containerPort: 80
```

Apply it:

```bash
kubectl apply -f deployment.yaml
```

Check pods:

```bash
kubectl get pods
```

---

## `kubectl apply` and Idempotency

Running the same command repeatedly:

```bash
kubectl apply -f deployment.yaml
```

Produces the same result if the desired state hasn’t changed.

This property is called **idempotency** and it enables:

* Safe retries
* Automation
* CI/CD pipelines
* GitOps workflows

Change the file:

```yaml
replicas: 5
```

Apply again. Kubernetes detects the difference and reconciles the cluster.

You didn’t scale manually. You declared a new reality.

---

## Multiple Resources in One File

YAML allows multiple resources separated by `---`.

```yaml
apiVersion: apps/v1
kind: Deployment
...
---
apiVersion: v1
kind: Service
...
```

This allows you to deploy tightly related resources together:

* Application
* Networking
* Configuration

It reduces partial deployments and misaligned state.

---

## Generating YAML with `--dry-run`

Kubernetes can generate manifests for you:

```bash
kubectl create deployment test \
  --image=nginx \
  --replicas=2 \
  --dry-run=client -o yaml
```

Redirect output to a file:

```bash
kubectl create deployment test --image=nginx --dry-run=client -o yaml > test.yaml
```

This is the fastest way to:

* Learn resource structure
* Avoid syntax errors
* Inspect defaults

---

## Inspecting Live Resources as YAML

To see how Kubernetes actually stores a resource:

```bash
kubectl get deployment my-app -o yaml
```

This shows:

* Defaults added by Kubernetes
* Controller-managed fields
* Full configuration surface

Reading live YAML is a critical debugging skill.

---

## Deleting Declaratively

Just like creation, deletion can be declarative:

```bash
kubectl delete -f app.yaml
```

Everything defined in the file is removed together.

---

## Security Perspective (Why This Lesson Matters)

Declarative configuration changes the security model:

* Git becomes an audit log
* Changes are reviewable
* Infrastructure state is reproducible
* Manual drift is eliminated

However, mistakes in declarative systems persist until corrected.

Declarative control amplifies both correctness and error.

This is why YAML deserves the same scrutiny as application code.

---

## Key Takeaways

* Imperative commands are for learning, not production
* YAML manifests define desired state
* `kubectl apply` reconciles reality
* Kubernetes resources follow a consistent structure
* Infrastructure as Code enables GitOps
* Declarative systems enforce intent continuously

Day 4 is the transition from *using Kubernetes* to *governing Kubernetes*.

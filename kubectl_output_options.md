
# Kubernetes `kubectl -o` Output Options (Complete List)

In Kubernetes, the `-o` flag controls how output is displayed. It’s useful for scripting, automation, debugging, and working with manifests.

---

## 1. `-o wide`
Displays extra information in table form such as internal IP, node name, and OS image.

```bash
kubectl get pods -o wide
```

**Use:** When you need more details than the default view (e.g., node info, IPs).

---

## 2. `-o json`
Outputs the full object in raw JSON format.

```bash
kubectl get pods -o json
```

**Use:** For scripting, automation, or inspecting full resource specs.

---

## 3. `-o yaml`
Outputs the full object in YAML format.

```bash
kubectl get service my-service -o yaml
```

**Use:** When you want to export, edit, or reuse a manifest.

---

## 4. `-o jsonpath='<expression>'`
Extracts specific fields using JSONPath syntax.

```bash
kubectl get pods -o jsonpath='{.items[*].metadata.name}'
```

**Use:** To script or filter specific values from resources.

---

## 5. JSONPath Example: Get Kubernetes version per node

```bash
kubectl get nodes -o jsonpath="{range .items[*]}{.metadata.name}: {.status.nodeInfo.kubeletVersion}{'\n'}{end}"
```

**Use:** To check the Kubelet version running on each node.

---

## 6. `-o go-template='<template>'` or `--go-template-file=<file>`
Use Go templating for advanced and structured output.

```bash
kubectl get pods -o go-template='{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}'
```

**Use:** When you need complex or custom formatting logic.

---

## 7. `-o name`
Displays only the resource names with kind prefix (e.g., `pod/my-pod`).

```bash
kubectl get pods -o name
```

**Use:** For piping or scripting with resource names.

---

## 8. `-o custom-columns=<spec>`
Displays selected fields as columns in table format.

```bash
kubectl get pods -o custom-columns="NAME:.metadata.name,STATUS:.status.phase"
```

**Use:** When you want readable, filtered tabular output.

---

## 9. `-o custom-columns-file=<file>`
Same as above, but loads the column definition from a file.

**Use:** When working with large or reusable column specs.

---

## 10. `-o server-print`
The default behavior of `kubectl get`, showing standard human-readable tables.

**Use:** You normally don’t need to specify this—it's the default unless overridden.

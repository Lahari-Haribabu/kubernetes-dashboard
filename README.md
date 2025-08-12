---

````markdown
# Kubernetes Dashboard Setup

This guide explains how to set up and access the Kubernetes Dashboard with an **admin user**.

---

## 1. Deploy the Kubernetes Dashboard

Apply the official Kubernetes Dashboard manifest:

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
````

---

## 2. Create an Admin User

Create a file named **`dashboard-admin-user.yml`** with the following content:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
```

Apply the configuration:

```bash
kubectl apply -f dashboard-admin-user.yml
```

---

## 3. Retrieve the Access Token

Run the following command to get the **login token**:

```bash
kubectl -n kubernetes-dashboard create token admin-user
```

Copy the token — you will use it to log into the dashboard.

---

## 4. Access the Kubernetes Dashboard

Start the Kubernetes proxy:

```bash
kubectl proxy
```

Open your browser and go to:

```
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
```

---

## 5. Login

1. Select **Token** authentication.
2. Paste the token retrieved in step 3.
3. Click **Sign In**.

---

## 📌 Notes

* The `admin-user` has full cluster-admin privileges. For production, consider creating roles with **least privilege**.
* The dashboard is accessible only via `kubectl proxy` by default. For external access, configure an Ingress or NodePort service (ensure proper authentication).

---

## 📚 References

* [Kubernetes Dashboard Documentation](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)
* [Kubernetes Dashboard GitHub](https://github.com/kubernetes/dashboard)

```


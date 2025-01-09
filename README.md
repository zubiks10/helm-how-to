# helm-how-to

### What is Helm?

Helm is a **package manager for Kubernetes** that simplifies the deployment and management of applications on Kubernetes clusters. It enables users to define, install, and manage Kubernetes applications in a consistent and repeatable way. Helm uses **charts**, which are pre-configured templates of Kubernetes resources.

### Key Concepts in Helm

1. **Chart**:
   - A Helm chart is a package that contains Kubernetes YAML manifests (deployments, services, etc.), along with templates and metadata files.
   - Charts can be shared and versioned, making them reusable across projects.

2. **Release**:
   - A release is an instance of a Helm chart running in a Kubernetes cluster. You can deploy the same chart multiple times with different configurations.

3. **Values**:
   - Values are configuration settings that can override the default settings in a chart, allowing customization.

4. **Repositories**:
   - Helm charts are stored in repositories, which are collections of Helm charts.

5. **Commands**:
   - `helm install`: Installs a chart into the Kubernetes cluster as a release.
   - `helm upgrade`: Updates an existing release with a new version of the chart or configuration.
   - `helm uninstall`: Deletes a release from the cluster.
   - `helm repo add`: Adds a Helm chart repository.

---

### Example 1: Deploying NGINX using Helm

1. **Install Helm** (if not already installed):
   ```bash
   curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
   ```

2. **Add a Chart Repository**:
   Helm uses repositories to fetch charts.
   ```bash
   helm repo add bitnami https://charts.bitnami.com/bitnami
   helm repo update
   ```

3. **Install an NGINX Chart**:
   Deploy the NGINX server using the Bitnami repository:
   ```bash
   helm install my-nginx bitnami/nginx
   ```

   - `my-nginx` is the release name.
   - `bitnami/nginx` is the chart name.

4. **Verify the Installation**:
   ```bash
   helm list
   kubectl get all
   ```

5. **Customize the Installation**:
   Create a `values.yaml` file to customize the NGINX configuration:
   ```yaml
   service:
     type: NodePort
     port: 8080
   ```

   Install the chart with custom values:
   ```bash
   helm install my-nginx bitnami/nginx -f values.yaml
   ```

---

### Example 2: Creating a Custom Helm Chart

1. **Create a New Chart**:
   ```bash
   helm create my-chart
   cd my-chart
   ```

   This creates the following structure:
   ```
   my-chart/
   ├── charts/
   ├── templates/
   │   ├── deployment.yaml
   │   ├── service.yaml
   │   └── _helpers.tpl
   ├── values.yaml
   └── Chart.yaml
   ```

2. **Modify the Template Files**:
   Edit `templates/deployment.yaml` to define the Kubernetes deployment resource:
   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: {{ .Values.app.name }}
   spec:
     replicas: {{ .Values.app.replicas }}
     selector:
       matchLabels:
         app: {{ .Values.app.name }}
     template:
       metadata:
         labels:
           app: {{ .Values.app.name }}
       spec:
         containers:
         - name: {{ .Values.app.name }}
           image: {{ .Values.app.image }}
           ports:
           - containerPort: 80
   ```

3. **Define Default Values in `values.yaml`**:
   ```yaml
   app:
     name: my-app
     replicas: 2
     image: nginx:latest
   ```

4. **Deploy the Chart**:
   ```bash
   helm install my-release ./my-chart
   ```

5. **Upgrade the Chart**:
   Update the `values.yaml` or templates, then upgrade:
   ```bash
   helm upgrade my-release ./my-chart
   ```

---

### Advantages of Helm

1. **Efficiency**: Streamlines deployment and updates for complex applications.
2. **Repeatability**: Allows you to easily replicate deployments across environments.
3. **Versioning**: Charts and releases are versioned for better control.
4. **Customization**: Offers flexibility through values and templates.

---

### Additional Resources

- [Helm Official Documentation](https://helm.sh/docs/)
- Popular Helm chart repositories:
  - [Bitnami](https://bitnami.com/)
  - [ArtifactHub](https://artifacthub.io/)

If you need specific examples or help with creating or deploying Helm charts, let me know!
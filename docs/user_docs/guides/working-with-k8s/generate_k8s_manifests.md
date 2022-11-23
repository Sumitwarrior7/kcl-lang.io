# KCL - Make Kubernetes Resource Management Easier

## What is KCL

[KCL (Kusion Configuration Language)](https://github.com/KusionStack/KCLVM) is an open source constraint-based record and functional language. KCL improves the writing of a large number of complex configurations through mature programming language technology and practice, and is committed to building better modularity, scalability and stability around configuration, simpler logic writing, fast automation and good ecological extensionality.

When we deploy software systems, we do not think they are fixed. Evolving business requirements, infrastructure requirements, and other factors mean that systems are constantly changing. When we need to change the system behavior quickly, and the change process needs expensive and lengthy reconstruction and redeployment process, business code change is often not enough. Configuration can provide us with a low overhead way to change system functions. For example, we often write JSON or YAML files as shown below for our system configuration.

+ JSON configuration

```json
{
    "server": {
        "addr": "127.0.0.1",
        "listen": 4545
    },
    "database": {
        "enabled": true,
        "ports": [
            8000,
            8001,
            8002
        ],
    }
}
```

+ YAML configuration

```yaml
server:
  addr: 127.0.0.1
  listen: 4545
database:
  enabled: true
  ports:
  - 8000
  - 8001
  - 8002
```

We can choose to store the static configuration in JSON and YAML files as needed. In addition, the configuration can also be stored in a high-level language that allows more flexible configuration, which can be coded, rendered, and statically configured. KCL is such a configuration language. We can write KCL code to generate JSON/YAML and other configurations. In this article, we focus on the use of KCL to generate and manage Kubernetes resources, and give you a simple and quick start through some simple examples. We will expand more in the following articles.

## Why use KCL

When we manage the Kubernetes resources, we often maintain it by hand, or use Helm and Kustomize tools to maintain our YAML configurations or configuration templates, and then apply the resources to the cluster through kubectl tools. However, as a "YAML engineer", maintaining YAML configuration every day is undoubtedly trivial and boring, and prone to errors. For example as follows:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata: ... # Omit
spec:
  selector:
    matchlabels:
      cell: RZ00A
  replicas: 2
  template:
    metadata: ... # Omit
    spec:
      tolerations:
      - effect: NoSchedules
        key: is-over-quota
        operator: Equal
        value: 'true'
      containers:
      - name: test-app
          image: images.example/app:v1 # Wrong ident
        resources:
          limits:
            cpu: 2 # Wrong type. The type of cpu should be str
            memory: 4Gi
            # Field missing: ephemeral-storage
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: is-over-quota
                operator: In
                values:
                - 'true'
```

+ The structured data in YAML is untyped and lacks validation methods, so the validity of all data cannot be checked immediately.
+ YAML has poor programming ability. It is easy to write incorrect indents and has no common code organization methods such as logical judgment. It is easy to write a large number of repeated configurations and difficult to maintain.
+ The design of Kubernetes is complex, and it is difficult for users to understand all the details, such as the `toleration` and `affinity` fields in the above configuration. If users do not understand the scheduling logic, it may be wrongly omitted or superfluous added.

Therefore, KCL expects to solve the following problems in Kubernetes YAML resource management:

+ Use **production level high-performance programming language** to **write code** to improve the flexibility of configuration, such as conditional statements, loops, functions, package management and other features to improve the ability of configuration reuse.
+ Improve the ability of **configuration semantic verification** at the code level, such as optional/required fields, types, ranges, and other configuration checks.
+ Provide **the ability to write, combine and abstract configuration blocks**, such as structure definition, structure inheritance, constraint definition, etc.

## How to use KCL to generate and manage Kubernetes resources

### Prerequisite

First, you can visit the [KCL project home page](https://github.com/KusionStack/KCLVM) to download and install KCL according to the instructions, and then prepare a [Kubernetes](https://kubernetes.io/) environment.

### Generate Kubernetes manifests

我们可以编写如下 KCL 代码并命名为 main.k ，KCL 受 Python 启发，基础语法十分接近 Python, 比较容易学习和上手, 配置模式写法很简单，`k [: T] = v`, 其中 `k` 表示配置的属性名称; `v` 表示配置的属性值; `: T` 表示一个可选的类型注解。

```python
apiVersion = "apps/v1"
kind = "Deployment"
metadata = {
    name = "nginx"
    labels.app = "nginx"
}
spec = {
    replicas = 3
    selector.matchLabels = metadata.labels
    template.metadata.labels = metadata.labels
    template.spec.containers = [
        {
            name = metadata.name
            image = "${metadata.name}:1.14.2"
            ports = [{ containerPort = 80 }]
        }
    ]
}
```

In the above KCL code, we declare the `apiVersion`, `kind`, `metadata`, `spec` and other variables of a Kubernetes `Deployment` resource, and assign the corresponding contents respectively. In particular, we will assign `metadata.labels` fields are reused in `spec.selector.matchLabels` and `spec.template.metadata.labels` field. It can be seen that, compared with YAML, the data structure defined by KCL is more compact, and configuration reuse can be realized by defining local variables.

We can get a Kubernetes YAML file by executing the following command line

```cmd
kcl main.k
```

The output is

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

Of course, we can use KCL together with kubectl and other tools. Let's execute the following commands and see the result:

```cmd
$ kcl main.k | kubectl apply -f -

deployment.apps/nginx-deployment configured
```

It can be seen from the command line that it is completely consistent with the deployment experience of using YAML configuration and kubectl application directly.

Check the deployment status through kubectl

```cmd
$ kubectl get deploy

NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           15s
```

### Write code to manage Kubernetes resources

When publishing Kubernetes resources, we often encounter scenarios where configuration parameters need to be dynamically specified. For example, different environments need to set different `image` field values to generate resources in different environments. For this scenario, we can dynamically receive external parameters through KCL conditional statements and `option` functions. Based on the above example, we can adjust the configuration parameters according to different environments. For example, for the following code, we wrote a conditional statement and entered a dynamic parameter named `env`.

```python
apiVersion = "apps/v1"
kind = "Deployment"
metadata = {
    name = "nginx"
    labels.app = "nginx"
}
spec = {
    replicas = 3
    selector.matchLabels = metadata.labels
    template.metadata.labels = metadata.labels
    template.spec.containers = [
        {
            name = metadata.name
            image = "${metadata.name}:1.14.2" if option("env") == "prod" else "${metadata.name}:latest"
            ports = [{ containerPort = 80 }]
        }
    ]
}
```

Use the KCL command line `-D` flag to receive an external dynamic parameter:

```cmd
kcl main.k -D env=prod
```

The output is

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

上述代码片段中的 `image = metadata.name + ":1.14.2" if option("env") == "prod" else  metadata.name + ":latest"` 意思为：当动态参数 `env` 的值被设置为 `prod` 时，image 字段值为 `nginx:1.14.2`, 否则为 `nginx:latest`，因此我们可以根据需要为 env 设置为不同的值获得不同内容的 Kubernetes 资源。

The `image=metadata.name+": 1.14.2" if option ("env")=="prod" else metadata.name + ": latest"` in the above code snippet means that when the value of the dynamic parameter `env` is set to `prod`, the value of the image field is `nginx: 1.14.2`; otherwise, it is' nginx: latest'. Therefore, we can set env to different values as required to obtain Kubernetes resources with different contents.

KCL also supports maintaining the dynamic parameters of the option function in the configuration file, such as writing the ` kcl.yaml ` file.

```yaml
kcl_options:
  - key: env
    value: prod
```

The same YAML output can be obtained by using the following command line to simplify the input process of KCL dynamic parameters.

```cmd
kcl main.k -Y kcl.yaml
```

The output is:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

## Next

This article briefly introduces the quick start of writing configurations with KCL and the use of KCL to define and manage Kubernetes resources.

At this stage, Kustomize and Helm have gradually evolved into the de facto standard in the field of Kubernetes configuration definition and management. Small partners familiar with Kubernetes may prefer to write explicit configurations. What are the differences and similarities between Kustomize and Helm in using KCL to write and render configuration files? Considering that many partners are already using tools like Helm and Kustomize, I will introduce the KCL method to write the corresponding configuration code in the next article.

For more highlights, please visit:

+ KCL: https://github.com/KusionStack/KCLVM
+ Kusion: https://github.com/KusionStack/kusion
+ Konfig: https://github.com/KusionStack/Konfig

Welcome to join our community for communication:

+ https://github.com/KusionStack/community
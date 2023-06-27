allow_k8s_contexts('eks.cdd-gloudgate.us-east-2.cdd-demo-aws')

LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='default')

k8s_custom_deploy(
    'spring-sensors',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --live-update" +
               " --local-path " + LOCAL_PATH +
               " --namespace " + NAMESPACE +
               " --label inner-loop=true " +
               " --service-ref db- " +
               " --yes >/dev/null" +
               " && kubectl get workload spring-sensors --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=['pom.xml', './target/classes', 'config/workload.yaml'],
    container_selector='workload',
    live_update=[
      sync('./target/classes', '/workspace/BOOT-INF/classes')
    ]
)

k8s_resource('spring-sensors', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'serving.knative.dev/service': 'spring-sensors'}])

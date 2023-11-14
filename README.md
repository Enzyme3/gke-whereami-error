# gke-whereami-error

`gke-whereami-error` is a fork of [gke-whereami](https://github.com/theemadnes/gke-whereami)

The only difference is the ability to pass an environment variable to force the application to throw HTTP status code 500 errors



```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: whereami
spec:
  replicas: 3 #whereami can be deployed as multiple identical replicas
  selector:
    matchLabels:
      app: whereami
  template:
    metadata:
      labels:
        app: whereami
    spec:
      containers:
      - name: whereami
        image: <image-location>
        ports:
          - name: http
            containerPort: 8080 #The application is listening on port 8080
        livenessProbe: #There is a health probe listening on port 8080/healthz that will respond with 200 if the application is running
          httpGet:
              path: /healthz
              port: 8080
              scheme: HTTP
          initialDelaySeconds: 5
          periodSeconds: 15
          timeoutSeconds: 5
        readinessProbe:
          httpGet:
            path: /healthz
            port: 8080
            scheme: HTTP
          initialDelaySeconds: 5
          timeoutSeconds: 1
        env:
          - name: FORCE_FAIL # if set to TRUE, causes app to return 5XX every minute
            value: "FALSE"
          - name: FORCE_FAIL_HEALTH # if set to TRUE, causes app healthcheck to return 5XX every minute
            value: "FALSE"
```

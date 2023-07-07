# argocd-implementation

1.	https://github.com/argoproj/argo-cd             https://artifacthub.io/packages/helm/argo/argo-cd
searched on internet and got.
2.	Run on VSCode: 
-	helm repo list
-	helm repo add argo https://argoproj.github.io/argo-helm
-	helm search repo argo-cd                output  -  argo/argo-cd    5.37.1
-	helm fetch --untar argo/argo-cd       (download helm chart for argocd)
-	create custom.yaml with content:
-	global:
-	    deploymentStrategy: 
-	      type: RollingUpdate
-	      rollingUpdate:
-	        maxSurge: 25%
-	        maxUnavailable: 25%
-	helm upgrade --install argo-cd --namespace=dev --values custom.yaml ./argo-cd                                 (upgrade the argo-cd with custom.yaml content)
3.	Now in order to access to own argocd website, I need to do following steps:
-	needs to add custom.yaml ingress
-	service:
-	  ingress:
-	    enabled: true
-	    annotations: 
-	      nginx.ingress.kubernetes.io/force-ssl-redirect: "true"
-	      nginx.ingress.kubernetes.io/backend-protocols: "HTTPS"
-	      nginx.ingress.kubernetes.io/ssl-passthrough: "true"
-	      kubernetes.io/ingress.class: nginx
-	      cert-manager.io/cluster-issuer: letsencrypt-prod
-	    hosts:
-	    - 'argocd.YOUROWNDOMAIN'
-	    paths:
-	    - '/'
-	    tls:
-	    - secretName: 'argocd-tls'
-	      hosts:
-	      - 'argocd.YOUROWNDOMAIN'
-	    labels: {}
-	    ingressClassName: ""
4.	Now you need to sign into argocd.yourname website:
Run this command: kubectl –n dev get secret argocd-initial-admin-secret –o jsonpath=”{.data.password}”  | base64 –d
And it gives you initial password, user name: admin

